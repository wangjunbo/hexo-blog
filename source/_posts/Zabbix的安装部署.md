title: Zabbix的安装部署
author: peace
tags:
  - 工具
categories:
  - 编程
date: 2020-02-08 20:42:00
---
### zabbix安装
按照https://www.zabbix.com/download 文中介绍，进行安装

### 注意
 1.  yum -y install zabbix-server-mysql zabbix-web-mysql zabbix-nginx-conf zabbix-agent 进行安装的时候，可能会失败，多试几次，每次文件都会多下载一点，最终能下载完。
 2. /etc/nginx/conf.d/zabbix.conf 中listen的端口，尽量不要是80，因为很多程序的默认端口都是80，所以为了避免端口冲突，最好改成其他的端口
 3. server_name 可以先不用设置，保持注释的状态
 4. /etc/php-fpm.d/zabbix.conf, uncomment and set the right timezone for you. 一般用Asia/Shanghai
 5. 默认用户名和密码是Admin zabbix


### 添加钉钉报警
管理 -> 报警媒介类型 -> 创建媒体类型

参考 https://www.cnblogs.com/yinzhengjie/p/10372566.html
参考 https://blog.51cto.com/m51cto/2051945
参考 https://segmentfault.com/q/1010000003894661