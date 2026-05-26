题目提示：`mdb文件是早期asp+access构架的数据库文件，文件泄露相当于数据库被脱裤了。`

手工测试了以下文件名均未成功下载到数据库文件

```
database.mdb
databases.mdb
asp.mdb
1.mdb
www.mdb
web.mdb
db.mdb
sjk.mdb
```

于是打开了提示（Hint），原来是 `/db/db.mdb`，属实是没想到

下载后使用 Access 打开，在 Switchboard Items 表中就能找到 flag 了：`flag{ctfshow_old_database}`

<img width="1280" height="690" alt="Image" src="https://github.com/user-attachments/assets/4e748958-87d2-4bfb-88a6-3e527817e257" />

或者如果你没有安装 Access 的话用任意文本编辑器打开搜索 flag ctfshow 等关键词也能找到

<img width="2560" height="1381" alt="Image" src="https://github.com/user-attachments/assets/37f66af6-fcff-4165-b48c-26423e489380" />

<!--<img width="1280" height="690" alt="Image" src="https://github.com/user-attachments/assets/5a969cba-8ead-4f1c-b91a-67c9b208e7fc" />-->