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

## Crypt 3 / Crypt

![grafik](https://user-images.githubusercontent.com/84674087/134204791-8e61621c-2e87-4be6-a884-622ca821e5de.png)

#### Solution

- This code reminds me on morse code ... 
- [https://morsedecoder.com/](https://morsedecoder.com/)

![grafik](https://user-images.githubusercontent.com/84674087/134205193-7f9e86c3-e681-4961-a5ce-37f7b57b78df.png)

- password: **THANKYOUSIR**

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

