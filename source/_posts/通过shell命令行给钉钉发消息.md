title: 通过shell命令行给钉钉发消息
author: peace
tags:
  - Linux
categories:
  - 编程
date: 2018-05-21 11:49:00
---
```
#!/bin/bash
#script_name:alert_to_DingDing.sh

subject="$1"
message="$2"

token="faaf1acd861a395bfb58170e17043188734cf0a1edc0ac6034a68ee2c6664de0" 
ip=`/sbin/ifconfig -a|grep inet|grep -v 127.0.0.1|grep -v inet6|awk '{print $2}'|tr -d "addr:"`
hostname=`hostname`
body="$hostname[$ip]$message"

function sendMessageToDingding(){
        url="https://oapi.dingtalk.com/robot/send?access_token=${token}"
        UA="Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/535.24 (KHTML, like Gecko) Chrome/19.0.1055.1 Safari/535.24"
 
        result=`curl -XPOST -s -L -H "Content-Type:application/json" -H "charset:utf-8" $url -d "
        {
        \"msgtype\": \"text\", 
        \"text\": {
                 \"content\": \"$1\n$2\"
                 }
        }"`
        echo $result
}
 
sendMessageToDingding $subject $body

```