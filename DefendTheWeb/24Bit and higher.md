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

## Crypt1/Crypt

![grafik](https://user-images.githubusercontent.com/84674087/134033376-d5ea4c9b-e2e5-41d9-9fc0-a7e47a88b568.png)

#### Solution
- After looking at the obfuscated text we can quickly recognize that the strings are reversed. 
- Let's reverse the string in python

```
string = "tpyrcoow :ssap siht retne level siht etelpmoc oT .rewop niarb fo tol a yolpme ot deen lliw uoy ,cigol dna noitpyrced tuoba lla era slevel esehT .sihtkcah no slevel tpyrc eht ot emoclew ,olleH"

string_rev = string[::-1] 
```

- This produces below output:

> Hello, welcome to the crypt levels on hackthis. These levels are all about decryption and logic, you will need to employ a lot of brain 
power. To complete this level enter this pass: woocrypt


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

## Intro3 / Javascript

![grafik](https://user-images.githubusercontent.com/84674087/134040350-f442fe4d-49fe-49a2-89fc-9c44eed43606.png)

#### Solution
- Check sourcode: `Control + U`
- We find below code snippet on line 36:

`<script type='text/javascript'> $(function(){ $('.level form').submit(function(e){ e.preventDefault(); if(document.getElementById('password').value == correct) { document.location = '?pass=' + correct; } else { alert('Incorrect password') } })})</script>`

- We recognize that the function compares the entered password with the variable **correct**
- Is this variable maybe hardcoded somewhere inside the website/sourcecode? Let's search with `Control + F`

![grafik](https://user-images.githubusercontent.com/84674087/134044546-d20d141b-2825-4e65-9377-817a9cc65a5e.png)

- password: **e0c00ddec3**

<br />

## Squashed image / Stego

![grafik](https://user-images.githubusercontent.com/84674087/134044679-3a62b1d5-db2f-4c7f-8701-9f4873340b7e.png)

#### Solution
- Considering the title of this game - Stego - the picture probably contains hidden information
- First, we start with **exiftool**, but it no interesting information at the first glance
- So, let's try **steghide**: `steghide extract -sf b5.jpg`

![grafik](https://user-images.githubusercontent.com/84674087/134047514-485db8d4-1bb1-4232-b3be-7493e418b2bc.png)

- How about **binwalk** which can also extract hidden data: `binwalk b5.jpg`

![grafik](https://user-images.githubusercontent.com/84674087/134047623-4a5e0a94-506d-493c-a5a3-a57bb03811ba.png)

- The image b5 seems to be zipped together with secret.txt
- `unzip b5.jpg`
- `cat secret.txt`
- user: **admin** , password: **safe** 

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

## HTTP method / Intro

![grafik](https://user-images.githubusercontent.com/84674087/134051998-fd93b506-8410-4760-83f2-b1e6a2994182.png)

#### Solution
- In the first place, we try to send a POST request by using Burp

![grafik](https://user-images.githubusercontent.com/84674087/134057291-25608174-4be4-4fa1-a173-feb5ae99962b.png)

- However, no reaction ... even when changing the input style to `password:d612765f63` or attaching the password as parameter to the URL
- Another attempt is Basic HTTP Authentication: [developer.mozilla.org/HTTP/Authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)
```
POST /playground/http-method HTTP/2
...
...

Authorization: Basic d612765f63
```

- This didn't help either
- So, why don't we check the sourcecode again ...

![grafik](https://user-images.githubusercontent.com/84674087/134056972-89920fd0-84f8-4089-a0fc-753419011408.png)

- It seems we have to input some HTML code by ourselves in order to enter the password inside a form
- Let's copy the needed HTML code from another/previous game
- Then simply remove the username section and add your specific token value

```
<form class="form--intro-1" method="post" enctype="multipart/form-data">

       <div class="input input--hidden " data-group="token">
         <input type="hidden" name="token" id="token" value="enterYourValueHere" maxlength="" placeholder="" class="u-full-width" />
       </div>
         
       <div class="input input--hidden " data-group="formid">
         <input type="hidden" name="formid" id="formid" value="enterYourValueHere" maxlength="" placeholder="" class="u-full-width" />
       </div>
                          
       <div class="input input--password " data-group="password">
         <label for="password">Password</label>                            
         <input type="password" name="password" id="password" value="" maxlength="" placeholder="" class="u-full-width" />
       </div>
      
       <button type="submit" class="button button--main right">Log in</button>
</form>
```

- Then the website looks like below and we can proceed by using the given password

![grafik](https://user-images.githubusercontent.com/84674087/134059264-7f5cf3e2-e5dd-410c-a85b-a2aa43bcab05.png)

<br />

## Crypt2/Crypt

![grafik](https://user-images.githubusercontent.com/84674087/134059760-880e4ffd-496f-4ff6-8642-d15a888f1788.png)

#### Solution
- Encrypted text: 
```
Aipgsqi fego, xlmw pizip mw rsx ew iewc ew xli pewx fyx wxmpp rsx xss gleppirkmrk. Ws ks elieh erh irxiv xlmw teww: wlmjxxlexpixxiv
```

- The first try to decode this text was based on ROT13: `echo "TEXT" | tr a-zA-Z n-za-mN-ZA-M` 
- But this did not work out ... 
- Then, by looking at the text we could guess some words. The last word **wlmjxxlexpixxiv** could be the password. If so, the word **teww** could stand for "pass"
- To verify our assumption we use this site for frequency analysis: [math.dartmouth.edu](https://math.dartmouth.edu/~awilson/tools/frequency_analysis.html)
- We start with our guess (teww = pass) and decode other letters step by step
- Then we finally catch the password: **shiftthatletter**

<br />

## Sid / Intro

![grafik](https://user-images.githubusercontent.com/84674087/134189910-2fccacec-840f-4538-a491-a863deb3e67e.png)

![grafik](https://user-images.githubusercontent.com/84674087/134190013-6773ca3d-4a2f-4452-ac9b-866127c6d2ab.png)

#### Solution

- Open DevTool: `Control + Shift + E`
- Go to storage and find four cookies, wherein only one is related to this game: *i3_access = false*
- Change the value to *true* et voilà

<br />

## Intro 10 / Javascript

![grafik](https://user-images.githubusercontent.com/84674087/134190677-fe1e497a-245a-4115-a9c5-3ebd156a8b78.png)

#### Solution
- Check sourcode: `Control + U`
- We find below code snippet on line 36:

```
<script type='text/javascript'> 
	document.thecode = 'code123';
	$(function(){ $('.level form').submit(function(e){ e.preventDefault();
		if(document.getElementById('password').value == document.thecode) 
			{ document.location = '?pass=' + document.thecode; } 
		else { alert('Incorrect password') } })});
</script>
```

- We can recognize the hardcoded password **code123** and use it to enter the next level ... but we fail !
- Let's try to rewrite the code: Instead of `document.thecode = 'code123'` we could write `var thecode = 'code123'` ... but this doesn't work either
- Then, why not delete the whole part `document.thecode = 'code123'` and implement 'code123' directly into the script? 
	- Means, `if(document.getElementById('password').value == code123`
	- Still no success !
- Maybe 'code123' is just not the right answer
- We start the search engine `Control + F` and enter "thecode": 4 hits ! The last hit reveals new information:

![grafik](https://user-images.githubusercontent.com/84674087/134198815-3ed975ef-13fe-48f1-878f-12d8d3db31f4.png)

- We translate the hexcode into ASCII and get this phrase: **c221802f20**

![grafik](https://user-images.githubusercontent.com/84674087/134201166-f613eeb3-cd36-4eba-be3c-520083e2828f.png)

- This is the correct password because we can enter the next game !

**ALTERNATIVE**
- Open DevTool: `Control + Shift + E`
- Go to Console, enter `document.thecode`

![grafik](https://user-images.githubusercontent.com/84674087/134201040-7d3667b3-411c-44f4-a82d-cde5150e8983.png)
