## Information

![grafik](https://user-images.githubusercontent.com/84674087/141322776-9f8edb55-e114-4753-9457-b7786c86007a.png)

#### Solution
- The first step: `exiftool cat.jpg`

![grafik](https://user-images.githubusercontent.com/84674087/141324712-9f5d9685-219b-4808-9ff4-151dd91e25d4.png)

- The first two eye-catching spots are **Current IPTC Digest** (hexcode?) and **License** (Base64 code?)
- The IPTC Digest does not reveal any helpful glue but the License does 🙂 It's truely Base64
- `echo "cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9" | base64 -d`
- - **FLAG:** picoCTF{the_m3tadata_1s_modified}

<br />

## xx

#### Solution

<br />
