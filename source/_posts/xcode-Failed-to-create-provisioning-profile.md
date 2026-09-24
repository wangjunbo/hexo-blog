title: xcode Failed to create provisioning profile.
author: peace
tags:
  - iOS
categories:
  - 编程
date: 2019-09-15 10:14:00
---
在使用xcode创建新项目时，有可能会出现以下的错误
```
Failed to create provisioning profile.
The app ID "com.hohode.Cal" cannot be registered to your development team. Change your bundle identifier to a unique string to try again.

No profiles for 'com.hohode.Cal' were found
Xcode couldn't find any iOS App Development provisioning profiles matching 'com.hohode.Calculator'.
```
这个时候，可能是因为包名用了大写的缘故，改为小写就可以了，比如改成com.hohode.cal 