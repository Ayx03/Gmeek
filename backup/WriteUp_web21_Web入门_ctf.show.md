使用 Burp Suite 的浏览器访问靶机，打开拦截后随便输入一个账号密码提交，把请求发送到攻击器（Intruder），在`Authorization: Basic `后面添加payload位置§比如`Authorization: Basic §userandpass§`，两个章节符号之间应该是相当于是变量名设置成什么无所谓反正这里就这一个，导入题目提供的密码字典，Payload处理添加前缀`admin:`然后Base64-encode，取消勾选下面的 URL 编码字符（否则payload Base64末尾的=会被URL编码无法正常工作），点击开始攻击

<img width="2560" height="1380" alt="Image" src="https://github.com/user-attachments/assets/024602af-1573-4d7c-8cb2-78ef2117b99d" />

按状态码排序找到唯一状态码为 `200` 即 `OK` 的请求，里面就返回了 flag `ctfshow{0cf9fadd-170b-4e9b-8793-f7677bd3c28d}`

<img width="2542" height="1370" alt="Image" src="https://github.com/user-attachments/assets/8c53365b-f8f8-4b47-a6cf-6d7bc894d017" />