title: linux history 提示自动补全
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2019-10-09 14:02:00
---
软件安装参考 https://github.com/dvorka/hstr
```
sudo yum install -y hstr
hstr --show-configuration >> /etc/profile
```
之后 记得使命令生效
```
source /etc/profile
```
在命令行收入hstr，就会出现命令history，If you want to make running of `hstr` from command line even easier, then define alias in your `~/.bashrc`:
```
alias hh=hstr
```
Don't forget to source ~/.bashrc to be able to to use hh command.


其它：  
使用root用户执行
```
sudo yum install -y hstr
hstr --show-configuration >> /etc/profile
echo "alias hh=hstr" >> /etc/profile
source /etc/profile
```