今天发现 Edge 的 UA 就在末尾多了个 `Edg/142.0.0.0` 居然在抖音网页版观看视频时不能获取 4K 的画质，而 Brave 可以，之前使用 Firefox 浏览器的时候也遇到过这种问题，现在应该也适用于 Firefox
使用[User-Agent Switcher and Manager](https://chromewebstore.google.com/detail/user-agent-switcher-and-m/bhchdcejhohfmigjafbampogmaanbfkg)伪装支持4K的User Agent就会有4K，下面是我测试过能获得 4K 画质选项的 UA，来自 `Brave 1.84.141 Chromium: 142.0.7444.176 (正式版本) （64 位）`

Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36

打开设置切换到自定义模式并粘贴以下内容并滚动到页面底部保存即可，可以通过 <https://check.retiehe.com/> 检查是否生效

```
{
  "check.retiehe.com": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36",
  "www.douyin.com": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36"
}
```

<img width="901" height="766" alt="Image" src="https://github.com/user-attachments/assets/9414e675-f81f-4ff6-a887-a019b0f5840d" />

<img width="1533" height="1567" alt="Image" src="https://github.com/user-attachments/assets/64c9c99d-d508-4a3f-b454-c511dc57c83a" />