title: 一些Tornado知识
author: peace
tags:
  - WEB
categories:
  - 编程
date: 2018-05-09 11:14:00
---
### static静态资源
```
        settings = dict(
            template_path=os.path.join(os.path.dirname(__file__),"../templates"),
            static_path =os.path.join(os.path.dirname(__file__), "/data/static"),
            debug=True,
        )
```
假设在服务器上存在/data/static/other/a.txt文件，那么对应的路径url则为http://www.yoursite.com/static/other/a.txt

### [Tornado模板转义处理](http://www.qttc.net/201305320.html)
在html页面的最上边加上
```
{% raw title %}
```
就不显示html标签了。