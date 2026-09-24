title: cloudera manager自定义报警脚本
author: peace
date: 2020-06-18 02:12:45
tags:
---
### 1. python报警脚本/data/tmp/alert.py
```
import requests
import sys
import json

configuraion_file="/data/apps/public/conf.ini"

def getConfig():
    print(configuraion_file)
    import configparser
    config = configparser.ConfigParser()
    config.read(configuraion_file)
    return config

def eweixin_send_text(message,token=None,mobiles=[],is_at_all=False):
    if not token:
        config = getConfig()
        token = config.get("dingding", "token")
    data = json.dumps({"msgtype": "text", "text": {"content": message, "mentioned_mobile_list": mobiles}})
    requests.post("https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key="+token, data, auth=('Content-Type', 'application/json'))

if __name__ == "__main__":
    print("1")
    myfile = sys.stdin
    print("2")
    data = json.load(myfile)
    print("3")
    for i in range(0, len(data)):
        alert = data[i]["body"]["alert"]
        alert_summary = alert["attributes"]["ALERT_SUMMARY"]
        alert_content = alert["content"]
        eweixin_send_text("alert summary: %s ,content: %s"%(alert_summary,alert_content))
```

### 2. /data/tmp/alert.sh

```
#!/usr/bin/env bash
echo $(date) >> /data/tmp/alert.log
echo $1 >>/data/tmp/peace/alert.log
echo "begin"
temp_content=`cat $1 | python3 /data/tmp/peace/alert.py`
content=`cat $1`
echo $content >>/data/tmp/alert.log
```

### 在cloudera manager中配置