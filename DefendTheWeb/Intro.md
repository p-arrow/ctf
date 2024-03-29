## Intro1

![grafik](https://user-images.githubusercontent.com/84674087/133981338-556d2ef2-a42e-4495-b18c-397e7358f300.png)

#### Solution
- Check sourcecode: `Control + U`

![grafik](https://user-images.githubusercontent.com/84674087/133981421-3ef0c035-60c8-4120-91b6-78c2e503983c.png)

- There we go:
- username: **haveahappyday**, password: **126b9e5884**

<br />

## Intro2

![grafik](https://user-images.githubusercontent.com/84674087/133981686-bccbb43e-2ca4-4905-b531-ae70cdcd93ec.png)

#### Solution
- Check sourcecode: `Control + U`

![grafik](https://user-images.githubusercontent.com/84674087/133981762-e9a0a373-d7a9-4b80-89a6-152fd4b24af1.png)

- username: **webtool**, password: **2b718cbba9**

<br />

## Intro4

![grafik](https://user-images.githubusercontent.com/84674087/133981686-bccbb43e-2ca4-4905-b531-ae70cdcd93ec.png)

#### Solution
- Check sourcecode: `Control + U`
![grafik](https://user-images.githubusercontent.com/84674087/133982073-364040b4-db6d-44e2-9a7f-740c039e50d8.png)

- Enter: `https://defendtheweb.net/extras/playground/9d2K4Fw.json`
- username: **thomas**, password: **S9234HKFnsd**

<br />

## Intro5

![grafik](https://user-images.githubusercontent.com/84674087/133982223-ab9a66bc-762f-4a8c-ac1d-9f5964b11d06.png)

#### Solution
- Check sourcecode: `Control + U`

![grafik](https://user-images.githubusercontent.com/84674087/133982519-b6d2a0a0-0323-4861-b946-a8882dae1e7d.png)

- password: **3315d26986**

<br />

## Intro6

![grafik](https://user-images.githubusercontent.com/84674087/133982662-a1c44d95-446d-4d75-9950-e6f57e7b63f6.png)

#### Solution

- Right-Click on dropdown menu, then select "Inspect Element"

![grafik](https://user-images.githubusercontent.com/84674087/133983269-ba10671d-bca1-4f07-92e0-dc138d42499e.png)

- Change one username to "soft_member", press enter and select "soft_member" from the dropdown menu

![grafik](https://user-images.githubusercontent.com/84674087/133983496-85542a8c-08d1-413d-b40a-d77d18109128.png)

- Success!

<br />

## Intro7

![grafik](https://user-images.githubusercontent.com/84674087/133983571-78e58627-8c69-4f0a-88ba-62bb353d39b1.png)

#### Solution

- Check: `https://defendtheweb.net/robots.txt`
- There you find following hint: `https://defendtheweb.net/extras/playground/jf94jhg03.txt`

![grafik](https://user-images.githubusercontent.com/84674087/134000483-05500e45-6139-413e-9f22-087cd0aabd72.png)

- username: **visualmaster**, password: **0ff735d018**

<br />

## Intro8

![grafik](https://user-images.githubusercontent.com/84674087/134000593-ca993565-247c-4bdc-b2de-e5f8f7d141c2.png)

#### Solution
- Check sourcecode: `Control + U`

![grafik](https://user-images.githubusercontent.com/84674087/134001109-8fa34a36-229f-4c70-8a81-406d930c6c12.png)

- Following the above hint leads to this binary code:
```
01100010 01110101 01110010 01101110 01100010 01101100 01100001 01111010 01100101 
01001100 01110000 00111001 01000101 01001101 00110010 00110111 01000111 01010010 
```
- Using a binary2ASCII converter reveals the credentials ([https://www.branah.com/ascii-converter](https://www.branah.com/ascii-converter))
- username: **burnblaze**, password: **Lp9EM27GR**

<br />

## Intro9

![grafik](https://user-images.githubusercontent.com/84674087/134001497-01d70453-a3a1-477b-86a1-8a23c0cda1ce.png)

#### Solution
- Check sourcecode: `Control + U`

![grafik](https://user-images.githubusercontent.com/84674087/134001619-40da0133-4449-4fa8-8885-61264e4ee524.png)

- This reveals a hidden email: *admin@defendtheweb.net*
- When playing around you will notice that the request function only works as below:
   - Both email addresses have different value: *invalid*
   - Both email addresses have the admin@ address: *Login details requested*
   - Both email addresses have same value except admin@: *Login credentials get revealed*
- username: **evilember**, password: **e352c1075e**

<br />

## Intro12

![grafik](https://user-images.githubusercontent.com/84674087/134003853-056a4f67-f03e-4b98-90af-6a71f92974f9.png)

#### Solution
- The shown code `1c63129ae9db9c60c3e8aa94d3e00495` is a MD5 hash: [https://hashes.com/en/tools/hash_identifier](https://hashes.com/en/tools/hash_identifier)
- After decryption: **1qaz2wsx**


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


## Sid / Intro

![grafik](https://user-images.githubusercontent.com/84674087/134189910-2fccacec-840f-4538-a491-a863deb3e67e.png)

- When we click on enter:

![grafik](https://user-images.githubusercontent.com/84674087/134190013-6773ca3d-4a2f-4452-ac9b-866127c6d2ab.png)

#### Solution

- Open DevTool: `Control + Shift + E`
- Go to storage and find four cookies, wherein only one is related to this game: *i3_access = false*
- Change the value to **true** et voilà
