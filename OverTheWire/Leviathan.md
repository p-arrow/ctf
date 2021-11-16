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
    - In the upper case we have access to file. The program asks for our effective uid twice (`geteuid()`) and sets the real uid (`setreuid(12002, 12002)`
    - In the lower case we do not get access

![grafik](https://user-images.githubusercontent.com/84674087/142065462-2d606c01-b0fa-4c45-8af3-d929abc845fb.png)
