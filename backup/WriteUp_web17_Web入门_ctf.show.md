题目提示：`备份的sql文件会泄露敏感信息`

回忆一下 sql 文件的扩展名好像就是 sql，也可能不同的数据库管理系统导出的不一样

尝试了访问 `db.sql` `1.sql` 最后发现是 `backup.sql`，使用任意文本编辑器（如记事本）打开文件即可找到 flag `ctfshow{a2ba4307-863b-4c2d-953b-12244304c73e}`
```
CREATE DATABASE IF NOT EXISTS ctfshow;
USE ctfshow;

--
-- Table structure for table `products`
--

CREATE TABLE IF NOT EXISTS `products` (
  `product_id` int(11) NOT NULL,
  `name` varchar(100) NOT NULL,
  `sku` varchar(14) NOT NULL,
  `price` decimal(15,2) NOT NULL,
  `image` varchar(50) NOT NULL,
  PRIMARY KEY (`product_id`),
  UNIQUE KEY `sku` (`sku`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8;

CREATE TABLE `ctfshow_secret` (
  `secret` varchar(255) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;


INSERT INTO `ctfshow_secret` VALUES ('ctfshow{a2ba4307-863b-4c2d-953b-12244304c73e}');

--
-- Dumping data for table `products`
--

INSERT INTO `products` (`product_id`, `name`, `sku`, `price`, `image`) VALUES
(1, 'Iphone', 'IPHO001', '400.00', 'images/iphone.jpg'),
(2, 'Camera', 'CAME001', '700.00', 'images/camera.jpg'),
(3, 'Watch', 'WATC001', '100.00', 'images/watch.jpg');

```