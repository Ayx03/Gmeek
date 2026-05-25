题目提示：
```
小0day:某编辑器最新版默认配置下，如果目录不存在，则会遍历服务器根目录
有时候源码里面就能不经意间泄露重要(editor)的信息,默认配置害死人
```

那我们肯定要知道用的是什么 editor 了，直接 Ctrl + U 查看网页源代码搜索 editor 发现这样一行代码：

`<img src="editor/upload/banner-app.png" alt="App">`

这张图片应该没什么特别的，访问上一级目录 `/editor/upload` 返回 403 Forbidden，再访问上一级目录发现显示一个网页编辑器，标题显示 KindEditor PHP，既然说会遍历服务器根目录我们就找到和文件相关的插入文件按钮

<img width="2349" height="1285" alt="Image" src="https://github.com/user-attachments/assets/10621370-3d99-4e7d-a992-6caaffb5562b" />

然后点击`文件空间`按钮看看能看到什么

<img width="2347" height="1285" alt="Image" src="https://github.com/user-attachments/assets/45c70197-ffb6-4ace-b249-35c8e7f02c30" />

好家伙，直接列出了很明显是一台 Linux 服务器根目录下的文件夹

<img width="2349" height="1285" alt="Image" src="https://github.com/user-attachments/assets/a11edfd4-e776-4aba-b065-af084e178361" />

`/home` 里面有个 `www-data` 应该是与网站有关的但是点不进去了

<img width="2349" height="1285" alt="Image" src="https://github.com/user-attachments/assets/5d08786b-256f-4d69-8e9c-7571ab8890ef" />

翻到 `/tmp` 发现里面有个 `flag.sh`

<img width="2349" height="1285" alt="Image" src="https://github.com/user-attachments/assets/b30b379e-2f07-43ea-b786-50e2599ca160" />

尝试插入文件提交内容但返回 404，尝试使用浏览器直接访问 `/editor/attached/file/tmp/flag.sh` 也只会返回默认首页，图片、CSS 等资源应该是因为使用了相对路径无法正常加载

<img width="2349" height="1285" alt="Image" src="https://github.com/user-attachments/assets/59971111-42ca-4b52-a4db-afa546e85389" />

<img width="2312" height="1290" alt="Image" src="https://github.com/user-attachments/assets/a3e0ce99-f7e6-4098-b55a-c9187038d261" />

最后发现通过文件空间访问 `/tmp/html` 里面有个文件夹叫 `nothinghere`，这不是此地无银三百两吗？打开里面果然有个 `fl000g.txt`，浏览器访问 `靶机域名/nothinghere/fl000g.txt` 获得 flag `ctfshow{6d516ff4-48c1-4677-8d81-9ea5ee82a215}`。

<img width="2350" height="1285" alt="Image" src="https://github.com/user-attachments/assets/ccc57def-9be7-4176-ae45-25a963dc82bc" />