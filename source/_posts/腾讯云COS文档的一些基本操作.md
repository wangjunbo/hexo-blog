title: 腾讯云COS文档的一些基本操作
author: peace
tags:
  - 腾讯云
categories:
  - 编程
date: 2021-08-05 07:57:00
---
本文只是对腾讯云COS文档的基本操作做了简单的介绍，具体的更详细的内容参考[腾讯云官方链接](https://cloud.tencent.com/document/product/436/12269
)。  

其实还有一点，腾讯云文档中可能也有一些错误的地方，本文中对其做了调整。比如下边代码中的`region`,应该是ap-加上地域的拼音，但是文档中给的却是COS_REGION，容易让人误解。

安装SDK依赖,其他安装方式见[腾讯云官方链接](https://cloud.tencent.com/document/product/436/12269
)。  
```
pip install -U cos-python-sdk-v5
```

```
# -*- coding=utf-8
#参考 https://cloud.tencent.com/document/product/436/12269

# appid 已在配置中移除,请在参数 Bucket 中带上 appid。Bucket 由 BucketName-APPID 组成
# 1. 设置用户配置, 包括 secretId，secretKey 以及 Region
from qcloud_cos import CosConfig
from qcloud_cos import CosS3Client
import sys
import logging
logging.basicConfig(level=logging.INFO, stream=sys.stdout)

client = None
new_bucket='test123-125313601'

default_bucket = "test-125313601"
secret_id = 'AKID1qPhjUyB6nEOHuGL8JKDrFMvEG3Z'  # 替换为用户的 secretId(登录访问管理控制台获取)
secret_key = 'HGyPaT5RWR7XhbEKDLF7yZuy'  # 替换为用户的 secretKey(登录访问管理控制台获取)
region = 'ap-beijing'  # 替换为用户的 Region ，腾讯云"存储痛管理"中的"所属地区"中为中文，在这里需要改写为拼音的形式,并且使用ap-前缀
token = None  # 使用临时密钥需要传入 Token，默认为空，可不填
scheme = 'https'  # 指定使用 http/https 协议来访问 COS，默认为 https，可不填

def init():
   global client
   config = CosConfig(Region=region, SecretId=secret_id, SecretKey=secret_key, Token=token, Scheme=scheme)
   # 2. 获取客户端对象
   client = CosS3Client(config)

init()

def create_bucket():
   print("创建存储桶")
   response = client.create_bucket(
      Bucket=new_bucket
   )
   print(response)


def upload_file():
   print("上传文件")
   with open('/Users/bill/Desktop/23.png', 'rb') as fp:
      response = client.put_object(
          Bucket=default_bucket,
          Body=fp,
          Key='desktop.jpg',
          StorageClass='STANDARD',
          EnableMD5=False
      )
   print(response['ETag'])


#查询存储桶列表
def look_buckets():
   print("查询存储桶列表")
   response = client.list_buckets()
   print(response)
   buckets = response['Buckets']['Bucket']
   #遍历对象列表
   for bucket in buckets:
      response = client.list_objects(
         Bucket=bucket['Name'],
         Prefix=''
      )
      print(response)


def download_object(bucket,local_path):
   print("获取文件到本地")
   response = client.get_object(
      Bucket=bucket,
      Key='desktop.jpg',
   )
   response['Body'].get_stream_to_file(local_path)


def delete_object(bucket,filename):
   # 删除object
   response = client.delete_object(
      Bucket=bucket,
      Key=filename
   )
   print("已经将腾讯上的{}/{}删除".format(bucket,filename))


# download_object(default_bucket,'/Users/bill/Desktop/img_downloaded2.jpg')
# delete_object(default_bucket,'icon2.png')

```