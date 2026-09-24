title: 自定义tornado请求日志格式
author: peace
tags:
  - tornado
categories:
  - 编程
date: 2021-08-16 02:31:00
---
请求日志中有很多的404 HEAD请求，为了将这部分日志过滤掉，修改了一下tornado请求日志的逻辑。  
代码如下：

自定义请求日志的处理逻辑
```
# 日志消息格式
def log_request(handler):
    if handler.get_status() < 400:
        log_method = tornado.web.access_log.info
    elif handler.get_status() < 500:
        log_method = tornado.web.access_log.info
    else:
        log_method = tornado.web.access_log.info
    request_time = 1000.0 * handler.request.request_time()

    request_status = handler.get_status()
    request_method = handler.request.method

    # log_method("HH status={} method={}".format(request_status, request_method ))
    if request_status == 404 and request_method == 'HEAD':
        #不输出日志
        pass
    else:
        log_method("%s|%d|%s|%.2f|%s",datetime.datetime.now(), request_status , handler._request_summary(), request_time, handler.request.headers.get("User-Agent", ""))

```

设置为Application.settings的log_function属性
```
app.settings["log_function"] = log_request
```

最后输出的日志格式：

![输出的日志格式](/images/pasted-53.png)

参考 https://blog.wencan.org/2017/02/07/tornado-log-format/