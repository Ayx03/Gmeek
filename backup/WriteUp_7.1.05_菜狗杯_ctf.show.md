从QQ群获得游戏后打开选择`载入游戏`，发现提示`你尚未保存任何游戏。`，搜索得知游戏存档路径在 `%homepath%\Documents\My Games\Capitalism Lab\SAVE`，尝试将`flagInHere.zip`中解压出的`flagInHere.SAV`文件放入即可读取。加载存档可以用发现玩家叫做`long_flag_in_R&D`，`R&D`应该就是`Research and Development`的缩写，也就是和研发有关，把地图上的研发中心简单翻一下发现是一行行按顺序排列的，每个研发中心可以有`0-9`个部门，把每个研发中心的部门数量记录下来得到`9794598612147726669494087179782678475623253058262173164497949649813569030779924086502049160804`这个长数字，编写 Python 脚本使用`long_to_bytes`函数并逆序处理得到flag `ctfshow{Cap1tal1sm_Lab_1s_S0_G00d_Gam3}`

```py
from Crypto.Util.number import long_to_bytes
num = 9794598612147726669494087179782678475623253058262173164497949649813569030779924086502049160804
print(long_to_bytes(int(str(num)[::-1]))[::-1])
```

<img width="2549" height="1478" alt="Image" src="https://github.com/user-attachments/assets/a586f856-da5c-444a-ace3-8b3600825309" />

<img width="1226" height="718" alt="Image" src="https://github.com/user-attachments/assets/c90c9d57-a8f6-4bac-b318-df845a9db3da" />

<img width="2549" height="1440" alt="Image" src="https://github.com/user-attachments/assets/e78cde7f-e509-4541-9c66-0e10cf4e050a" />