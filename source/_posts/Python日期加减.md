title: Python日期的加减
tags:
  - Python
categories:
  - 编程
date: 2018-08-03 09:39:52
---
https://segmentfault.com/q/1010000012025203

对于天，小时，分钟和秒的加减使用timedelta就可以做到
```
from datetime import datetime,timedelta
yesterday = datetime.now() + timedelta(days=-1,hours=-1,minutes=-1,seconds=-1)
```
但是对于月的加减就需要用到下边的方式
```
from dateutil.relativedelta import relativedelta
last_month = datetime.now() + relativedelta(months=-1)
```
