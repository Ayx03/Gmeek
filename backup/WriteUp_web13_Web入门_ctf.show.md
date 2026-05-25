题目提示：`技术文档里面不要出现敏感信息，部署到生产环境后及时修改默认密码`

推测部署到生产环境使用了默认密码，并且默认密码就在技术文档里

在页面上搜索 `doc` 发现底部有个 `document` 链接

<img width="2560" height="1380" alt="Image" src="https://github.com/user-attachments/assets/a8321705-7724-4f21-be3a-1e7afc47cc9f" />

打开后发现第二页就写着默认后台地址、默认登录用户名和默认密码

<img width="2560" height="1380" alt="Image" src="https://github.com/user-attachments/assets/c7a84267-4b2f-4889-9021-1ece76f31d64" />

访问 `/system1103/login.php`，使用用户名 `admin` 密码 `admin1103` 登录后获得 flag `ctfshow{dea5c12a-2f7c-4d1e-954a-8a8acd7172ac}`。