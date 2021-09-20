## 24 bit

![grafik](https://user-images.githubusercontent.com/84674087/134005997-a9e9a3e7-71fe-4c37-bed8-ae35ca8daabf.png)

#### Solution
- Download the file and examine it...depite the fact it has a .txt extension it doesn't contain text
- Let's use `hexdump -C b1.txt | head` to get a better understanding
   - `-C`  = hex+ASCII display
- We find **BM6** ... and we have the first bytes of the file which may help us when looking for magic numbers
- When we talk to google we get the hint that BM6 seems to be a **bitmap format**
- Let's change the file name from b1.txt to b1.bmp é voilá:

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


