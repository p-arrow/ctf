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
y = y.join(", ");                   // join these words together as requested

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
