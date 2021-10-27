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
- Let's try to rewrite the code: Instead of `document.thecode = 'code123'` we could write `var thecode = 'code123'` ... but the password code123 still doesn't work
- Then, why don't we delete the whole part `document.thecode = 'code123'` and implement 'code123' directly? 
	- Means, for instance, `if(document.getElementById('password').value == code123`
	- Still no success !
- Maybe 'code123' is just not the right answer
- We start the search engine `Control + F` and enter "thecode": 4 hits ! The last hit reveals new information at the end of the sourcecode:

![grafik](https://user-images.githubusercontent.com/84674087/134198815-3ed975ef-13fe-48f1-878f-12d8d3db31f4.png)

- No matter what we change at the beginning of the sourcecode, "thecode" gets overwritten in the final end ...
- We translate the hexcode into ASCII and get this phrase: **c221802f20**

![grafik](https://user-images.githubusercontent.com/84674087/134201166-f613eeb3-cd36-4eba-be3c-520083e2828f.png)

- This is the correct password because we can enter the next game !

**ALTERNATIVE**
- Open DevTool: `Control + Shift + E`
- Go to Console, enter `document.thecode`

![grafik](https://user-images.githubusercontent.com/84674087/134201040-7d3667b3-411c-44f4-a82d-cde5150e8983.png)

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
