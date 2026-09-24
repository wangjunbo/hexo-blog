title: 'mysql Incorrect string value: \xF0 for column ''nickName'
author: peace
tags:
  - 编码
categories:
  - mysql
date: 2021-01-09 14:46:00
---
获取微信的昵称在存储到mysql中的的时候，总是报`mysql Incorrect string value: \\xF0 for column 'nickName`错误, 今天研究了一下。
先参考[链接](https://blog.csdn.net/weixin_44395707/article/details/104392970) 中的内容。

其实就是将nickName的字符集调成 utf8mb4，排序规则调成 utf8mb4_general_ci。 

然后链接mysql的时候字符集设置成utf8mb4。如下

![upload successful](/images/pasted-40.png)
