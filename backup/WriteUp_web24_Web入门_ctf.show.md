准备像上一题一样循环暴力破解，跑的时候卡住了跑不出来突然发现这不是固定种子随机数吗，直接把随机数算出来就行了，因为这种伪随机数算法对于同一个随机数种子来说每次算出的随机数都是相同的，在 URL 后拼接 `/?r=1155388967` 即可获得 flag `ctfshow{e0b5f9a7-dcc0-425a-bd48-f56f315ab76a}`
```php
<?php
    // for($i=0;;$i++) {
        if(true){
            $r = $i;
            mt_srand(372619038);
            echo intval(mt_rand());
            //if(intval($r)===intval(mt_rand())){
                //echo $r;
            //}
        }   
    // }
?>
```