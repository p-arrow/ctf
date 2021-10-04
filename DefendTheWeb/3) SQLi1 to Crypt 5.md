## SQLi 1 / SQLi

![grafik](https://user-images.githubusercontent.com/84674087/134203849-9545248b-3071-4645-8aef-0f189a4da224.png)

#### Solution
- We enter `'` inside the username field and get below query information:

![grafik](https://user-images.githubusercontent.com/84674087/134769313-b4d6bdeb-09ee-43f9-95cb-46e4f0d757ac.png)

- We utilize this hint by entering a typical SQL injection inside the username field: `' or '1'='1`
- We get "Invalid login details"

![grafik](https://user-images.githubusercontent.com/84674087/134204439-0004d1d9-f866-4852-82b6-9d66e4c49936.png)

- Then we enter the same SQL injection in both fields: Success !

<br />

## Crypt 3 / Crypt

![grafik](https://user-images.githubusercontent.com/84674087/134204791-8e61621c-2e87-4be6-a884-622ca821e5de.png)

#### Solution

- This code reminds me on morse code ... 
- [https://morsedecoder.com/](https://morsedecoder.com/)

![grafik](https://user-images.githubusercontent.com/84674087/134205193-7f9e86c3-e681-4961-a5ce-37f7b57b78df.png)

- password: **THANKYOUSIR**

<br />

## Intro 11 / Javascript

![grafik](https://user-images.githubusercontent.com/84674087/134205511-9f05aded-8d0c-4dc6-bb4c-7f6c6c7f2056.png)

#### Solution

- Puhhh ! Nothing special to find in the sourcecode. No hidden password, no suspicious JS script ... 
- After a while, we recognize the search parameter **?input** in the URL: https://defendtheweb.net/playground/intro11/?input
- We play around and enter specific values: `?input=123` , `?input=kgjgfkjhkjhk` etc.
- Nothing happens
- When we remove the parameter and reload the page, the parameter simply returns
- Additionally, whatever we enter after "?input", it remains in the URL even after reloading the page
- DevTool: While having the Debugger open, we notice that `?input + any other content` like `?input==` prevents the loading of the playground:

![grafik](https://user-images.githubusercontent.com/84674087/134385289-a62d1850-aad1-4e78-8f4b-14fe1206fa64.png)

- As soon as we remove one char from `?input`, as `?in`, or remove it completely the playground gets loaded:

![grafik](https://user-images.githubusercontent.com/84674087/134385391-b0e6d445-4f8e-49f5-a486-9bb0879a1f9c.png)

- Moreover, one line is added in the sourcecode on line 412
- This can only be seen with the Debugger. The original sourcecode (`Control + U`) does not show modifications of DOM caused by JavaScript

![grafik](https://user-images.githubusercontent.com/84674087/134383707-c64e9aca-a71e-470b-8f96-b6e1839cb89e.png)

- That is, some thoughts later we experimentally replace `intro11?input` by `intro11#`
- The # pretends that everything followed after that is NOT sent to the server (as if we remove `?input` entirely)
- We send  # and check the sourcecode with Debugger et voilà - look at line 473 !

![grafik](https://user-images.githubusercontent.com/84674087/134394228-d17b906d-08f2-4da7-8991-825c7e3d3dc1.png)

- Password: **69a6681821**

<br />

## Recon

![grafik](https://user-images.githubusercontent.com/84674087/134402400-800d26ed-da93-44b5-b78f-fbd35099a71a.png)

#### Solution
- The IP address can be found for instance with WebDevTool: `Control + Shift + E`
- **3.10.42.19**

![grafik](https://user-images.githubusercontent.com/84674087/134406650-b73f1a77-c712-442b-81cb-5cf85232e6d3.png)

- The hosting company can be disclosed like this: `host 3.10.42.19`
- It is **Amazon** (AWS)

![grafik](https://user-images.githubusercontent.com/84674087/134406913-41aad135-2d0e-4634-a7c1-c3b119b8da0d.png)

- The B6-Key Header ... never heard about B6? Me neither
- After a quick search on Google we learn that this header is related to email headers. Actually, most Defendtheweb forum posts state that
- Everbody says: Look at the email!! The email you receive(d) from Defendtheweb ...and this is where I lost at least one hour because these advices are outdated not valid anymore
- Luckily, I figured out that everything we need is our WebDevTool: `Control + Shift + E`

![grafik](https://user-images.githubusercontent.com/84674087/134650671-b2e228a2-c7fd-4c6f-bb66-eb6ae0c103f4.png)

- B6-key: **goobles**


<br />

## Crypt 4 / Crypt

![grafik](https://user-images.githubusercontent.com/84674087/134478883-974b26f8-b744-4900-b23e-748e53ff8876.png)

#### Solution
```
 Dc, gdcl cl h lcrcshn ckqh gz sqwqs guz. Gdcl gcrq qhyd sqggqn cl hllcomqk h ljqycacqk nqshgczmldcj ucgd hmzgdqn sqggqn. Jhll: cdhwqancqmkl
```

- We use this handy frequency analysis tool: [https://math.dartmouth.edu/](https://math.dartmouth.edu/~awilson/tools/frequency_analysis.html)
- After playing around with the letters we produce below result:

```
 hi, this is a similar idea to level two. this time each letter is assigned a specified relationship with another letter. pass: ihavefriends
```

- password: **ihavefriends**

<br />

## Map it

![grafik](https://user-images.githubusercontent.com/84674087/134675088-1d0af628-5bbc-49ab-aeb4-9cef9e7738f7.png)

#### Solution
- "Map it" ... nmap it?
- Let's check out all ports: `nmap -p- -sV 3.10.42.19`

![grafik](https://user-images.githubusercontent.com/84674087/134685817-dfd2f49f-7b6b-4d14-814e-da8cbb12011f.png)

- The option `-F` speeds up the nmap scan and investigates only the most common 100 ports. However, when I use `nmap -F 3.10.42.19` I get following output:

![grafik](https://user-images.githubusercontent.com/84674087/135122900-a12061c6-5991-443c-8967-c6d03fe1bfdf.png)

- So, I find other ports, which I did not discover when scanning all ports. Mysterious ...
- I figured out - after reading some DtW forum posts - that several nmap scans seem necessary to reveal most/all services running
- Let me sum up the ports which I discovered after several scans:

Port | Service
---- | ------- 
22 | SSH
25 | SMTP
80 | HTTP
443 | HTTPS
8000 | HTTP Alternative (Go Web Server)
9100 | Jetdirect (HP printer service)

- Further, I figured out that slow scans are preferred over aggressive scans to get results
- Some days passed while I put this challenge aside. Then I tried it again with following setup: `nmap -A -T2 3.10.42.19` ... no new insight though
- By coincidence it came to my mind that IRC's are a common service and defendtheweb perhaps is running one
- IRC is normally running on port 6667, but to be on the safe side I decided to check port 6000- 7000
- 1st try: `nmap -A -T3 -p6000-7000 3.10.42.19`: No result !
- 2nd try: `nmap -A -T2 -p6000-7000 3.10.42.19`: Success !

![grafik](https://user-images.githubusercontent.com/84674087/135901633-cbb705eb-0b19-4b28-8f6f-0abb53ff37cd.png)


<br />

## Crypt 5 / Crypt

![grafik](https://user-images.githubusercontent.com/84674087/134480226-ec945b9f-ebb2-4408-9141-b39e04cc1b66.png)

#### Solution
```
qoymlNlpY :ccdf lpy yzJ .qoh ln lxigqoh qlxlm eeiw zot ydpy gmipylnoC ,zot gmiyqdncyzo ho ydpy ci lniqk tN .lsie sooe tlpy ydpw yom ,smipy amd tdc tlpy ydpw tj lefolf gmigazb ho ydpy ci lniqk tN .tyicoiqzk ho ydpy ci lniqk tN .edminiqk d nd i clT
```

- It took me a while, until I noticed that the letters are not only replaced but the whole text is reversed !
- Look here: `qoymlNlpY :ccdf` ... this is actually the end of the text and should look like `fdcc: YplNlmyoq`
- So, `fdcc:` is most probably `pass:`
- From there you will decode the text step by step and reach to the solution
- Tools: [https://math.dartmouth.edu/](https://math.dartmouth.edu/~awilson/tools/frequency_analysis.html), [https://www3.nd.edu/](https://www3.nd.edu/~busiforc/handouts/cryptography/cryptography%20hints.html)

```
rotnemeht :ssap eht tub .rof em evigrof reven lliw uoy taht gnihtemos ,uoy gnitramstuo fo taht si emirc ym .ekil kool yeht tahw ton ,kniht dna yas yeht tahw yb elpoep gnigduj fo taht si emirc ym .ytisoiruc fo taht si emirc ym .lanimirc a ma i sey
```

- password: **thementor**

<br />
