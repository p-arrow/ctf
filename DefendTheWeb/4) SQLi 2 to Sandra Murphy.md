## SQLi 2 / SQLi

![grafik](https://user-images.githubusercontent.com/84674087/134689346-87c189a9-924e-4729-833a-d9a5a92674cf.png)

#### Solution
- When we click through the member alphabet we notice the parameter `?q=` in the URL:
![grafik](https://user-images.githubusercontent.com/84674087/134702569-65fce3cf-1e6e-402f-b9da-fef40acd5d7d.png)

- Being at section "D": We extend D to Derun (one available user) in the parameter
- The output is Derun only:

![grafik](https://user-images.githubusercontent.com/84674087/134702827-f2750dfb-2ea1-4875-8fea-8d3077c8a5af.png)

- We try to further manipulate the parameter by writing `Derun'`:
- The output is a Debug info: **DEBUG: SELECT username, admin FROM members WHERE username LIKE ... %'**

![grafik](https://user-images.githubusercontent.com/84674087/134702341-cbeb29aa-9e7c-4116-a73e-1e5f4e86edfd.png)

- The difference to SQLi1 is the WHERE clause, which does not contain `=` but the **LIKE Operator**
- The LIKE Operator is often used with wildcards such as: `%` `_` (FYR: [:https://www.w3schools.com/SQL/sql_like.asp](https://www.w3schools.com/SQL/sql_like.asp))
- For example, the SQL DB displays all usernames and admins when we enter `%` 
- Now the question: How can we manipulate the SQL query to present admins and their corresponding passwords ?? 
- Pro Tip: Go through each SQL operator, try to understand all of them and think about how to leverage those --> [www.w3schools.com/SQL](https://www.w3schools.com/SQL/default.asp)
- After a while we learn to escape the LIKE operator and use other operators subsequently
- My Trial & Error journey:
   - `___a%' AND password LIKE '`
   - `______x'; SELECT username FROM members WHERE username = 'Derun`
   - `%' ORDER BY admin ;--`
   - `%' ORDER BY 1--`
   - Until here it does no work out
   - `%' ORDER BY admin; '`
   - `%' ORDER BY admin DESC; '`
   - Now I notice that my SQL manipulation does have an effect onto the output (thanks to **ORDER BY Operator**)
   - By changing the admin order (ASC / DESC) I recognize that only bellamond is switching its position from top to bottom and vice versa
   - So, it seems this account is an admin
   - `%' UNION SELECT admin, password FROM members WHERE username LIKE '`
   - `%' UNION SELECT admin, password FROM members WHERE username LIKE '%' ORDER BY admin DESC; '` ... nearly
   - `%' UNION SELECT password, admin FROM members WHERE username LIKE '%' ORDER BY admin DESC; '` ... Yeah!
   - The **UNION Operator** helps to combine the result-set of two or more SELECT statements. 
   - I use the same table "members" but query different columns:
      -  **username, admin VS password, admin**
      -  The output is not handy and clear, but we realize that admins are displayed, but when there is no corresponding username, the password is shown instead
      -  More precisely: The SHA1 Hash (for bellamond: `1b774bc166f3f8918e900fcef8752817bae76a37`)
      -  This can be figured out here: [hash_identifier](https://hashes.com/en/tools/hash_identifier)

![grafik](https://user-images.githubusercontent.com/84674087/135313488-d88ff0dd-90cb-493d-8abf-03e1099e4888.png)

![grafik](https://user-images.githubusercontent.com/84674087/135313553-15f0fb5b-dab4-4659-80ac-cf73bb315d1d.png)

- When we enter username: **bellamond** and password: **sup3r** we pass the challenge :)

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

## Access Logs

![grafik](https://user-images.githubusercontent.com/84674087/135623993-bd1159e2-285f-40ef-ba8d-39c11fc3fda7.png)

#### Solution
- How does the output look like after failed login attempt?

![grafik](https://user-images.githubusercontent.com/84674087/135624955-7a971e7e-f4d7-45df-a321-af0da08ead69.png)

- Following data gets logged: date/time, password correct/wrong, entered username and IP address
- It is not requested to find specific username or password (!)
- Simply avoid detection when entering your data into the access log file
- ... So, we have to escape from parsing ...
- The standard escape sequence for end of line is `\n`
- When you enter `\n` in the username field and press enter you pass the challenge ;)

<br />

## Sandra Murphy

![grafik](https://user-images.githubusercontent.com/84674087/135635789-ab9783d4-b2eb-48f4-80d5-4e3be52b82fb.png)

#### Solution
- The content of the XML data:
```
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="users">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="user">
          <xs:complexType>
            <xs:sequence>
              <xs:element type="xs:string" name="login"/>
              <xs:element type="xs:string" name="password"/>
              <xs:element type="xs:string" name="realname"/>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

- A simple SQL injection does not work here: `' or '1'='1`
- Most probably this level is about XML injection
- More precisely: XPath, because this level contains a XML data with user data in it. Data from Sandra Murphy
- XPath can be used to navigate through elements and attributes in an XML document
- If so, we may have to search for **XPath Injection** ?!
    - For basic understanding: [https://www.w3schools.com/xml/xpath](https://www.w3schools.com/xml/xpath_intro.asp)
    - OWASP: [https://owasp.org/.../XPATH_Injection](https://owasp.org/www-community/attacks/XPATH_Injection) 
    - OWASP WSTG: [https://owasp.org/.../09-Testing_for_XPath_Injection](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection)

- The basic syntax of XPath injection: 
    - Username: `blah' or 1=1 or 'a'='a`
    - Password: `blah` 
- Let us think once more: We have no username and no password, but we have a real name
- Additionally OWASP states: *"In this case, only the first part of the XPath needs to be true. The password part becomes irrelevant, and the UserName part will match ALL employees because of the “1=1” part."*
- Applied to our challenge we need the username to be evaluated as TRUE 
- If we enter `blah' or 1=1 or 'a'='a` we don't get "Invalid login details" but **"Error on request"** ... are we getting closer ? :)
- The `1=1` part has apparently no effect here, so why don't we enter the information that we know is true ?!
- Let's modify the XPath injection: `blah' or realname='Sandra Murphy' or 'a'='a`
- SUCCESS!

