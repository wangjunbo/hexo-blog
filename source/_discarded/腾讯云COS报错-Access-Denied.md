title: 腾讯云COS报错 Access Denied
author: peace
date: 2021-09-07 09:08:28
tags:
---
```
qcloud_cos.cos_exception.CosServiceError: {'code': 'AccessDenied', 'message': 'Access Denied.', 'resource': 'xxx-123456.cos.ap-shanghai.myqcloud.com/qVmkxz.jpg', 'requestid': 'ZTFkMDlfNGQyZV9iZTc4Y2Y=', 'traceid': 'AxODc0OWRkZjk0ZDM1NmI1M2E2MTRlY2MzZDhmNmI5MWI1OTQyYWVlY2QwZTk2MDVmZDQ3MmI2Y2I4ZmI5ZmM4ODFjYjQ4YWNmNTExZDQwYWNmODY3OGE1ODU3ZDQzMDUzM2I='}
```

原因是没有向根目录上传文件的权限，