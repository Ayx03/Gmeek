这就是所谓的 OSINT（Open Source Intelligence，开源情报？）吧，感觉会挺有意思

使用 HoneyView 打开发现图片 EXIF 中有 GPS（？其实应该是 GNSS，拍摄时不一定是完全或者部分通过 GPS 获取的）经纬度信息，可以直接打开 Google 地图查看对应位置

<img width="540" height="798" alt="Image" src="https://github.com/user-attachments/assets/6fa88abc-44bc-4f21-b052-d59cddb866ed" />

发现位置在中国大陆，在中国大陆 Google 地图可能会出现漂移，不过其实这里已经可以确定是厦门市，猜测到是鼓浪屿了，就是不太确定

<img width="2560" height="1600" alt="Image" src="https://github.com/user-attachments/assets/28cf026b-56e3-49bb-b51e-0b3fac68a7bc" />

可以手动复制经纬度，也可以将图片上传到 <https://www.strerr.com/cn/exif.html> 这种读取图片 EXIF 信息的工具一键打开高德地图查看对应位置

<img width="2560" height="1600" alt="Image" src="https://github.com/user-attachments/assets/ef1a8b57-f4ba-4408-8c7f-59252633bddf" />

打开高德地图提示`在港仔后海滨沙滩附近`，尝试提交`ctfshow{厦门市_港仔后海滨沙滩}`不正确，后提交`ctfshow{厦门市_鼓浪屿}`通过。

<img width="2560" height="1600" alt="Image" src="https://github.com/user-attachments/assets/07c3a4bc-80fc-493b-bb29-1ae86b479837" />

<!--<img width="2560" height="1600" alt="Image" src="https://github.com/user-attachments/assets/9dd91dc2-d186-4e12-a709-c14b6b6e6f8a" />-->