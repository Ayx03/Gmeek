## 前言
如果您的手机正处在无限重启（Bootloop）的状态，请看[这一节（点击跳转）](#%E5%AF%B9%E4%BA%8E-bootloop%E6%97%A0%E9%99%90%E9%87%8D%E5%90%AF%E9%A6%96%E5%85%88%E5%B0%9D%E8%AF%95%E7%A1%AC%E6%A0%BC)来了解解决方案。

如果您使用的是其他型号或版本的手机（甚至可以不是 LG 的手机），本教程也可能部分（但不太可能完全）适用于您。

本教程旨在整合其他资源和教程以节省韩版 LGU+ 的 LG V50 ThinQ 用户的时间，因此如果您的机器版本或型号不同，它可能浪费您的时间，甚至对您的设备造成不可挽回的损坏。

无论如何，请谨慎操作，尤其是在 Download 和 9008 模式下进行操作时一定要三思而后行，选用可靠的数据线，保持耐心，因为它们造成的损坏很可能无法通过简单地在软件层面进行操作来恢复。

## 拆箱收货
笔者建议您签收贵重物品前先与快递员当面验货，或在快递柜旁拆开，以避免不必要的纠纷。

不过，笔者自己从来不这么做。

如果卖家声称您购买的机器有以下特性，您可以用下文所述的方法验证他们的描述是否属实。

### 气密性

（该部分适用于其他出厂时支持 IP68 防水且有气压传感器的设备）

普遍的检测方法是使用 DevCheck 显示气压传感器的数值（其实系统自检中也可以查看压力传感器的数值，但是我更喜欢使用 DevCehck）

DevCheck 的包名：`flar2.devcheck`

蓝奏云下载：<https://cpk.lanzoui.com/iZdritwxo8b>

- 打开传感器中的压力传感器

- 堵住右下角扬声器左侧的小孔（对于 LG G8 ThinQ，这个小孔在左上角；别问我为什么知道，因为我也用过）

- 用力按压屏幕后松开（小心别把屏幕压碎了，适可而止；别问我为什么要提一嘴，因为我压碎过）。

- 如果气压值的波动大于 `10 hPa`，那么这台手机很可能有气密性，可以下水。

气密性好的组装机波动甚至能达到 `40 hPa`，因此是否有气密性不应作为判断手机是否原装的标准。

但是请注意，不要将手机置于热水或蒸汽中。防水功能防水溅不防手贱，不建议经常性主动下水，也不要跟我一样想出水下跑分或者半浸没充电这种操作。

### 完美屏

一般指显示和触摸功能完美（没有彩点、黑块或绿线黑线），全原装非后压屏的机器外屏没有任何划痕的应该很少。

## 获取 IMEI 并记录，查询手机版本
得到 IMEI 的几种方法：

- 正常进入系统时，打开拨号键盘输入 `*#06#`；

- 正常开机进系统后在 设置-关于手机 中查看；

- 关机后长按开机键、音量上下键，同时按住。

- 查看手机背面的标签、包装盒上的标签（二手机可能没有）；

记录好 IMEI 后可以[在这里](https://www.imeidb.com/imei/lg)使用它查询来获得关于这台手机很多有用的信息，包括但不限于运营商（简称）、国家、区域、CPU 类型、手机型号、颜色、S/N 码、CSN 码、生产与出厂日期，但 KDZ 相关的四项信息似乎总是空白或没有。

查询手机的型号和版本就是为了下载对应版本的 KDZ（类似于其他安卓手机的线刷包，不懂也不用慌），避免浪费时间。

<http://www.lgbbs.com/lg_mobile-lg_mobile.html>

（这个地址似乎暂时无法访问了）

## 升级 Magisk 版本
此部分适用于收到时卖家已帮您刷好旧版 Magisk 的机器。

旧版 Magisk 除了用于授予应用 Root 权限外能刷入的模块非常有限，因此我们需要升级 Magisk 版本。

推荐先升级 Magisk Manager（新版去掉了 Manager 的后缀），再升级 Magisk 本体。

但是，先别急着使用直接安装升级 Magisk，这很可能导致您的设备陷入 BootLoop（无限重启）的状态中（如果您甚至无聊到启用了安全启动），而且您还有可能完全不知道怎么让它停下来。

正确的做法应该是把新版 Magisk 的 .apk 文件扩展名改为 .zip，然后以安装模块的方式在 Manager 中刷入。

## 破解 VoLTE
我还没成功...

## 破解 5G
卖家已经帮你破解好了 VoLTE，移动卡电信卡都能边打电话边上网了（如果没有破解 VoLTE，电信卡无法正常使用），但你还听说这是一台 5G 手机？

其实，它似乎只支持联通与电信的 5G 频段。并且它只支持 NSA（非独立组网模式）而不支持 SA（独立组网模式）。

### 我没有开通 5G 套餐，也没有换新的 USIM 卡，可以用 5G 吗？

不存在只支持 4G 而不支持 5G 的 UIM 卡，并且未开通 5G 套餐也可以连接 5G 网络，只不过速度会被限制在 300 Mbps。开通 5G 套餐后可享受 500 Mbps 或 1 Gbps 甚至高达 4 Gbps 的速率。

能不能成功，一试便知。

下载 QPST TOOL 并安装，同意许可协议一路 Next 最后 Finish 即可。LG 驱动同理。

<https://cpk.lanzoui.com/i0cvUtwz3qf>

<https://cpk.lanzoui.com/i8Edvtwz7be>

替换用文件，记得去掉 .txt 的后缀，笔者加上此后缀是因为蓝奏云不支持上传 .xml 格式的文件。

<https://cpk.lanzoui.com/iDTmPtwza4f>

电脑下载文件的同时，手机上也可以操作起来了。

- 打开拨号键盘，输入 *#546368#*500#

- 进入 HiddenMenu（隐藏菜单）

- SVC Menu - LDB - USB debugging 打钩

- SVC Menu - Port Check Test 选择 Enabled

- 设置 - 系统 - 关于手机 - 软件信息 - 内部版本号 - 连续点击7次 - 您现在是开发人员！

- 设置 - 系统 - 开发人员选项 - 开启 - 启动 OEM解锁 - 启用

- 设置 - 系统 - 开发人员选项 - 开启 - USB调试 - 确定

- USB 线连接 PC 下拉菜单更改 USB 选项 选择 仅充电 USB 调试授权 同意

- 设置 - 网络和互联网 - 绑定 - 开启 - USB绑定

![](1.png)


如果你的桌面上有「LG 网络」这个 App，可以直接用它进入 Hidden Menu 和 Field Test 等隐藏菜单，无需繁琐地在拨号键盘中输入代码。

![](2.png)

「绑定」实际上是网络共享的意思，官方系统的中文翻译很有问题。（甚至比机器翻译还烂）

好的，现在我们打开 QPST Configuration，点击右下角的 Add New Port 按钮。

如果你的手机已经以正确的姿势连接到了电脑，那么应该有一个端口后面显示 (LGE Mobile USB Serial Port)。选中它，OK。

然后在菜单栏的 Start Client 中选择第一项 EFS Explorer，选择刚刚添加的端口，OK。

成功连接并打开类似文件资源管理器的界面后，在左侧找到 policyman 文件夹，选中其中的所有文件（包括子目录 persisted_items），右键选择 Delete 或者按键盘上的 DEL 键删除。（建议备份一份，但这个 Explorer 似乎只支持每次传输一个文件到电脑）

然后把刚才下载的 carrier_policy.xml（记得去掉 .txt 后缀）拖进去（或者右键空白处选择 Copy Data File from PC）

重启手机，大功告成。

如果重启完移动数据的图标还是 4G 或者 LTE 也不用灰心，在拨号键盘中输入 5457#*510# 进入 Service Menu，选择第一项 Debug Screen，如果 NR5G Link 冒号后面的数字为 1，那么恭喜你破解成功了，只是你所在的区域暂时没有 NSA 5G 覆盖或者已经退网了。

如果破解 5G 后在拨号键盘输入 `5457#*510#` 没有反应，在 `*#*#4636#*#*` 中查看也是可以的。

以上内容修改自：[LG V50S 开5G教程 LG V50适用-855板块-LG社区](https://bbs.lge.fun/thread-147.htm)

## 使用 LG UP 来更换系统版本
安装并破解好 LG UP，同时下载好对应版本的 .kdz 线刷包文件。

将手机关机后在插入数据线的同时按住音量加来进入 Download 模式，数据线另一头需要事先连接到电脑。

线刷时请选择可靠的数据线，过程中一定要耐心等待，千万不要在进度条跑完手机端自动重启前断开数据线。

打开 LG UP 并拖入 KDZ 即可开始刷机。

保持默认模式（第一个），不会造成 BootLoader 被自动回锁。

一般两三分钟即可完成，完成后手机会自动重启。

韩版的 KDZ 中一般包含很多无用的 App，能卸载的开机后建议尽快卸载，嫌麻烦也可以刷完先硬格一次以清除所有预装应用，具体方法在下方。

（该章节尚未完成）

## 意外与失败的处理
### 对于 Bootloop（无限重启），首先尝试硬格。

同时按住电源键和音量下键 8 秒左右，当第一屏（即显示手机型号的画面）亮起时松开，3 秒后再次按住电源键，即可进入硬格菜单。

用音量减切换到 Yes，用电源键确认，Erase（擦除）很快。数据飞灰烟灭。

此操作在刷入 TWRP 后不会擦除手机，而是会进入 Recovery，且通过此方式进入一般触屏功能可以使用。

### LG UP 线刷救砖

只要你还能进入 Download 模式，就能使用 LG UP 刷入官方 KDZ 来进行线刷救砖，无需冒险使用 9008 进行救砖。

详见「使用 LG UP 来更换系统版本」一节。

## 鸣谢
对本文启发很大的

[LG 855系列-快速黑砖手册 愿你刷机半生，归来仍是黑砖](https://855.lge.fun/doc_hz/)