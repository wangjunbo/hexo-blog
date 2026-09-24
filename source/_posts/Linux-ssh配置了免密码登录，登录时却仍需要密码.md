title: Linux ssh配置了免密码登录，登录时却仍需要密码
author: peace
tags:
  - Linux
categories:
  - 编码
date: 2019-12-11 09:46:00
---
https://blog.csdn.net/b_x_p/article/details/78534423

如果配置了机器A免密码登录B，登录的时候还是需要密码，那么很有可能是B禁止了root登录。

在B上查看配置文件 /etc/ssh/sshd_config

```
#禁用root账户登录，如果是用root用户登录请开启
PermitRootLogin no
```

将PermitRootLogin配置改成yes就可以了。

修改完，记得重启一下ssh的服务:
```
/bin/systemctl restart  sshd.service
或者
service sshd restart
```