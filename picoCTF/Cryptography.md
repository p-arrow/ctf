## Mod 26

![grafik](https://user-images.githubusercontent.com/84674087/141133886-c0a14b98-6472-446c-ac67-23ee67445d08.png)

#### Solution
- `echo "cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_Ncualgvd}" | tr a-zA-Z n-za-mN-ZA-M`
- **FLAG**: picoCTF{next_time_I'll_try_2_rounds_of_rot13_Aphnytiq}

<br />

## Mind your Ps and Qs

![grafik](https://user-images.githubusercontent.com/84674087/141134513-dc00a06d-7d34-4fcd-8e6d-cd584282966a.png)

#### Solution
- Download "values"
- Open the file via `nano values`

![grafik](https://user-images.githubusercontent.com/84674087/141134926-f7992332-aba9-4861-97b3-9247958f1551.png)

- N = modolus
- e = encryption key
- c = cipher message
- Check out RsaCtfTool.py: (https://github.com/Ganapati/RsaCtfTool)[ëhttps://github.com/Ganapati/RsaCtfTool]
- This tool can help to decode the cipher message **c** without having the decyrption key **d**
- After installation of RsaCtfTool.py enter following code:

```python3 RsaCtfTool.py -n 769457290801263793712740792519696786147248001937382943813345728685422050738403253 -e 65537 --uncipher 8533139361076999596208540806559574687666062896040360148742851107661304651861689```

- **FLAG:** picoCTF{sma11_N_n0_g0od_45369387}

![grafik](https://user-images.githubusercontent.com/84674087/141141522-8bf6261e-0764-4314-9de7-be807c4bead3.png)


<br />
