题目提示`爆个🔨，不爆了`，所以这题还是得智取
```php
<?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-03 13:56:57
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-03 15:47:33
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/


error_reporting(0);
include("flag.php");
if(isset($_GET['r'])){
    $r = $_GET['r'];
    mt_srand(hexdec(substr(md5($flag), 0,8)));
    $rand = intval($r)-intval(mt_rand());
    if((!$rand)){
        if($_COOKIE['token']==(mt_rand()+mt_rand())){
            echo $flag;
        }
    }else{
        echo $rand;
    }
}else{
    highlight_file(__FILE__);
    echo system('cat /proc/version');
}
```
随机数种子是 $flag 的 md5 值的前 8 位的 16 进制转 10 进制，$rand 是输入的 r 的值减去生成的第一个随机数，需要COOKIE中的token==第二和第三个随机数才会显示flag，否则只会显示 $rand。

问了一下 Gemini 得知有 [php_mt_seed](https://github.com/openwall/php_mt_seed) 这样一款工具可以反向爆破出 php 的随机数种子，先通过传入 ?r=0 获取随机数，得到 `-1514623366`，由于 
```php
$rand = intval($r)-intval(mt_rand());
```
当 `intval($r) = 0` 时 `$rand = -intval(mt_rand())`
故实际的随机数为 `1514623366`。（实际上就算你没发现这点 php_mt_seed 也不支持你输入负数）

由于使用了不支持的头文件在 Windows 下使用 MinGW64 中的 gcc 似乎无法正常编译
```
.\gcc.exe "C:\Users\Ayx\Downloads\php_mt_seed-main\php_mt_seed.c"
C:\Users\Ayx\Downloads\php_mt_seed-main\php_mt_seed.c:15:10: fatal error: sys/times.h: No such file or directory
   15 | #include <sys/times.h>
      |          ^~~~~~~~~~~~~
compilation terminated.
```
在 Kali Linux 下使用 git clone 拉取 GitHub 仓库并使用 make 命令编译，不要直接使用 gcc 编译，应该是没有开启 O2 优化之类的会导致运行效率非常差。
```
┌──(ayx㉿AyxPower)-[~]
└─$ git clone https://github.com/openwall/php_mt_seed.git
Cloning into 'php_mt_seed'...
remote: Enumerating objects: 119, done.
remote: Counting objects: 100% (119/119), done.
remote: Compressing objects: 100% (69/69), done.
remote: Total 119 (delta 65), reused 103 (delta 50), pack-reused 0 (from 0)
Receiving objects: 100% (119/119), 55.62 KiB | 1.50 MiB/s, done.
Resolving deltas: 100% (65/65), done.

┌──(ayx㉿AyxPower)-[~]
└─$ cd php_mt_seed/

┌──(ayx㉿AyxPower)-[~/php_mt_seed]
└─$ ls
Makefile  php_mt_seed.c  README

┌──(ayx㉿AyxPower)-[~/php_mt_seed]
└─$ make
gcc -Wall -march=native -mtune=generic -O2 -fomit-frame-pointer -funroll-loops -fopenmp php_mt_seed.c -o php_mt_seed
php_mt_seed.c:47:2: warning: #warning AVX-512 not enabled. Try gcc -mavx512f (on Intel Knights Landing, Skylake-X, or some newer). [-Wcpp]
   47 | #warning AVX-512 not enabled. Try gcc -mavx512f (on Intel Knights Landing, Skylake-X, or some newer).
      |  ^~~~~~~
```
（warning 是因为我的 Ryzen 7 7435H CPU 不支持 AVX-512）
如果你直接使用 gcc 编译的话就会这样：
```
┌──(ayx㉿AyxPower)-[~]
└─$ ./php_mt_seed.out 1514623366
Pattern: EXACT
Version: 3.0.7 to 5.2.0
Found 0, trying 0x78000000 - 0x7bffffff, speed 130.5 Mseeds/s
seed = 0x794cd53c = 2035078460 (PHP 3.0.7 to 5.2.0)
seed = 0x794cd53d = 2035078461 (PHP 3.0.7 to 5.2.0)
Found 2, trying 0xfc000000 - 0xffffffff, speed 129.6 Mseeds/s
Version: 5.2.1+
Found 2, trying 0x08000000 - 0x09ffffff, speed 1.1 Mseeds/s ^C
```

编译出可执行文件后给它加上可执行权限（chmod +x），然后调用它爆破随机数种子
```
┌──(ayx㉿AyxPower)-[~/php_mt_seed]
└─$ chmod +x php_mt_seed

┌──(ayx㉿AyxPower)-[~/php_mt_seed]
└─$ ./php_mt_seed
Usage: ./php_mt_seed VALUE_OR_MATCH_MIN [MATCH_MAX [RANGE_MIN RANGE_MAX]] ...

┌──(ayx㉿AyxPower)-[~/php_mt_seed]
└─$ ./php_mt_seed 1514623366
> ^C

┌──(ayx㉿AyxPower)-[~/php_mt_seed]
└─$ ./php_mt_seed 1514623366
Pattern: EXACT
Version: 3.0.7 to 5.2.0
Found 0, trying 0x78000000 - 0x7bffffff, speed 20132.7 Mseeds/s
seed = 0x794cd53c = 2035078460 (PHP 3.0.7 to 5.2.0)
seed = 0x794cd53d = 2035078461 (PHP 3.0.7 to 5.2.0)
Found 2, trying 0xfc000000 - 0xffffffff, speed 24869.8 Mseeds/s
Version: 5.2.1+
Found 2, trying 0x94000000 - 0x95ffffff, speed 376.2 Mseeds/s
seed = 0x95820833 = 2508326963 (PHP 7.1.0+)
Found 3, trying 0xfe000000 - 0xffffffff, speed 366.1 Mseeds/s
Found 3
```
题目的代码输出了 `/proc/version` 文件的内容，包含这些版本信息：
```
Linux version 5.4.0-163-generic (buildd@lcy02-amd64-067) (gcc version 9.4.0 (Ubuntu 9.4.0-1ubuntu1~20.04.2)) #180-Ubuntu SMP Tue Sep 5 13:21:23 UTC 2023
```
根据 Gemini 的说法 `Ubuntu 20.04 官方 apt 软件源默认提供的 PHP 版本是 PHP 7.4`，所以我们选择使用找到的三个随机数种子中的最后一个 `2508326963`（PHP 7.1.0+），由于浏览器中的 PHP Playground 基于 wasm 之类的原因是 32 位的，输入超过 32 位 int 类型上限的数字作为随机数种子会报错，所以使用本地的 64 位 php 运行代码计算第一个随机数和第二个、第三个随机数的和。
```
PS C:\phpstudy_pro\Extensions\php\php7.3.4nts> .\php.exe -r "mt_srand((int)2508326963);echo mt_rand();echo '|';echo mt_rand()+mt_rand();"
1514623366|2335728148
```
在 URL 后拼接 `/?r=1514623366`（第一个随机数），点击 HackBar 中的 MODIFY HEADER 并将 Cookie 设为 `token=2335728148`（第二个和第三个随机数的和），点击 Execute 即可获得 flag `ctfshow{7e19dc1e-0d07-47ce-a791-29b87681650c}`。

<img width="1062" height="375" alt="Image" src="https://github.com/user-attachments/assets/07ff1d0c-a475-4783-a1a9-c315cfe9b53c" />

```
