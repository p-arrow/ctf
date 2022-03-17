## Captcha 1 / Captcha

![grafik](https://user-images.githubusercontent.com/84674087/138234030-3f8ec05d-1eef-4f17-a2a6-70933cdbb68b.png)

### Solution
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
        driver.refresh()
        
captcha()

```

<br />

## Captcha 2 / Captcha

![image](https://user-images.githubusercontent.com/84674087/158890464-4486b743-a5ff-4b64-b2e5-48b1ab11276f.png)

### Solution
```
import pytesseract
from selenium import webdriver
from PIL import Image
from PIL import ImageChops

def emoji2text():
    # crop image into equal parts, one emoji per cropped image
    for i in range(0,15):
        with Image.open("./captcha2.png") as im:
            width, height = im.size
            # 4-tuple (left, upper, right, lower)
            area = (5+(i*26), 0, 31+(i*26), height)
            im = im.crop(area)
            im = im.convert("L")
            im.save("./download/"+str(i)+".png")

    # compare cropped images with reference images and return answer-string
    ref = {1: ":D", 2: ":)", 3: ":p", 4: ":(", 5: ";)", 6: "B)", 7: ":@", 8: ":o", 9: ":s", 10: ":|", 11: ":/", 12: "<3"}
    string = ""

    for i in range (0,15):
        for j in range (1,13):
            image_one = Image.open("download/"+str(i)+".png")
            image_two = Image.open("references/"+str(j)+".png")

            diff = ImageChops.difference(image_one, image_two)
            if not diff.getbbox():
                string += ref[j]
    return string

def captcha():
    URL = "https://defendtheweb.net/playground/captcha2"
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
    for i in range (0,3):
        # Get captcha
        captcha = driver.find_elements(by="tag name", value='img')[1]
        captcha.screenshot("./captcha2.png")
        # Convert captcha to string
        string = emoji2text()
        # Input answer
        driver.find_element(by="id", value="answer").send_keys(string)
        driver.execute_script('document.forms[1].submit();')
        driver.refresh()

captcha()
```

## Captcha 3 / Captcha

![image](https://user-images.githubusercontent.com/84674087/158890970-a6e9b8e2-dc87-4809-a487-54be2e034dcc.png)

### Solution
