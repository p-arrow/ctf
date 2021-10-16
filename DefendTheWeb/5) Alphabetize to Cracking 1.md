## Alphabetize / Coding

![grafik](https://user-images.githubusercontent.com/84674087/137523473-ee801f8e-4942-49e6-8e71-6ff53de095af.png)

#### Solution
- We have to use JavaScript (on this website), otherwise we get in trouble with the 5sec timer
     - Given time is too short to use an online tool or a script running on your computer 
- Open your WebDevTool and go to Console
- Start the challenge, enter below JS code in your console and push the submit button. SUCCESS ! 🙂


```
var text = document.getElementsByTagName("textarea")[0].innerHTML; // extract all jumbled words
var x = text.replaceAll(" ","");    // remove white space
x = x.split(",");                   // split the entire string into single words
x = x.sort();                       // then sort these words
x = x.join(", ");                   // join these words together as requested

console.log = function(message) {
    document.getElementsByTagName("textarea")[1].innerHTML = message;
};
console.log(x);                     // output the sorted words into the answer field
```

<br />

## Aliens / Stego

![grafik](https://user-images.githubusercontent.com/84674087/136847842-4b6d98a1-e422-42ba-b73d-dd60390a6ae0.png)

#### Solutions

<br />

## Custom encoding / Coding

![grafik](https://user-images.githubusercontent.com/84674087/137583606-917142ca-8eb8-4103-8fc8-c81fb60dbd86.png)

The custom encryption method:

![grafik](https://user-images.githubusercontent.com/84674087/137583647-5e8143eb-27bf-4c31-bc46-77d713091e94.png)

#### Solution
- Firstly, understand how the encryption works and then reverse the process to decrypt the given text
- I assemble the code in Javascript and enter it inside the console (open WebDevTool)

```
var x = document.getElementsByTagName("textarea")[0].innerHTML    // extract encoded numbers
x = x.split(",")        // split entire string into single strings
for (i = 0; i < x.length; i++) {
    if (x[i] != " "){
        y = x[i] - 32;  // re-number ASCII value
        y = 94 - y;     // total visible ASCII chars: 95 ... but you have to use 94 to get proper decoded text (?!)
        y = y + 32;     // return to original ASCII value
        y = String.fromCharCode(y); // convert integer into ASCII char
        document.getElementsByTagName("textarea")[1].innerHTML += y;
        }
    else {
        document.getElementsByTagName("textarea")[1].innerHTML += " ";
    }
} 
```

<br />

## Missile Codes / Forensics

![grafik](https://user-images.githubusercontent.com/84674087/137589545-a262053b-23fc-4602-bbf5-d3bbab253a44.png)

#### Solution

<br />

