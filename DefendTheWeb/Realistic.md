## World of Peacecraft

![grafik](https://user-images.githubusercontent.com/84674087/134007737-b4ca4fb4-c120-4716-b7c8-3465b0122638.png)

#### Solution
- We click on the email button and get redirected to user's mailbox:

![grafik](https://user-images.githubusercontent.com/84674087/134007900-235c45ac-5d84-4f29-a98d-e56b9d24dd81.png)

- How about some dumpster diving? The Trash folder contains one mail...

![grafik](https://user-images.githubusercontent.com/84674087/134008144-0f3cd168-22ea-4517-bd16-0f5d6099228b.png)

- user: **uStudio**, password: **iamgod**

- We have to go to Inbox, click on email "Activate Account", follow the link and enter the found password


<br />

## Library Gateway / Realistic

![grafik](https://user-images.githubusercontent.com/84674087/134048474-644117ac-aab2-4c4c-a3e5-b288a4f571dc.png)

#### Solution
- We follow the link to Library Gateway and reach a login.php site:

![grafik](https://user-images.githubusercontent.com/84674087/134048683-773196cb-2de7-469f-824d-f95ada8c1236.png)

- We take a look at the sourcecode and find a hint regarding member directory
```
function loadimage()
			{
				var username= document.getElementById('username').value;
				var password= document.getElementById('password').value;
            
				URL= "members/" + username + " " + password + ".htm";      <<---- this line (!)
            
				path = URL;
				document.getElementById("status").innerHTML = 'Checking details...';

				req = getreq();
				req.onreadystatechange = exists;
				req.open("get", path, true);
				req.send(null);     
			}
```

- We enter: **https://defendtheweb.net/extras/playground/real/2/members/**

![grafik](https://user-images.githubusercontent.com/84674087/134051083-5ee9eab9-68fa-4b87-9256-5c879a57c880.png)

- Then we simply test each user name and password one by one until we find the correct pair
- user: **librarian** , **sweetlittlebooks**

<br />

## Princess slag / Realistic

![grafik](https://user-images.githubusercontent.com/84674087/135134845-b2ba8e3a-6ea4-40bd-be24-13ffad196dc6.png)

#### Solution
- The Website:

![grafik](https://user-images.githubusercontent.com/84674087/135137977-7b8d648e-0ba2-4ab5-bce7-ff6da893f635.png)

- News: A collection of blog entries. Nothing interesting at the first glance
- Contact: We find an Email address called princess@kindom.far.away
- /admin.php: Login page to enter admin area (GET method used to send password !)
- The subdirectories are accessed via p-parameter:

![grafik](https://user-images.githubusercontent.com/84674087/135138486-716fa8b7-c3d0-498c-8bae-25e599c6a693.png)

- I play around with the parameter and below output appears when I use `%` or `#`:

![grafik](https://user-images.githubusercontent.com/84674087/135137316-b36be16c-5a91-40c0-9278-fc4acf5cc923.png)

- We read at [w3schools](https://www.w3schools.com/php/func_filesystem_file_get_contents.asp) about this PHP functionality:

> This function is the preferred way to read the contents of a file into a string.

- Syntax: `file_get_contents(path, include_path, context, start, max_length)`
- If configured incorrectly, this function can be an entry point for LFI or SSRF attacks (!)
- After playing around we try following code snippet `../admin.php%00` (%00 = Null Byte, stops the string in order to avoid undesired file extension)
- We get the login field directly on the main page 

![grafik](https://user-images.githubusercontent.com/84674087/135161512-9fa46e00-e277-4e6b-8162-88cbf451e273.png)

- When we check the source code of this new found page we get the final hint:

![grafik](https://user-images.githubusercontent.com/84674087/135161578-c44a7764-63cf-4bf8-a400-ca072aa707cd.png)

- password: **544e14708b**


<br />

## Xmas '08 / Realistic

![grafik](https://user-images.githubusercontent.com/84674087/135165137-ec93ffef-3b4c-4a23-a82b-403955a88440.png)

#### Solution
- Website:

![grafik](https://user-images.githubusercontent.com/84674087/135170965-08818cc6-e723-476b-824d-aa5bf18c47b0.png)

- We click around and get finally to the submit-page where we can enter our own wish letter
- While checking the source code of this site we notice the href **/mod.php**:

![grafik](https://user-images.githubusercontent.com/84674087/135169670-fd189fa0-2c4c-41a4-a0f5-d15d1b76dda5.png)

- We follow the link and get to a login page. How dare you ;)

![grafik](https://user-images.githubusercontent.com/84674087/135170918-3864a256-8853-4e11-9cf3-982536a271b9.png)

- We have some potential usernames:
    - Sam Wright
    - Thomas Bell
    - Richard Head
    - Santa Claus
    - Admin ;)
    
- At the first glance, none of these is working in combination with simple/common passwords (I tried it with Burpsuite)
- So, let's bypass the login page with SQLi: `' or '1'='1` (entered in both input fields)
- Well done, we are now at **/mod.php?edit** !

![grafik](https://user-images.githubusercontent.com/84674087/135908220-403553a3-a62e-4987-aaed-761ac5b215ba.png)

- It is not possible to upload own files (e.g. the picture "xmas_warning")
- Instead, we can access files that exist on the web server, e.g. letter1.txt, letter2.txt ... also php files like index.php, top.php

![grafik](https://user-images.githubusercontent.com/84674087/135908557-4512dce6-0823-490b-8c48-008ce1ee9e58.png)

- This being said, why don't we copy&paste the content of `https://defendtheweb.net/extras/playground/xmas08/xmas_new.html` into index.php and save it?
- We do so and yes, it works ! 🙂


<br />

## Planet Bid / Realistic

![grafik](https://user-images.githubusercontent.com/84674087/135499843-15c32718-769e-4a87-9baf-7eeff8fb2322.png)

#### Solution
- PlanetBid username: Revoked.Mayhem
- Safe Transfer account (of Revoked.Mayhem): 64957746
- "Opponent" uses Safe Transfer
- "Opponent" uses email beta
- Other user names: John, nemesis, Zinc, Gizmo
- Top10 Passwords:

```
password
qwerty
pass
(name)
12345
abc123
123
letmein
asdfg
monkey
```

- Let us start with PlanetBid login website
- We open Burpsuite, activate our proxy and catch a POST request (I removed the cookie values):

![grafik](https://user-images.githubusercontent.com/84674087/135508306-9d611c8a-bbfa-47c8-8030-d85231db7f7d.png)

- We start with the username **admin** and grab the passwords listed above (I added some more for safety sake):

![grafik](https://user-images.githubusercontent.com/84674087/135508144-f842ab57-f29f-4c2f-83dc-3288f412439a.png)

- We succeed with password **letmein** !
- Now we have access to member DB and bids DB

![grafik](https://user-images.githubusercontent.com/84674087/135508509-c41327c2-1479-4362-ad70-95f28f9933e8.png)

- We obtain valuable information from member DB:

\# | User | Email
--| ---- | -----
11 | nemisis (!Attention! Not "nemesis" !) | jfelliot@mail.com
14 | John | run@dmail.com
23 | Zinc | Loltown@Googtube.com
29 | Gizmo | Decalz@mappers.com
31 | Revoked.Mayhem | Caffe@hotbiz.com
36 | admin | hellomum@yawn.go

- At bids DB we can discover the **"Opponent"** who made the deal with Revoked.Mayhem: It is **nemisis** !
- So, Revoked.Mayhem goes crazy because of £1.32 ?! 😄

![grafik](https://user-images.githubusercontent.com/84674087/135510187-f533c82c-fff2-42fb-9995-5d6af8579503.png)

- But how can we retrieve the password of any user, especially nemesis ?
- We look at the URL of member dbs/SQL query: `/planetbid/view.php?members&1=user&2=email`
- Let's modify it a bit ... `/planetbid/view.php?members&1=user&2=password` ... we get an empty column 😒
- How about: `/planetbid/view.php?members&1=user&2=pass` ... Success ! Hashes ! MD5 Hashes !
    - Check out here: [hash_identifier](https://hashes.com/en/tools/hash_identifier) 
- `hashcat -m 0 -a 0 hash.txt /usr/share/john/password.lst` (hash.txt contains the hashes)
   -  nemisis -> **password: chicken**
   -  Revoked.Mayhem -> **password: westwoodworld**
   -  John -> **password: gymsoft**
   -  Gizmo -> **password: evisumake**

- Now we try to logon to email_beta as nemisis, however, it doesn't work even ... after several attempts ... 
- Reminder: There is a password recovery service at Safe Transfer
- There we enter nemisis and jfelliot@mail.com - this works ! 

![grafik](https://user-images.githubusercontent.com/84674087/135525139-58ef7200-7dcb-4bf8-9abf-9034183511de.png)

- So, the mail address must be correct ...  is there something wrong with the username ?!
- I got a clue after several minutes passed : Maybe jfelliot is the username for email_beta ?
- In the inbox we find the new **password** details: **i.am.awesome**

![grafik](https://user-images.githubusercontent.com/84674087/135524922-25b59d39-399d-4cf9-bc97-c69a82e6df3a.png)

- We go back to Safe Transfer and login as nemisis
- Final step is to transfer the amount of £1.32 back to the account no. 64957746 (Revoked.Mayhem)


<br />

