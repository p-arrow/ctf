## SQLi 2 / SQLi

![grafik](https://user-images.githubusercontent.com/84674087/134689346-87c189a9-924e-4729-833a-d9a5a92674cf.png)

#### Solution
- When we click through the member alphabet we notice the parameter `?q=` in the URL:
![grafik](https://user-images.githubusercontent.com/84674087/134702569-65fce3cf-1e6e-402f-b9da-fef40acd5d7d.png)

- Being at section "D": We extend D to Derun (one available user) in the parameter
- The output is Derun only:

![grafik](https://user-images.githubusercontent.com/84674087/134702827-f2750dfb-2ea1-4875-8fea-8d3077c8a5af.png)

- We try to further manipulate the parameter by writing `Derun'`:
- The output is a Debug info:

![grafik](https://user-images.githubusercontent.com/84674087/134702341-cbeb29aa-9e7c-4116-a73e-1e5f4e86edfd.png)

