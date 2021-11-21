## Natas0

#### Task

![grafik](https://user-images.githubusercontent.com/84674087/133155764-5376c618-69cb-45ef-b76f-3a4060879765.png)

#### Solution

- Open source code: `Control + U`
- Password: **gtVrDuiDfck831PqWsLEZy5gyDz1clto**

![grafik](https://user-images.githubusercontent.com/84674087/133155920-d7b3aa9c-886f-4087-a5f3-dfc95e0bb71e.png)

<br />

## Natas1

#### Task

![grafik](https://user-images.githubusercontent.com/84674087/133156164-fa3372d1-bf4f-46a5-b1dd-e79427041888.png)

#### Solution

- Open WebDev Tool: `Control + Shift + E`
- Password: **ZluruAthQk7Q2MqmDeTiUij2ZvWy2mBi**

![grafik](https://user-images.githubusercontent.com/84674087/133156322-b89aec88-d8cc-49a8-8cf1-9c8017ed4a90.png)

<br />

## Natas2

#### Task

![grafik](https://user-images.githubusercontent.com/84674087/133156488-d55227b2-f89b-4b21-94e9-f6937be6b375.png)

#### Solution

- Open Source Code: `Control + U`
- Investigate directory `/files/`
- Take a look at `/files/user.txt`
- Password: **sJIJNW6ucpu6HPZ1ZAchaDtwd7oGrD14**

![grafik](https://user-images.githubusercontent.com/84674087/133156859-5fbeceae-4245-4fc9-804d-8b1d4fe3d72b.png)

![grafik](https://user-images.githubusercontent.com/84674087/133156942-396cebe3-40e4-47c9-80c7-f22afdbc0871.png)

![grafik](https://user-images.githubusercontent.com/84674087/133157019-cf70a75f-b942-4113-9cf9-0b4856b9da1b.png)

<br />

## Natas3

#### Task

![grafik](https://user-images.githubusercontent.com/84674087/133157139-ea4a3b16-6c6c-4859-984c-80ac5b0feb56.png)

#### Solution

- Open Source Code: `Control + U`

![grafik](https://user-images.githubusercontent.com/84674087/133630704-13412617-d4e1-47a4-8575-b9bb05982cc2.png)

- Enter: *http://natas3.natas.labs.overthewire.org/robots.txt*

![grafik](https://user-images.githubusercontent.com/84674087/133630834-c9ce6483-d91d-44a8-93ce-961acfc66353.png)

- Enter: *http://natas3.natas.labs.overthewire.org/s3cr3t/*
- Password: **Z9tkRkWmpt9Qr7XrR5jWRkgOU901swEZ**

<br />

## Natas4

#### Task

![grafik](https://user-images.githubusercontent.com/84674087/133631392-ef5eabe7-5f06-4294-90b4-fa21361059f2.png)

#### Solution

- Change the referrer accordingly, for example via BurpSuite:

![grafik](https://user-images.githubusercontent.com/84674087/133634576-0849712e-9c14-4541-bd2a-08a067f9cc4c.png)

- Password: **iX6IOfmpN7AYOQGPwtn3fXpbaJVJcHfq**

<br />

## Natas5

#### Task

![grafik](https://user-images.githubusercontent.com/84674087/133635013-71ae3aaf-90f5-455b-93a5-a28eb07b506c.png)

#### Solution

- Open WebDev Tool: `Control + Shift + E`
- Got to Storage, check Cookies
- Change Cookie value from 0 to 1
- Password: **aGoY4q2Dc6MgDq4oL4YtoKtyAg9PeHa1**

![grafik](https://user-images.githubusercontent.com/84674087/133635138-5d0b55bb-438a-475a-9298-1741496109ad.png)

<br />

## Natas6

#### Task

![grafik](https://user-images.githubusercontent.com/84674087/133635978-63058451-924f-4a2c-b4b8-1ba0008c752e.png)

#### Solution

- Sourcecode:
```

<?

include "includes/secret.inc";

    if(array_key_exists("submit", $_POST)) {
        if($secret == $_POST['secret']) {
        print "Access granted. The password for natas7 is <censored>";
    } else {
        print "Wrong secret";
    }
    }
?>
```

- Check out: *http://natas6.natas.labs.overthewire.org/includes/secret.inc*
- Hoewever, nothing is shown ...
- Check out sourcecode: `Control + U`
- There we go: **FOEIUWGHFEEUHOFUOIU** (secret)
- This reveals the password: **7z3hEENjQtflzgnT29q7wAvMNfZdh0i9**

<br />

## Natas7

#### Task

![grafik](https://user-images.githubusercontent.com/84674087/133637159-fa6492e5-d59a-44c5-9de2-e0d775aa3a75.png)

#### Solution

- Check out sourcecode: `Control + U`

![grafik](https://user-images.githubusercontent.com/84674087/133649993-a44b258d-6c37-45ee-a43e-e750d0f2ef56.png)

- Try to use the hint as parameter: *http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas8*
- This reveals the password: **DBfUBfqQG69KvJvJ1iAbMoIpwSNQ9bWe**

<br />

## Natas8

#### Task

![grafik](https://user-images.githubusercontent.com/84674087/133650338-90c43fce-223a-44b2-9c9d-3c0d31c3f2e2.png)

#### Solution

- Check out the sourcecode:

```
<?

$encodedSecret = "3d3d516343746d4d6d6c315669563362";

function encodeSecret($secret) {
    return bin2hex(strrev(base64_encode($secret)));
}

if(array_key_exists("submit", $_POST)) {
    if(encodeSecret($_POST['secret']) == $encodedSecret) {
    print "Access granted. The password for natas9 is <censored>";
    } else {
    print "Wrong secret";
    }
}
?>
```

- Following line must be reversed to encode the given $encodedSecret: `bin2hex(strrev(base64_encode($secret)))`
- One can use following website for testing: [www.w3schools.com/PHP](https://www.w3schools.com/PHP/phptryit.asp?filename=tryphp_func_string_bin2hex)

```
<?php 
$str = "3d3d516343746d4d6d6c315669563362";
$str = pack("H*", $str);
$str = strrev($str);
$str = base64_decode($str);
echo($str); 
?>
```

- The calculated secret: **oubWYf2kBq**
- Password: **W0mMhUcRRnG8dcghE4qvk3JA9lGt8nDl**

<br />

## Natas9

#### Task

![grafik](https://user-images.githubusercontent.com/84674087/133651714-d9400fed-be7c-4919-863e-aedbb71147c2.png)

#### Solution

- Sourcecode:
```

Output:
<pre>
<?
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    passthru("grep -i $key dictionary.txt");
}
?>
</pre>
```

- As long as $key is not empty, our search term is passed to **passthru** and then **grep**
- grep is searching inside dictionary.txt
- If we enter `""`, all content is displayed

![grafik](https://user-images.githubusercontent.com/84674087/133931581-e470f5db-708b-4c5b-9b8a-1d31dbedf08d.png)

- Let's check if we can dump content from other directories: `"" /etc/passwd`

![grafik](https://user-images.githubusercontent.com/84674087/133931617-9c6374f2-5d4e-4e61-8cc7-4c23aaf8e911.png)

- Consequently, search at the password location: `"" /etc/natas_webpass/natas10`

![grafik](https://user-images.githubusercontent.com/84674087/133931757-5a807dc7-b04b-4091-93b4-73fb5e3723ae.png)

- Password: **nOpp1igQAkUzaI1GUUjzn1bFVj7xCNzu**

<br />

## Natas10

#### Task

![grafik](https://user-images.githubusercontent.com/84674087/133931772-07dbff54-422e-4554-96ec-7f76994141b8.png)

#### Solution

- Sourcecode:

```

Output:
<pre>
<?
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    if(preg_match('/[;|&]/',$key)) {
        print "Input contains an illegal character!";
    } else {
        passthru("grep -i $key dictionary.txt");
    }
}
?>
</pre>
```

- Despite the existing **preg_match** filter regarding `; | &`, we can apply the same scheme as in Natas10

![grafik](https://user-images.githubusercontent.com/84674087/133932002-2f1c3798-071d-4c9e-a726-8f8274f30bcb.png)

- Password: **U82q5TCMMQ9xuFoI3dYX61s7OZD9JKoK**

<br />

## Natas11

#### Task

![grafik](https://user-images.githubusercontent.com/84674087/133932028-50261f64-df76-435f-a137-29c54cf1a7f3.png)

#### Solution

- Sourcecode:

```
<?

$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

function xor_encrypt($in) {
    $key = '<censored>';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}

function loadData($def) {
    global $_COOKIE;
    $mydata = $def;
    if(array_key_exists("data", $_COOKIE)) {
    $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
    if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
        if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
        $mydata['showpassword'] = $tempdata['showpassword'];
        $mydata['bgcolor'] = $tempdata['bgcolor'];
        }
    }
    }
    return $mydata;
}

function saveData($d) {
    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
}

$data = loadData($defaultdata);

if(array_key_exists("bgcolor",$_REQUEST)) {
    if (preg_match('/^#(?:[a-f\d]{6})$/i', $_REQUEST['bgcolor'])) {
        $data['bgcolor'] = $_REQUEST['bgcolor'];
    }
}

saveData($data);

?>

<?
if($data["showpassword"] == "yes") {
    print "The password for natas12 is <censored><br>";
}

?>
```

- The website points out that cookies are XOR encrypted
- After reviewing the sourcecode, we can see the cookie contains two information:
     - **showpassword** (default: `no`)
     - **bgcolor** (default: `#ffffff`)
- The following happens when we refresh the website:
     1. The defaultdata from Cookie is loaded
         - ` $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);`
     3. The variable `$_REQUEST['bgcolor']`, which we entered in the mask, gets verified and saved inside the cookie:
         - `setcookie("data", base64_encode(xor_encrypt(json_encode($d))));`
- We have to change the cookie-value of "showpassword" to **yes** in order to reveal the password
- But we don't know the key nor its length to crack the XOR-encryption ... 
- Well, we know how data looks during "setcookie" operation:
     - 1) data: `$d = array("showpassword" => "no", "bgcolor"=>"#ffffff");`
     - 2) json_encode: `{"showpassword":"no","bgcolor":"#ffffff"}`
     - 3) XOR-encryption
     - 4) base64_encode: `ClVLIh4ASCsCBE8lAxMacFMZV2hdVVotEhhUJQNVAmhSEV4sFxFeaAw=`

![grafik](https://user-images.githubusercontent.com/84674087/133932669-ca29d2fa-a5a3-4f8c-bc24-76b286728bca.png)

- And we know how the data should look like
     - 1) data: `$d = array("showpassword" => "yes", "bgcolor"=>"#ffffff");`
     - 2) json_encode: `{"showpassword":"yes","bgcolor":"#ffffff"}`
     - 3) XOR-encryption
     - 4) base64_encode: `???=`

- Let's see how the cookie value gets changed when we modify "bgcolor":

bgcolor | cookie
------- | -------
#ffffff | ClVLIh4ASCsCBE8lAxMacFMZV2hdVVotEhhUJQNVAmhS**EV4sFxFe**aAw%3D
#aaaaaa | ClVLIh4ASCsCBE8lAxMacFMZV2hdVVotEhhUJQNVAmhS**FlkrEBZZ**aAw%3D
#bbbbbb | ClVLIh4ASCsCBE8lAxMacFMZV2hdVVotEhhUJQNVAmhS**FVooExVa**aAw%3D

- 
