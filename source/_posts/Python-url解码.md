title: Python Url解码，判断字符串编码
author: peace
tags:
  - Python
categories:
  - 编程
date: 2018-06-30 10:23:00
---
参考https://www.jianshu.com/p/53bb448fe85b   
http://www.cnblogs.com/kaituorensheng/p/3927000.html    
[检测字符串编码是utf8还是GBK](https://blog.csdn.net/u013314786/article/details/78735139)   

先通过type()判断字符串的类型，是str还是unicode，如果是str，使用如下方式解码
```
import urllib
rawurl = "%E6%B2%B3%E6%BA%90"
url = urllib.unquote(rawurl)
print(url)
```
如果是unicode，则要先转成str，再进行解码
```
url_temp= url_temp.encode('UTF-8')
url_temp = urllib.unquote(url_temp)
event["url"] = url_temp
```