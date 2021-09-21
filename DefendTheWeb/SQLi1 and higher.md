## SQLi 1 / SQLi

![grafik](https://user-images.githubusercontent.com/84674087/134203849-9545248b-3071-4645-8aef-0f189a4da224.png)

#### Solution

- We enter a typical SQL injection inside the username field: `' or '1'='1`
- We get "Invalid login details"

![grafik](https://user-images.githubusercontent.com/84674087/134204439-0004d1d9-f866-4852-82b6-9d66e4c49936.png)

- Then we enter the same code in both fields: Success !

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

- Puhhh ! Nothing special to find at the sourcecode. No hidden password, no suspicious JS script ... 
- After a while, we recognize a search parameter in the URL: https://defendtheweb.net/playground/intro11/ **?input**
- We play around and enter specific values: `?input=123` , `?input=kgjgfkjhkjhk` etc.
- Nothing happens
- When we remove the parameter and reload the page, the parameter simply returns


