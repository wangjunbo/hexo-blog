title: 查看腾讯云COS中的appid、Secretld、SecretKey、所属地域
author: peace
date: 2021-08-05 02:04:49
tags:
---
## 1 所属地区
腾讯云"存储痛管理"中的"所属地区"中为中文，在程序里可能需要改写为拼音的大写形式
![所属地区](/images/pasted-48.png)
比如
```
# -*- coding=utf-8
# appid 已在配置中移除,请在参数 Bucket 中带上 appid。Bucket 由 BucketName-APPID 组成
# 1. 设置用户配置, 包括 secretId，secretKey 以及 Region
from qcloud_cos import CosConfig
from qcloud_cos import CosS3Client
import sys
import logging
logging.basicConfig(level=logging.INFO, stream=sys.stdout)
secret_id = 'dsddOHuGL8fwwCFDFSMvEG3Z'      # 替换为用户的 secretId(登录访问管理控制台获取)
secret_key = 'xxxEZfU2JL8etrYy7yZuy'      # 替换为用户的 secretKey(登录访问管理控制台获取)
region = 'ap-beijing'          # 替换为用户的 Region ，腾讯云"存储痛管理"中的"所属地区"中为中文，在这里需要改写为拼音的形式,并且使用ap-前缀
token = None                # 使用临时密钥需要传入 Token，默认为空，可不填
scheme = 'https'            # 指定使用 http/https 协议来访问 COS，默认为 https，可不填
config = CosConfig(Region=region, SecretId=secret_id, SecretKey=secret_key, Token=token, Scheme=scheme)
# 2. 获取客户端对象
client = CosS3Client(config)
# 参照下文的描述。或者参照 Demo 程序，详见 https://github.com/tencentyun/cos-python-sdk-v5/blob/master/qcloud_cos/demo.py
```
## 2 查看秘钥
![查看秘钥](/images/pasted-44.png)
<!-- more -->


![点击继续使用按钮进行创建](/images/pasted-45.png)
在下图中可以看到appid，SecretId,SecretKey

![appid](/images/pasted-46.png)


![upload successful](/images/pasted-47.png)