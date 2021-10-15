## Alphabetize / Coding

![grafik](https://user-images.githubusercontent.com/84674087/137523473-ee801f8e-4942-49e6-8e71-6ff53de095af.png)

#### Solution
- We have to use JavaScript, otherwise we get in trouble with the 5sec timer
- Open your WebDevTool and go to Console
- Start the challenge, enter below JS code and push the submit button. SUCCESS ! 🙂


```
var text = document.getElementsByTagName("textarea")[0].innerHTML;
var x = text.replaceAll(" ","");
x = x.split(",");
var y = x.sort();
y = y.join(", ");

console.log = function(message) {
    document.getElementsByTagName("textarea")[1].innerHTML = message;
};
console.log(y);
```

<br />

## Aliens / Stego

![grafik](https://user-images.githubusercontent.com/84674087/136847842-4b6d98a1-e422-42ba-b73d-dd60390a6ae0.png)

#### Solutions

<br />
