**>> Details: [https://overthewire.org/wargames/leviathan/](https://overthewire.org/wargames/leviathan/)**

## Leviathan0
- `ls` reveals nothing but `ls -la` do: **.backup**
- We enter the backup and find **bookmarks.html**
- After opening the file we see a lot of content, so we specify the search: `cat bookmarks.html | grep pass`
- Output: 

```
<DT><A HREF="http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, 
the password for leviathan1 is rioGegei8m" ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A>
```
- **PASSWORD:** rioGegei8m

## Leviathan1
- The directory of leviathan1 contains an ELF file called **check**
- `strings -n 6 check` does not reveal specific information
- We start `ltrace ./check` and see below hint:

![grafik](https://user-images.githubusercontent.com/84674087/142062887-0ab6516d-2a17-491d-9b98-8f2fcb62cf2c.png)

- While performing the program we entered the password `hello` and failed
- During execution the program do `strcpy` and compares the first three letters with the given string `sex`
- Then, most probably the password is **sex** - Success!
- We escalate our privilege and turn into leviathan2 (check with `whoami`)
- Consequently we go to `cd /etc/leviathan_pass` and do `cat leviathan2`
- **PASSWORD:** ougahZi8Ta

## Leviathan2
- We find the file **printfile** which works as: `Usage: ./printfile filename`
- Immediately we try: `./printfile /etc/leviathan_pass/leviathan3` but we we get this output: **You cant have that file...**
- It seems we have to escalate our user rights
- Below you can see how printfile works: `ltrace ./printfile /etc/leviathan_pass/leviathan3`
    - We get access granted to the file we specified
    - Our effective uid is retrieved twice (`geteuid()`)
    - printfile then sets the effective uid (`setreuid(12002, 12002)`)
    - A system call performs `/bin/cat` upon the file we specified

![grafik](https://user-images.githubusercontent.com/84674087/142065462-2d606c01-b0fa-4c45-8af3-d929abc845fb.png)

- A) We could try to modify the running program using gdb
- B) We find a way to bypass the access and getuid controls until we reach the system call
- The option B) can be achieved by using a trick: We create a file that contains whitespace in its name, e.g. "file 2.txt"
- printfile will split up the system call into two parts: "file" + " 2"
- Proof of concept: `printfile /tmp/test/"file 2".txt`

![grafik](https://user-images.githubusercontent.com/84674087/142075688-5b3d4b3b-5c0c-4bd0-82ec-c08aebba25d5.png)

- In this way, printfile gets through the whole code and performs the system call
- We just have to find a way to let it read the file that contains the password for Leviathan3
- To accomplish this task we utilize a symbolic link: `ln -s /etc/leviathan_pass/leviathan3 /tmp/test/file.txt`
- `./printfile /tmp/test/"file 2".txt` will then try to read out `/tmp/test/file.txt` and `/tmp/test/ 2.txt`, wherein the latter one does not exist and the former one leads to the password 
- **PASSWORD:** Ahdiemoo1j

<br />

## Leviathan3
- The directory contains the file **level3**
- Let's start it attached to ltrace: `ltrace ./level3`
- The output:

![grafik](https://user-images.githubusercontent.com/84674087/142216555-6ace1658-5873-4b18-bffc-c8d7f4b6c7e8.png)

- The program compares the password we entered: `strcmp("hit\n", "snlprintf\n")`
- We run level3 once again and enter snlprintf é voila: We got the Shell !
- Consequently we hit `cat /etc/leviathan_pass/leviathan4` and obtain the password
- **PASSWORD:** vuH0coox6m

<br />

<br />

## Leviathan4
- We enter `ls -la` and find **.trash**, where the binary **bin** exists
- The program outputs a binary code: `01010100011010010111010001101000001101000110001101101111011010110110010101101001`
- Transformed into ASCII reveals the **PASSWORD**: Tith4cokei
- How to transform?
    - A) Online Converter
    - B) Perl: `./bin | perl -lape '$_=pack"(B8)*",@F'`
        - `-e` expression evaluate the given expression as perl code
        - `-p` sed mode. The expression is evaluated for each line of input 
        - `-l` sed mode. Instead of the full line, only the content of the line (without the line delimiter)
        - `-pack` Convert binary to ASCII. See `perldoc -f pack` for details
        - `"(B8)*"` extract 8 bits at a time 
<br />


## Leviathan5
- The story:

![grafik](https://user-images.githubusercontent.com/84674087/142221088-366fa39d-19c5-4ac4-bafc-b3725ff6e23e.png)

- If we create the required file, fill some random chars into it and start leviathan5 then the following happens:

![grafik](https://user-images.githubusercontent.com/84674087/142230845-6b58c57d-63f5-40ad-82fc-1d66a3550ddb.png)

- Each char is read and displayed: `fgetc` = file get char
- We need to find a way to access `/etc/leviathan_pass/leviathan6` through leviathan5 in order to get the password disclosed
- The first thought is a symbolic link: `ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log`
- Apparently it is a good thought ;) We run `./leviathan5` and succeed
- **PASSWORD:** UgaoFee4li

<br />

## Leviathan6
- The story:

![grafik](https://user-images.githubusercontent.com/84674087/142235623-2753577e-aaaf-4f69-b7d6-179256c78012.png)

- A four digit code is simple to brute force
- We just have to write a small script to automate the process

Option A)
```
#!/bin/bash

i=1
while [ $i -le 10000 ]
do
    echo "$i" 
    ~/leviathan6 $i 
    ((i++))
    sleep 0.01
done
```

Option B)
```
#!/bin/bash

for i in {0..9999}
do
    echo "$i"
    ~/leviathan6 $i
done
```

- Once we find the correct pin (7123) we get the shell:

![grafik](https://user-images.githubusercontent.com/84674087/142388063-37207317-3fdb-4a05-9c35-82e608f81dfc.png)

- **PASSWORD:** ahy7MaeBo9
