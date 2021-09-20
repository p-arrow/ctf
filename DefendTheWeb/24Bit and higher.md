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
