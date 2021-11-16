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

## Aliens / Stego

![grafik](https://user-images.githubusercontent.com/84674087/136847842-4b6d98a1-e422-42ba-b73d-dd60390a6ae0.png)

#### Solutions
- When we listen to the sound file we only hear cryptic alien sounds
- Typical forensic tools like `binwalk`, `strings`, `steghide` or `hexdump` don't bring us ahead
- Since we are speaking about an audio file, how about an audio analyze software?
- After searching through the web I found recommendations about:
   - [Sonic Visualizer](https://sonicvisualiser.org/download.html)
   - [Audacity](https://www.audacityteam.org/); further installation details: [github](https://github.com/audacity/audacity/blob/master/BUILDING.md)
   - At the end of the day I decided to go along with Audacity
- We open the file in audacity (Initially I played around with the settings to get used with the program)
- I figured out that applying the spectogram filter provides a better insight that the original sound waveform
- Additionally I zoomed into the relevant frequency area and changed the color setting

![grafik](https://user-images.githubusercontent.com/84674087/138738405-de0f8080-8895-4ada-bb28-9dbfa51b2cec.png)

- We recognize a code that is most probably the password...but...what kind of code is that?
- Looks similar to morse code but this is a wrong conclusion
- Honestly speaking I needed a hint to figure out that we are speaking about mayan numbers (!)
- The Mayan Numbers from 0 to 19 are shown below

![grafik](https://user-images.githubusercontent.com/84674087/138738457-48940187-8610-4c98-9072-45ac40d4d7fd.png)

- When we decode the message we recive this code: **69593078616075**
- The numbers itself are not the password, unfortunately. Still we see the message "invalid login details" when entering this code as it is ...
- First, I thought the stereo format requires us to add up both lines (left and right sound), e.g. 12 - 18 - 10 - 18 ...
- That is not the case!
- Secondly, I converted the numbers into letters: **FIEIC0GHFAF0GE**  (Zero remains Zero)
- No success either!
- Thirdly, these numbers may represent another format? Like hexcode? We simply use a hex decoder to turn the numbers into ASCII: **iY0xa`u**
- SUCCESS !


<br />

## 0xBBA5 / Stego

![grafik](https://user-images.githubusercontent.com/84674087/142041047-4e5e61b6-1e7f-4356-9f9d-cc596b3126e3.png)

#### Solution
- The text file shows reversed text. We transform it accordingly

input | reversed
----- | --------
17:xeH | Hex:71
70:loC | Col:07
0121:woR | Row:1210
AF:xeH | Hex: FA
41:loC | Col:14 
9021:woR | Row:1209
11:xeH | Hex:11
51:loC | Col:15
0021:woR | Row:1200
47:xeH | Hex:74
60:loC | Col:06
1911:woR | Row:1191

- I download the image **DSWii6x.png** and try to analyze it with zsteg
- `zsteg DSWii6x.png`

![grafik](https://user-images.githubusercontent.com/84674087/142044248-f0ae8659-4085-4b6a-b1df-ad2f63d5da34.png)

- Another try: `binwalk DSWii6x.png`

![grafik](https://user-images.githubusercontent.com/84674087/142044478-94043a93-8d1a-4072-88fb-8c1f48497af9.png)

<br />
