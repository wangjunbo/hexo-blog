title: Python发送钉钉消息
author: peace
tags:
  - Python
categories:
  - 编程
date: 2018-08-17 10:45:00
---
使用程序给钉钉发消息，目前看起来只能通过群机器人的方式，先获取机器人的token，然后在程序里调用。   

如果想给个人发送消息，就先拉个人建一个钉钉群，然后将别人踢掉，就剩自己了，就可以只有自己接收消息了。
如果用的Mac，先切换到root用户
```
sudo su -
```
输入密码后，使用如下命令安装钉钉的Python依赖。
```
pip install DingtalkChatbot
```
以下是使用python进行钉钉消息的代码示例：
```
from dingtalkchatbot.chatbot import DingtalkChatbot
# WebHook地址
webhook = 'https://oapi.dingtalk.com/robot/send?access_token=sdfs1c5711212e62ada4b25b88b17966d65'
# 初始化机器人小丁
xiaoding = DingtalkChatbot(webhook)
# Text消息@所有人
at_mobiles=['18655398189']
to = '0289806f09dc2baaaf098555790a492e11c5711212e62ada4b25b88b17966d65'
xiaoding.send_text(msg='我就是小丁，小丁就是我！', is_at_all=False,at_mobiles=at_mobiles)
```

### 使用shell的方式发送钉钉消息
钉钉提供了Webhook协议的自定义接入。使用命令行方式发送钉钉消息的代码示例如下：
```
curl 'https://oapi.dingtalk.com/robot/send?access_token=xxxxxxxx' \
   -H 'Content-Type: application/json' \
   -d '
  {"msgtype": "text", 
    "text": {
        "content": "欢迎访问 hohode.com"
     }
  }' 
```

参考  http://zhangchuzhao.site/2018/01/23/dingtalk-chatbot/