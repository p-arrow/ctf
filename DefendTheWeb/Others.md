## 24 bit

![grafik](https://user-images.githubusercontent.com/84674087/134005997-a9e9a3e7-71fe-4c37-bed8-ae35ca8daabf.png)

#### Solution
- Download the file and examine it...depite the fact it has a .txt extension it doesn't contain text
- Let's use `hexdump -C b1.txt | head` to get a better understanding
   - `-C`  = hex+ASCII display
- We find **BM6** ... and we have the first bytes of the file which may help us when looking for magic numbers

![grafik](https://user-images.githubusercontent.com/84674087/134029403-90d8a0fa-8bae-4b73-9283-86ecb01d5a26.png)

- When we talk to google we get the hint that BM6 seems to be a **bitmap format**
- Let's change the file name from b1.txt to b1.bmp
- é voilá:

![grafik](https://user-images.githubusercontent.com/84674087/134007583-e8e821da-07ad-4266-ac7a-04874ebc7b37.png)

- user: **paint**, password: **rules**

<br />


## Secure agent

![grafik](https://user-images.githubusercontent.com/84674087/134008699-c61681bc-bb4c-49d1-be62-4bb93424ece1.png)

#### Solution
- The browser we use for this task is Firefox
- When searching for **secure_user_agent** and **Firefox** on google, we get pointed to the settings page: `about:config`
- After prompting "Accept the Risk and Continue" we reach the configuration
- Probably, when you enter secure_user_agent you won't find any existing setting
- Instead we have to create a new one called `general.useragent.override` as string

![grafik](https://user-images.githubusercontent.com/84674087/134033196-840e5930-5585-4e0b-abf7-747470309565.png)

- Then we allocate secure_user_agent as value, reload the page and see the congratulation banner
- Now we can delete the `general.useragent.override` entry


<br />


## Beach

![grafik](https://user-images.githubusercontent.com/84674087/134034816-cb5cd201-d135-4ec7-bf9f-fb0398fa730b.png)

#### Solution
- We download the picture b4.jpeg and investigate it with exiftool
- In case you miss it on your Kali machine: `apt install libimage-exiftool-perl`
- Then `exiftool b4.jpeg` and we get a bunch of information. An extract:

![grafik](https://user-images.githubusercontent.com/84674087/134039920-b943f978-b082-40d7-8eb0-54efc06ca571.png)


- The two most interesting information hereof are: 
    - user: **james** 
    - user comment: **I like chocolate**
- Well, now it's time to be creative ... after some attempts we simply try:
    - user: **james** 
    - password: **chocolate**
- Success!

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
25 | SMTP (closed)
80 | HTTP
443 | HTTPS
8000 | HTTP Alternative (Go Web Server)
9100 | Jetdirect (HP printer service)

- Further, I figured out that slow scans are preferred over aggressive scans to get results
- Some days passed while I put this challenge aside
- Then I tried it again with following setup: `nmap -A -T2 3.10.42.19` ... no new insight though
- By coincidence, it came to my mind that IRC's are a common service and perhaps defendtheweb is running one
- IRC is normally running on port 6667, but to be on the safe side I decided to check port 6000 - 7000
- 1st try: `nmap -A -T3 -p6000-7000 3.10.42.19`: No result !
- 2nd try: `nmap -A -T2 -p6000-7000 3.10.42.19`: Success !

![grafik](https://user-images.githubusercontent.com/84674087/135901633-cbb705eb-0b19-4b28-8f6f-0abb53ff37cd.png)

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

<br />

