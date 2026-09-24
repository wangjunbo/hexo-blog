title: '在Mac上错bash: pip: command not found '
author: peace
tags:
  - Python
categories:
  - 编程
date: 2018-04-16 11:02:00
---
执行   
```
pip install configparser
```

报错：bash: pip: command not found   

解决办法：执行   
```
sudo easy_install pip
```
输入密码   

```
sudo pip install configparser
```

提示：   
以后执行pip的时候都要带上sudo