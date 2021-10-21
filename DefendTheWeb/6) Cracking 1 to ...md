## Cracking 1 / Cracking

![grafik](https://user-images.githubusercontent.com/84674087/137898570-c9617660-c36f-4b71-8c83-16e2433e40a0.png)

#### Solution
- `file cracking1.exe`: Identify exact file type
   - PE32 executable (console) Intel 80386 (stripped to external PDB), for MS Windows
- `ldd cracking1.exe`: Print shared object dependencies
   -  Since we talk about a .exe file we only get "not a dynamic executable" as response
- `nm cracking1.exe`: searches the executable/binary for important data structures and functions
   - no symbols (because cracking1.exe is stripped unfortunately)
- `binwalk cracking1.exe`: extract data from binary

![grafik](https://user-images.githubusercontent.com/84674087/137899539-e644a943-9233-4678-b7ec-e1360436284c.png)

- `strings -n 6 cracking1.exe`: Looks for strings

![grafik](https://user-images.githubusercontent.com/84674087/137901419-9841a700-c690-4ee2-ae01-533dab4d8073.png)

- `objdump cracking1.exe -s`: Display information from object by resembling assembler instructions
   - `-s` = full contents 
   - not much new output  
- `objdump cracking1.exe -d`: Display assembly code
   - `-d` = disassemble 
   - good output but not so handy as real Reverse Engineering tools

All those tools mentioned above give us a first impression about what this .exe contains, but to get deeper into program flow and structure we need a Revere Engineering tool. There are several tools available, but for now we will take a look at **ollydbg** only.

- If not yet installed: `sudo apt install ollydbg`
- Initial info: [https://www.kali.org/tools/ollydbg/](https://www.kali.org/tools/ollydbg/)
- I will roughly describe what I did, however, honestly speaking it's easier to demonstrate such tools along with a video
- Despite that fact, I stick to comments in this markdown for efficiency purpose ;-)

1. Load crackin1.exe into ollydbg
2. Right click in the main thread, select "search for" and then "all referenced text strings". This brings us to this overview:

![grafik](https://user-images.githubusercontent.com/84674087/138116134-0b1fcec7-1fe6-4cec-9947-358ba2a5d337.png)

...

Finally we find the password: **JE74toJNZ75** 🙂

<br />

**...**

<br />

## Captcha 1 / Captcha

![grafik](https://user-images.githubusercontent.com/84674087/138234030-3f8ec05d-1eef-4f17-a2a6-70933cdbb68b.png)

#### Solution
```
//Access captcha.php image
var captcha = document.getElementsByTagName("img")[1]

//extract string from image
//tesseract.js is popular on that, referring to OCR [Optical Character Recognition])
//But I am to solve this task inside the console with JS only
captcha ...

//Reverse string
captcha = captcha.split('').reverse().join('')

//Enter reversed string in answer field
document.getElementById("answer").value="captcha";

//Press submit button
document.forms[1].submit()
```


<br />

## Unknown Sender / GPG

![grafik](https://user-images.githubusercontent.com/84674087/138114953-4cbdd87d-4018-4dd8-8ad1-16a0d7aabdf1.png)

Pretty Good Privacy or PGP is a popular program used to encrypt and decrypt email over the Internet.
Its is used to authenticate messages with digital signatures and encrypted stored files.

PGP: Proprietary solution owned by Symantec
OpenPGP: Originally derived from PGP; wdiely used email encryption standard --> [https://www.openpgp.org/](https://www.openpgp.org/)

#### Solution
- Below is the PGP message block, which shows a message encrypted with pgp keys
- We would need a public and private key to decrypt this message
- However, we have neither the keys nor is it our task here
- Instead, let's check who has sent and signed this message by using the Linux tool `gpg`
- `gpg` is the OpenPGP part of the GNU Privacy Guard
- Good tutorial is given here: [https://www.howtogeek.com](https://www.howtogeek.com/427982/how-to-encrypt-and-decrypt-files-with-gpg-on-linux/)

```
-----BEGIN PGP MESSAGE-----  
owEBfQKC/ZANAwAIAUi/Y1ergOteAaweYg1wZ3AxaW5wdXQudHh0XSYJ3khlbGxv  
IHdvcmxkiQJLBAABCAA1FiEE6DCERSe70kQIH/X5SL9jV6uA614FAl0mCd4XHGFp  
ZGVuQGRlZmVuZHRoZXdlYi5uZXQACgkQSL9jV6uA6150GRAAsMaWuzODcLaGEP62  
7a/JaGUB/nKAFhDEo7J9JEihn92q9efNPjEMH97WJl4GU25cUZLyrtp4cDl5mZ0T  
CiguNISviQLHKTz6KBgxoxEi5QCgh0uygfVCdJOL+Gah+EWqSZ1ACSrt+ilMJRna  
rppcNPvwUUP3fw/QcqcoHWIh4Y4Nij0Q3+ywGNgi+JxBWUnWI5RJdSY579dCNOgD  
RJW4FcWHNovKuAgs0pNBU+TX07qJpdH9Kmru5D+QDKFTVBIgDxs2EDHa/tMRgG6C  
MNVnEW4MsAcwqOaU6M/HyzQdy27nRJZjYFImM5si8JXk8whWUAJU5EOQuQsO62X8  
MsFqyIuSWMq3SY0tCXrWZwcT7t3zR9DyxZQRXEjmcp6XwnjqZKL2C8H4r3knu4vm  
JaofblxYHuMWDjzMPD7KGUyuhHBDWSBGBs3njGgplzUWcYaNtegmj2x/a3TjSznF  
bHZPA6VHQLzlEnSZe243Wx5PGVuYOxqkBKe9wD48xJXWnblu7UE73WKy5r8q2Qma  
q9l/TZfs3OZ23HNK1aPJ7Aii3FD4KF31KrwbOq54oO/8J15Vygz/S0XJmE4WdXJD  
gMnSRdNNyeUlOQmgeNx8NW0wRhW0bQclHSAUKyw6p5WbzYB8KSnLfDBIx2p8FfJb  
DK8y2toYISJVK3qK+CEjl3ntntE=  
=He9M  
-----END PGP MESSAGE-----  
```

- We save the message as `dtw.key`
- Then we check/verify the signer when we decrypt the message: 
   - `gpg --decrypt dtw.key > plain.txt` OR
   - `gpg --verify dtw.key`
- This yields below output:

![grafik](https://user-images.githubusercontent.com/84674087/138153609-7354f2a7-2188-4b38-b67b-560c7dec4ca3.png)

- If we enter **Aiden** or **aiden@defendtheweb.net** into the answer field we get "invalid login details"
- We do not have the public key of aiden ...
- But we can search for it online: `gpg --keyserver pgp.mit.edu --search-keys aiden@defendtheweb.net`
   -  `pgp.mit.edu` is the public key server of MIT University
   -  Apparently a popular one where chance is high you find the key you are looking for
- The output is not helpful though: 

![grafik](https://user-images.githubusercontent.com/84674087/138156997-6f27cd9d-9a16-4a33-be49-c5f8f9030aeb.png)

- Another command which may provide us the desired hint: `gpg --list-packets --verbose dtw.key`
- This commmand is for debugging purpose and dumps below information

![grafik](https://user-images.githubusercontent.com/84674087/138159938-39edf5c4-87c8-428f-9dd2-8f2184d225df.png)

What we notice here:

- issuer keyid:  48BF6357AB80EB5E
- data packet name: pgp1input.txt
- issue fingerprint: E830844527BBD244081FF5F948BF6357AB80EB5E

So, you might ask yourself how we can utilize this output to proceed on this challenge.
Well, I found this pretty clue after scrolling through `man gpg`:

![grafik](https://user-images.githubusercontent.com/84674087/138163003-2e287960-653c-493e-8663-46c37e0c1feb.png)

![grafik](https://user-images.githubusercontent.com/84674087/138163082-3610029e-d905-4859-8d82-8ea46e121abf.png)

- Awesome, so let's give it another try
- We use: `gpg --keyserver pgp.mit.edu --receive-keys 48BF6357AB80EB5E`
- This provides us with the information about the public key owner: **Aiden Lawrence**
- SUCCESS ! 😙

<br />

## Private Correspondence / GPG

![grafik](https://user-images.githubusercontent.com/84674087/138331725-a24af591-9a04-4d40-9198-a2f1e1d806c6.png)

#### Solution
- Plain text: `Hello, my name is TestDummy but you can call me lord_tryzalot`
- We open our terminal and enter: `gpg --recipient aiden@defendtheweb.net --armor --encrypt plain.txt`
   - `--armor` = for base64 encoded output
   - `--recipient` = to specify the public key

![grafik](https://user-images.githubusercontent.com/84674087/138336044-f9b90916-758f-419d-a0fb-2267cca5e29e.png)

- This produces **plain.txt.asc**:

![grafik](https://user-images.githubusercontent.com/84674087/138336367-8fd13af5-dcaf-4b2a-af7b-fe70fa74729f.png)

- We copy&paste this into the answer field and succeed !
