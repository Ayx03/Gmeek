打开靶机发现是一个教务管理系统，点击`录取名单`链接即可下载到一个 Excel 表格 `list.xlsx`，里面有被录取的学生的姓名和隐去了第7-14位也就是生日部分的身份证号码，点击`学生学籍信息查询系统`发现可以使用姓名和身份证号查询，使用 Burp Suite Professional（Community Edition/社区版好像会限制 Intruder 的速度，不知道会不会太慢需要等太久）创建临时项目打开内置的 Chromium 浏览器随便输入一个学生的信息抓包，将其中的 POST 请求发送到 Intruder（Send to Intruder Ctrl+I）

<img width="2558" height="1524" alt="Image" src="https://github.com/user-attachments/assets/4df8b4ba-e3ab-4ec2-bb32-c1855f5ae144" />

把请求中身份证号生日部分选中点击 `Add §`（个人理解相当于创建一个变量），Payload Type（载荷类型）选择 Dates（日期），Format: `yyyyMMdd`，我这里为了保险起见设置的起（From）止（To）时间是从 1949 年 1 月 1 日到 2026 年 12 月 31 日，然后点击 `Start attack` 开始攻击

<img width="2560" height="1528" alt="Image" src="https://github.com/user-attachments/assets/5a01b21c-a78b-4335-b86f-2f935121033e" />

由于这个接口沟槽的设计即使输入的身份证号是错误的也会返回 HTTP 200 OK 状态码，所以攻击完成后我们通过按 Length 从大到小排序找到成功的长度为 `822` 的请求，发现对应的 Payload（载荷）是 `19900201`，因此`高先伊`同学正确的身份证号码应该是`621022199002015237`（免责声明：这应该是虚构的数据，如果这是您的真实身份证号，请发邮件到 privacy@imayx.top 联系我删除），我们输入这组信息去网页上查询（或者也可以使用 Unicode 转中文的工具转换返回值）得到消息：`恭喜您，您已被我校录取，你的学号为02015237 初始密码为身份证号码`

<img width="2560" height="1528" alt="Image" src="https://github.com/user-attachments/assets/eb623c56-2c92-4846-af1d-3ea4c9604f94" />

尝试使用这些信息在首页登录，提示`恭喜您，登陆成功!ctfshow{5dc94063-0253-4b5c-a0f0-a35ad9623a52}`，成功获得 flag。

<img width="2554" height="1465" alt="Image" src="https://github.com/user-attachments/assets/9530cc2a-39f4-4873-bf0b-5c74d3c23d3e" />