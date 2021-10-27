## Captcha 1 / Captcha

![grafik](https://user-images.githubusercontent.com/84674087/138234030-3f8ec05d-1eef-4f17-a2a6-70933cdbb68b.png)

#### Solution
```
//Access captcha.php image
var captcha = document.getElementsByTagName("img")[1]

//extract string from image
//tesseract.js is popular on that, referring to OCR [Optical Character Recognition])
//But I am to solve this task inside the console with JS only
captcha ...

//Reverse string
captcha = captcha.split('').reverse().join('')

//Enter reversed string in answer field
document.getElementById("answer").value="captcha";

//Press submit button
document.forms[1].submit()
```


<br />
