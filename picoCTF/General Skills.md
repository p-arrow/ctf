## Obedient Cat

![grafik](https://user-images.githubusercontent.com/84674087/141130367-31752a27-caed-40d2-9bfb-23dce8a14274.png)

#### Solution
- `file flag`
- `cat flag`
- **FLAG**: picoCTF{s4n1ty_v3r1f13d_2aa22101}

![grafik](https://user-images.githubusercontent.com/84674087/141129265-6beaa1cd-4b02-4229-95e2-eb4b3ce11a3b.png)

## Wave a flag

![grafik](https://user-images.githubusercontent.com/84674087/141129139-6442abd3-b02c-4f3c-8550-eb46a9a1042c.png)

#### Solution
- `chmod +x warm`
- `./warm -h`
- **FLAG**: picoCTF{b1scu1ts_4nd_gr4vy_30e77291}

![grafik](https://user-images.githubusercontent.com/84674087/141129922-edee3212-516c-48b6-844d-df449a87e209.png)


## Python Wrangling

![grafik](https://user-images.githubusercontent.com/84674087/141142818-e5b4ef33-4d9e-477e-956f-d29ccc7ec02b.png)

#### Solution
- Download all three files

![grafik](https://user-images.githubusercontent.com/84674087/141143442-9d742fe7-cadc-484c-8895-1520238d30f1.png)

- To solve the challenge: `python3 ende.py -d flag.txt.en`
- **FLAG:** picoCTF{4p0110_1n_7h3_h0us3_67c6cc96}

![grafik](https://user-images.githubusercontent.com/84674087/141143265-1156d679-c127-4fd7-9344-71dd69698367.png)

<br />

## Nice netcat

![grafik](https://user-images.githubusercontent.com/84674087/141315622-b3ea5ce4-7d61-4160-8719-8baf6c4e0ce5.png)

#### Solution
- Open the terminal and enter the given command `nc mercury.picoctf.net 35652`
- This prints a bunch of numbers separated by newline

![grafik](https://user-images.githubusercontent.com/84674087/141318950-d167b147-0efd-4119-bc73-1d5fe8ebdbd9.png)

- When we turn these numbers from decimal into ASCII we get the flag
- To make our life easier we write a short python script as below:

```
string = ""
with open("text") as text:   // save the nc output into a file called "text"
        for line in text:    // look at each line of text
                string += chr(int(line.strip('\n')))  // remove newline, turn the string into integer and convert to ASCII
print(string)
```
- **FLAG:** picoCTF{g00d_k1tty!_n1c3_k1tty!_9b3b7392}

<br />

## Nice netcat

#### Solution

<br />

## Nice netcat

#### Solution

<br />

## Nice netcat

#### Solution

<br />

## Nice netcat

#### Solution

<br />
