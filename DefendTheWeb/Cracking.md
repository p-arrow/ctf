## Cracking 1 / Cracking

![grafik](https://user-images.githubusercontent.com/84674087/137898570-c9617660-c36f-4b71-8c83-16e2433e40a0.png)

#### Solution
- To get a better understanding of the program flow and structure we need a Revere Engineering tool
- There are several tools available, but for now we will take a look at **ollydbg** only
- If not yet installed: `sudo apt install ollydbg`
- Initial info: [https://www.kali.org/tools/ollydbg/](https://www.kali.org/tools/ollydbg/)

1. Load cracking1.exe into ollydbg
2. Right click in the main thread, select "search for" and then "all referenced text strings". This brings us to this overview:

![grafik](https://user-images.githubusercontent.com/84674087/138116134-0b1fcec7-1fe6-4cec-9947-358ba2a5d337.png)

3. Before the welcome text `Hello challengers...` starts we identify the name **kevin**. It might be a username?

![image](https://user-images.githubusercontent.com/84674087/159350890-1165215a-b663-4806-9b6f-ccbd8978cf77.png)

4. We place breakpoints after the input of username and password. It's recommended to do so at the line with `CMP`
5. At `REPE CMPS` strings from `EDI` and `ESI` are compared repetitive. Namely our entered "kevin" and the hardcoded string "kevin". Look at Registers `ESI` and `EDI` on the right hand side

![image](https://user-images.githubusercontent.com/84674087/159351712-de0570b1-ac48-4d70-9daf-746470aa9aed.png)

6. At the next breakpoint most probably password comparison takes place. The value `200B2` and the pointer at `ESP+14` are of interest
7. At the register window we do right-click on `ESP` and select `Follow in Dump` to see what `ESP` contains

![image](https://user-images.githubusercontent.com/84674087/159351823-0ec8d1eb-cf19-4d71-af1c-8e7fffb1a7a1.png)

8. At the dump window we see the content of `ESP` address `0060FE30` 
9. We recognize the string **JE74toJNZ75** at address `0060FE54` 🙂

<br />

##  Variable switcheroo / Cracking

![image](https://user-images.githubusercontent.com/84674087/159357453-1feff0fb-f9c5-40dd-99ba-b73a6de0b8bd.png)

## Solution

![image](https://user-images.githubusercontent.com/84674087/159357237-837d1bfd-1ae7-4a06-8cb1-192c22cf4af4.png)

![image](https://user-images.githubusercontent.com/84674087/159363773-b855f7bc-3576-4007-bc74-d28a1acb8388.png)

