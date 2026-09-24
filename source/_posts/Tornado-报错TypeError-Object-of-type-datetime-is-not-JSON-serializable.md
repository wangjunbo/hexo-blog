title: 'Tornado 报错TypeError: Object of type datetime is not JSON serializable'
author: peace
tags:
  - Tornado
categories:
  - 编程
date: 2021-11-07 04:27:00
---
在使用Tornado从mysql查询出来数据，然后返回通过`self.write(result)`给前端，但是如果数据中包含日期时间类型的字段，比如timestamp类型，就会报`TypeError: Object of Type Datetime Is Not JSON Serializable`错误。  

这个问题其实很好解决，看tornado的源码就可以知道，`self.write()`属于web.py中的方法，这个方法中有一段如下的代码
```
if isinstance(chunk, dict):
    chunk = escape.json_encode(chunk)
    self.set_header("Content-Type", "application/json; charset=UTF-8")
```
其中`escape.json_encode(chunk)`方法中调用了
`json.dumps(value).replace("</", "<\\/")`

这就知道如何做了，改变json.dumps对时间日期字段的解析方式就可以了,先定义一个解码类

```
import json
from decimal import Decimal
from datetime import datetime,date

class DateEncoder(json.JSONEncoder):
    def default(self, o):
        if isinstance(o, datetime):
            return o.strftime('%Y-%m-%d %H:%M:%S')
        elif isinstance(o, date):
            return o.strftime("%Y-%m-%d")
        elif isinstance(o, Decimal):
            return float(o)
        else:
            return json.JSONEncoder.default(self, o)
```

然后将 `def json_encode(value: Any) -> str:` 方法的实现改为
```
return json.dumps(value,cls=DateEncoder).replace("</", "<\\/")
```
就可以了，重启一下服务，就可以了。   

<font color=red>这种方式看似可以，实则有个大坑，会导致很多接口调用参数传递失败，还是不建议这么做，如果真的要修改字符字段的类型，在程序中修改吧。</font>   
      

    
