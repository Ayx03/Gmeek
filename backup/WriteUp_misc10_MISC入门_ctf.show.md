```
┌──(kali㉿AyxGaming)-[~]
└─$ binwalk -e '/mnt/d/CTF/misc10.png'

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
1382          0x566           Zlib compressed data, default compression
4325          0x10E5          Zlib compressed data, default compression

WARNING: One or more files failed to extract: either no utility was found or it's unimplemented


┌──(kali㉿AyxGaming)-[~]
└─$ ls
_easySteg0.jpg.extracted  _misc10.png.extracted

┌──(kali㉿AyxGaming)-[~]
└─$ cd _misc10.png.extracted/

┌──(kali㉿AyxGaming)-[~/_misc10.png.extracted]
└─$ l
10E5  10E5.zlib  566  566.zlib

┌──(kali㉿AyxGaming)-[~/_misc10.png.extracted]
└─$ cat 10E5
ctfshow{353252424ac69cb64f643768851ac790}
```