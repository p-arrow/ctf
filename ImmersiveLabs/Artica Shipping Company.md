> The Arctica Shipping Company cyber range is an offensive range that contains a Windows domain with connected Windows hosts and an intranet website. 
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
- Result: Host 4 has **Apache/2.4.29** running

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

- We start scanning host 4 for vulnerabilities: `nmap -p80 --script=vuln 10.102.11.66`
- Result: It seems no obvious DOM based XSS or stored XSS vulnerability is exisiting
- Further, after analyzing the website we can confirm there is no input field available, i.e. no file upload mechanism
- The leftover option seems SQL Injection, so we follow this track ...

