```php
<?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-03 11:43:51
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-03 11:56:11
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/
error_reporting(0);

include('flag.php');
if(isset($_GET['token'])){
    $token = md5($_GET['token']);
    if(substr($token, 1,1)===substr($token, 14,1) && substr($token, 14,1) ===substr($token, 17,1)){
        if((intval(substr($token, 1,1))+intval(substr($token, 14,1))+substr($token, 17,1))/substr($token, 1,1)===intval(substr($token, 31,1))){
            echo $flag;
        }
    }
}else{
    highlight_file(__FILE__);

}
?>
```
因为这题属于爆破考虑直接改造题目提供的代码暴力计算从1开始的自然数的md5找到能通过校验获取 flag 的值，因为对 PHP 不熟悉加上很久没有自己手写代码了借助 AI 分析报错原因和修正代码逻辑三轮才写出能用的代码，得到 token=422 时可以通过条件检查，在URL后拼接 /?token=422 即可获得 flag `ctfshow{4906bbb7-13b6-442a-87a9-e8576d70a0e3}`。另外试了一下很明显直接访问 /flag.php 是无法获得 flag 的，别想了（
```php
<?php
for($i=0;;$i++) {
    if(true){
        $token = md5($i);
        if(substr($token, 1,1)===substr($token, 14,1) && substr($token, 14,1) ===substr($token, 17,1)){
            if((intval(substr($token, 1,1))+intval(substr($token, 14,1))+substr($token, 17,1))/substr($token, 1,1)===intval(substr($token, 31,1))){
                echo $i;
            }
        }
    }
}
?>
```