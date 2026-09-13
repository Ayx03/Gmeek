打开靶机发现 302 跳转到`/0/1/2.txt`，页面只有一句话：`web28:where is flag?`，Ctrl+U查看源代码也只有这一句话，题目提示`大海捞针`，访问任何其他文件似乎都会触发无限 302 跳转一直在 URL 中间拼接 `/0/1`，触发 Chromium 浏览器的 ERR_TOO_MANY_REDIRECTS（网站将您重定向的次数过多。）错误，即使手动重新加载直到触发 nginx/1.20.1 服务器的 414 Request-URI Too Large 错误也不会有任何结果（鬼知道我为什么觉得只要一直重新加载下去就会有结果，我可能觉得万一呢），没招了看了下 Hint：

```
通过暴力破解目录/0-100/0-100/看返回数据包

爆破的时候去掉2.txt 仅仅爆破目录即可
```

```
ctfshow{d0460185-d3c3-4120-bade-f48e0de74884}
```


<img width="2560" height="1528" alt="Image" src="https://github.com/user-attachments/assets/1a1ba2c3-35e9-4f47-a332-8337eb4c71a7" />