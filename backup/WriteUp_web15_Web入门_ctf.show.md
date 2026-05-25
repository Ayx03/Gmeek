题目提示：`公开的信息比如邮箱，可能造成信息泄露，产生严重后果`

靶机首页页面最下面有一个 QQ 邮箱：`1156631961@qq.com`

<img width="2349" height="1285" alt="Image" src="https://github.com/user-attachments/assets/962fcaab-2259-45fe-99ed-c06001883bb9" />

访问 `靶机域名/admin` 发现后台登陆系统有忘记密码功能

<img width="2349" height="1285" alt="Image" src="https://github.com/user-attachments/assets/176490aa-cf0e-4933-9641-ec54af44dd5c" />

尝试点击忘记密码，发现密保问题是`我的所在地是哪个城市？`

<img width="2349" height="1285" alt="Image" src="https://github.com/user-attachments/assets/f5ab5691-6592-4bd9-ab42-e1a60a97dd11" />

我们使用 QQ 搜索这个邮箱对应的 QQ 号：`1156631961`，发现公开的资料卡上显示`现居陕西西安`

<img width="1024" height="1084" alt="Image" src="https://github.com/user-attachments/assets/5776aedf-2769-4bd6-abe3-ffb59af4fabb" />

<img width="576" height="1280" alt="Image" src="https://github.com/user-attachments/assets/df2dac38-6c79-4037-b0d7-d9e7605ab269" />

在忘记密码页面输入`西安`并提交，提示`您的密码已重置为admin7789`

<img width="2349" height="1285" alt="Image" src="https://github.com/user-attachments/assets/f895a30d-7316-4490-ab8e-1f0b77f3c594" />

回到 `靶机域名/admin`，使用用户名 `admin` 密码 `admin7789` 登录，获得 flag `ctfshow{206f7e71-d4ed-4b10-98dd-bb0309388641}`。

~~为什么不能使用电脑版 QQ 呢，因为沟槽的 QQ NT 根本没法查看非好友的资料卡~~

刚发现其实新的 QQ NT 电脑版也是可以查看非好友的资料卡的，点击头像即可

<img width="1024" height="1084" alt="Image" src="https://github.com/user-attachments/assets/5776aedf-2769-4bd6-abe3-ffb59af4fabb" />