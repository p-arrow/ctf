
## Missile Codes / Forensics

![grafik](https://user-images.githubusercontent.com/84674087/137589545-a262053b-23fc-4602-bbf5-d3bbab253a44.png)

#### Solution
- First I try to mount the file forensics1.img onto Kali Linux: 
     - `sudo mkdir /mnt/disk`
     - `sudo mount -o loop ../forensics1.img /mnt/disk` 
- Surprise ! No files are mounted ... I try it several times and even though there is no error message it doesn't work
- Hint:
     - `fdisk -l [file.img]` To check from which block the file starts
     - `mount -o loop,offset=$((128 * 512)) file.img /mnt/disk1`: Wherein 128 is the start block and 512 the block size, to calculate the offset
     - `umount /mnt/disk1`: If you need to unmount the file system
- Hm, then let's check any forensic tools could extract information from an image file
- `binwalk forensics1.img` ... not much output except that reveals information about the file system: ext4
- `bulk_extractor forensics1.img`

> Bulk extractor tool extracts credit card numbers, URL links, email addresses, which are used digital evidence. This tool lets you identify malware and intrusion attacks, identity investigations, cyber vulnerabilities, and password cracking. The specialty of this tool is that not only does it work with normal data, but it also works on compressed data and incomplete or damaged data.

- We get some interesting insight into the data: several .txt files

![grafik](https://user-images.githubusercontent.com/84674087/137773155-c714a80b-92d2-4e37-b164-7451d356e2c0.png)

- But it's time consuming to look through all files. Moreover the data seems a bit messy
- By searching on the internet I find another tool: foremost

> Foremost is a faster and reliable Command line based recovery tool to get back lost files in Forensics Operations. Foremost has the ability to work on images generated by dd, Safeback, Encase, etc, or directly on a drive. Foremost can recover exe, jpg, png, gif, bmp, avi, mpg, wav, pdf, ole, rar and a lot other file types.

- `apt install foremost`
- `foremost forensics1.img -o test` This gives us a niet answer (with "test" as output folder):
     - 95 extracted files: gif, jpg, pdf, rar, dll, png
     - The rar data is password protected (!): 00017414.rar and 00020114.rar
     - When we apply `md5sum` onto both files we get the same hash. So, both files seem identical
- Initially I try to find any password hint among the jpg and pdf section, because these look most promising. Other data does not contain suitable hints
- After a while I find some password hints, but none of these seems to work when unpacking the rar archive: **secret, CYB3RCR4WL3R, 123456**
- Then, if we don't know the password why don' we bypass it?
- To utilize john the ripper for that task we need to use an utility called rar2john:
      - `rar2john 00017414.rar > code.hashes` 
      - `john code.hashes` ... this takes some minutes to complete
      - The result is hilarious: **123** is the cracked password

![grafik](https://user-images.githubusercontent.com/84674087/137771903-0ef5aaa8-bd8c-4ed9-9acc-d18f203ee2fb.png)

- The wav file is called **conversation_dtmf**
- If I play the audio it sounds like an old 56k modem dials in ...
- When we look for we find "dtmf" we find it's a abbreviation of **Dual Tone Multi Frequency**
- Then we search for "decode dtmf tone from wav file" and get this handy site: [http://www.dialabc.com](http://www.dialabc.com/sound/detect/index.html)

![grafik](https://user-images.githubusercontent.com/84674087/137801325-362ea58c-7d06-49a1-b64e-61d46df113be.png)

- When uploading the wav file we get this code: **AA6BA4A83C67DDC7**
- I expected too much, because when I enter this code into the answer field at DefendTheWeb I cannot succeed 😞
- I decided to play around with the code like inputting the code reversed or only some parts of it (straight and reversed) - still no success
- Finally, I listened again to the sound file and I noticed this pattern: ####...####...####...####
- There is a break in between four connected sounds
- Maybe, I need to split the code into four fragments like: **AA6B-A4A8-3C67-DDC7**
- Yes, it works !
