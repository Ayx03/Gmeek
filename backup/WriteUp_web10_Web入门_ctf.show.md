题目提示：`cookie 只是一块饼干，不能存放任何隐私数据`

推测 Cookies 中直接存放了 flag，按下 F12 或 Ctrl + Shift + I 打开浏览器的开发人员工具（DevTools），切换到应用程序（Application）选项卡，在左侧列表的`存储`下面找到 `Cookie`，发现有名为 `flag` 的 Cookie 值为 `ctfshow%7Bf4ccc0fe-2a23-464b-9f16-2558e4c3b508%7D`，在下方的 `Cookie Value` 中勾选`显示已解码的 URL`后即可获得 flag `ctfshow{f4ccc0fe-2a23-464b-9f16-2558e4c3b508}`，或者也可以手动把`%7B`替换为`{`，把`%7D%`替换为`}`，这应该就是所谓的 URL 编码，在浏览器的地址栏中输入这两个符号回车也可以发现会被自动编码。

<img width="2079" height="893" alt="Image" src="https://github.com/user-attachments/assets/c86b04aa-e8d8-4407-9521-462257b3778d" />