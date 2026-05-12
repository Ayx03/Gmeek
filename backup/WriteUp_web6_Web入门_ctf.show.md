题目提示`解压源码到当前目录，测试正常，收工`，感觉暗示源代码压缩包泄露，试了 `main.zip`、`web.zip`、`src.zip` 最后发现文件名是 `www.zip`，下载到源码压缩包后打开发现 `fl000g.txt`，打开文件发现 `flag{flag_here}`。

忘记加 Label 了，修改一下触发重新部署（