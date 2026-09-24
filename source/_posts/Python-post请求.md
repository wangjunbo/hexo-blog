title: Python post请求
author: peace
tags:
  - Python
categories:
  - 编程
date: 2018-07-19 16:36:00
---
[python requests 快速上手](http://docs.python-requests.org/zh_CN/latest/user/quickstart.html)
```
import requests
import json
source={'title':'易企秀世界领先的h5制作平台'}
source=json.dumps(source)
r = requests.post(uri, data = {'index':'user_test','id':'7','source':source})
print(r.status_code)
print(r.text)

```