## SQLi 2 / SQLi

![grafik](https://user-images.githubusercontent.com/84674087/134689346-87c189a9-924e-4729-833a-d9a5a92674cf.png)

#### Solution
- When we click through the member alphabet we notice the parameter `?q=` in the URL:
![grafik](https://user-images.githubusercontent.com/84674087/134702569-65fce3cf-1e6e-402f-b9da-fef40acd5d7d.png)

- Being at section "D": We extend D to Derun (one available user) in the parameter
- The output is Derun only:

![grafik](https://user-images.githubusercontent.com/84674087/134702827-f2750dfb-2ea1-4875-8fea-8d3077c8a5af.png)

- We try to further manipulate the parameter by writing `Derun'`:
- The output is a Debug info:

![grafik](https://user-images.githubusercontent.com/84674087/134702341-cbeb29aa-9e7c-4116-a73e-1e5f4e86edfd.png)

- The database displays all usernames and admin when we enter for example this command: `' or 1=1 AND password = ' ' or 1=1--`


## Princess slag / Realistic

![grafik](https://user-images.githubusercontent.com/84674087/135134845-b2ba8e3a-6ea4-40bd-be24-13ffad196dc6.png)

#### Solution
- The Website:

![grafik](https://user-images.githubusercontent.com/84674087/135137977-7b8d648e-0ba2-4ab5-bce7-ff6da893f635.png)

- News: A collection of blog entries. Nothing interesting at the first glance
- Contact: We find an Email address called princess@kindom.far.away
- /admin.php: Login page to enter admin area (GET method used to send password !)
- The subdirectories are accessed via p-parameter:

![grafik](https://user-images.githubusercontent.com/84674087/135138486-716fa8b7-c3d0-498c-8bae-25e599c6a693.png)

- I play around with the parameter and below output appears when I use `%` or `#`:

![grafik](https://user-images.githubusercontent.com/84674087/135137316-b36be16c-5a91-40c0-9278-fc4acf5cc923.png)

- We read at [w3schools](https://www.w3schools.com/php/func_filesystem_file_get_contents.asp) about this PHP functionality:

> This function is the preferred way to read the contents of a file into a string.

- Syntax: `file_get_contents(path, include_path, context, start, max_length)`
- If configured incorrectly, this function can be an entry point for LFI or SSRF attacks (!)
- After playing around we try following code snippet `../admin.php%00` (%00 = Null Byte, stops the string in order to avoid undesired file extension)
- We get the login field directly on the main page 

![grafik](https://user-images.githubusercontent.com/84674087/135161512-9fa46e00-e277-4e6b-8162-88cbf451e273.png)

- When we check the source code of this new found page we get the final hint:

![grafik](https://user-images.githubusercontent.com/84674087/135161578-c44a7764-63cf-4bf8-a400-ca072aa707cd.png)

- password: **544e14708b**


<br />

## Xmas '08 / Realistic

![grafik](https://user-images.githubusercontent.com/84674087/135165137-ec93ffef-3b4c-4a23-a82b-403955a88440.png)

#### Solution
- Website:

- We click around and get finally to the submit-page where we can enter our own wish letter
- While checking the source code of this site we notice the href `mod.php`:

![grafik](https://user-images.githubusercontent.com/84674087/135169670-fd189fa0-2c4c-41a4-a0f5-d15d1b76dda5.png)

- We follow the link and get to a login page. How dare you ;)
