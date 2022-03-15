## Captcha 1 / Captcha

![grafik](https://user-images.githubusercontent.com/84674087/138234030-3f8ec05d-1eef-4f17-a2a6-70933cdbb68b.png)

#### Solution
- My first thought
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

- The final result
```
import pytesseract, requests, shutil
from selenium import webdriver

def img2text():
    pytesseract.pytesseract.tesseract_cmd = r'/usr/bin/tesseract'
    result = pytesseract.image_to_string('captcha1.png')
    result = result[::-1].replace(" ","")
    return result

def captcha():
    URL = "https://defendtheweb.net/playground/captcha1"
    # Setup Selenium
    driver = webdriver.Firefox()
    # Credentials
    username = "xxx"
    password = "yyy"
    # head to login page
    driver.get(URL)
    # find username/email field and send the username itself to the input field
    driver.find_element(by="id", value="login-username").send_keys(username)
    # find password input field and insert password as well
    driver.find_element(by="id", value="login-password").send_keys(password)
    # click login button
    driver.find_element(by="tag name", value='button').click()
    # click Dismiss button
    driver.find_element(by="link text", value='Dismiss').click()
    # Trial & Error 
    for i in range (0,10):
        # Get Captcha
        captcha = driver.find_elements(by="tag name", value='img')[1]
        captcha.screenshot("./captcha1.png")
        # Convert captcha to string
        string = img2text()
        # Input answer
        driver.find_element(by="id", value="answer").send_keys(string)
        driver.execute_script('document.forms[1].submit();')

captcha()

```

<br />
