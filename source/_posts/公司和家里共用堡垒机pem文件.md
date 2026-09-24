title: 公司和家里电脑共用堡垒机Pem文件
author: peace
tags:
  - 运维
categories:
  - 编程
date: 2020-03-24 07:58:00
---
问题：发现在公司和家里不能共用一个pem文件，家里的电脑可以用的时候，公司就无法免密码登录，反之亦然。

运行 ssh-add  把键值添加的 ssh-agent 代理中，就可以了。

步骤如下：
1 启动ssh-agent
```
ssh-agent bash
```

2 然后运行 ssh-add  把键值添加的 ssh-agent 代理中
```
ssh-add ~/.ssh/id_rsa
```

参考：
https://yijiebuyi.com/blog/4b5c272e7058cb331098250c8e98eb3e.html
https://help.github.com/cn/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent