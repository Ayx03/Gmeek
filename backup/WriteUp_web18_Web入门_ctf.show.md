题目提示：`不要着急，休息，休息一会儿，玩101分给你flag`

估计是一个游戏，需要修改分数获得 flag，当然你也可以就硬玩

靶机容器开好了，果然是类似安卓彩蛋的 Flappy Bird，很难操作，我们直接打开开发人员工具的源代码选项卡，在`js`目录里找到`Flappy_js.js`，发现101分给flag的逻辑：

```javascript
if(score>100)
{
var result=window.confirm("\u4f60\u8d62\u4e86\uff0c\u53bb\u5e7a\u5e7a\u96f6\u70b9\u76ae\u7231\u5403\u76ae\u770b\u770b");
}
```

<img width="2387" height="1375" alt="Image" src="https://github.com/user-attachments/assets/e78cf6a0-3d20-45d2-9521-c07f3012f0c4" />

把`window.confirm("\u4f60\u8d62\u4e86\uff0c\u53bb\u5e7a\u5e7a\u96f6\u70b9\u76ae\u7231\u5403\u76ae\u770b\u770b");`直接复制到控制台执行，提示`你赢了，去幺幺零点皮爱吃皮看看`

<img width="590" height="253" alt="Image" src="https://github.com/user-attachments/assets/131cdf11-f4a3-45c0-ae56-d290c778d4f5" />

访问 `靶机域名/110.php` 获得 flag `ctfshow{89a8220c-938e-4a54-848c-0625098fe266}`