题目提示：`密钥什么的，就不要放在前端了`

推测某个密钥放在前端了

查看网页源代码可以发现密码使用AES CBC ZeroPadding加解密，密钥和偏移量以及正确密码的加密结果也都暴露在了前端，虽然非常鸡贼地把存放密码地变量从 `password` 改成了 `pazzword`，但是因为 index 代码量实在太少了一眼就能发现，这里也能看到账号是 admin

```html
<script type="text/javascript">
    function checkForm(){
        var key = "0000000372619038";
        var iv = "ilove36dverymuch";
        var pazzword = $("#pazzword").val();
        pazzword = encrypt(pazzword,key,iv);
        $("#pazzword").val(pazzword);
        $("#loginForm").submit();
        
    }
    function encrypt(data,key,iv) { //key,iv：16位的字符串
        var key1  = CryptoJS.enc.Latin1.parse(key);
        var iv1   = CryptoJS.enc.Latin1.parse(iv);
        return CryptoJS.AES.encrypt(data, key1,{
            iv : iv1,
            mode : CryptoJS.mode.CBC,
            padding : CryptoJS.pad.ZeroPadding
        }).toString();
    }

</script>
    <!--
    error_reporting(0);
    $flag="fakeflag"
    $u = $_POST['username'];
    $p = $_POST['pazzword'];
    if(isset($u) && isset($p)){
        if($u==='admin' && $p ==='a599ac85a73384ee3219fa684296eaa62667238d608efa81837030bd1ce1bf04'){
            echo $flag;
        }
}
    -->
</html>
```

`aes.js` 的注释中有加解密函数，我试图直接在控制台中调用这个函数解密密码但是不知道 cfg 是什么，就放弃了

```javascript
    /**
     * Shortcut functions to the cipher's object interface.
     *
     * @example
     *
     *     var ciphertext = CryptoJS.AES.encrypt(message, key, cfg);
     *     var plaintext  = CryptoJS.AES.decrypt(ciphertext, key, cfg);
     */
```

然后使用找到的第三个 AES 网页工具（<https://www.mklab.cn/utils/aes>）解密出了密码，使用账号 `admin` 密码 `i_want_a_36d_girl` 登录获得 flag `ctfshow{acf28a70-65c0-49ba-9935-021cffa01a25}`。

<img width="2550" height="1375" alt="Image" src="https://github.com/user-attachments/assets/f5a9299a-a638-4666-87a8-b042a4b6243a" />

其实我一开始根本没注意到它用的是 CBC 还是什么加密模式，单纯是这个工具默认选择的 ECB 无法输入偏移量，而 ECB 下面的 CBC 就可以输入，如果 CBC 不行我估计会把剩下三种都试一遍吧...

另外其实根本不需要把密码解密出来，我们只需要打开开发人员工具的网络选项卡然后随便提交一个用户名和密码就能看到是通过 POST 提交的，Body格式如下，后面一长串很明显直接就是加密过的密码

```username=admin&pazzword=a599ac85a73384ee3219fa684296eaa62667238d608efa81837030bd1ce1bf04```

所以我们直接使用 HackBar 或者 cURL 或者 Burp Suite 提交一个包含 index 中注释掉的代码里包含的密码密文的请求就可以获得 flag 了

<img width="2387" height="1375" alt="Image" src="https://github.com/user-attachments/assets/7391f175-9e52-432f-bb91-9ec55fdbd5b3" />