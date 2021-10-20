## Cracking 1 / Cracking

![grafik](https://user-images.githubusercontent.com/84674087/137898570-c9617660-c36f-4b71-8c83-16e2433e40a0.png)

#### Solution
- `file cracking1.exe`: Identify exact file type
   - PE32 executable (console) Intel 80386 (stripped to external PDB), for MS Windows
- `ldd cracking1.exe`: Print shared object dependencies
   -  Since we talk about a .exe file we only get "not a dynamic executable" as response
- `nm cracking1.exe`: searches the executable/binary for important data structures and functions
   - no symbols (because cracking1.exe is stripped unfortunately)
- `binwalk cracking1.exe`: extract data from binary

![grafik](https://user-images.githubusercontent.com/84674087/137899539-e644a943-9233-4678-b7ec-e1360436284c.png)

- `strings -n 6 cracking1.exe`: Looks for strings

![grafik](https://user-images.githubusercontent.com/84674087/137901419-9841a700-c690-4ee2-ae01-533dab4d8073.png)

- `objdump cracking1.exe -s`: Display information from object by resembling assembler instructions
   - `-s` = full contents 
   - not much new output  
- `objdump cracking1.exe -d`: Display 
   - `-d` = disassemble 

