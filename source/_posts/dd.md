title: 电脑Chrome调试手机(包括微信)上的网页
author: peace
date: 2025-03-26 02:27:46
tags:
---
## 1 手机上打开开发者模式
每个品牌开启开发者模式的方式可能不同。以华为手机为例：

1. 打开手机，进入“设置”页面，搜索“关于手机”选项。

2. 快速点击“HarmonyOS版本”的项目多次，直到提示“您正处于开发者模式！”或者“您已处于开发者模式，无需进行此操作”，即表示您已经打开开发者选项，然后退出该页面。


![upload successful](/images/pasted-68.png)
<!-- more -->

## 2 一定要开启USB调试
华为手机“系统和更新”，然后进入“开发人员选项”

打开“USB 调试”开关，

打开“连接USB时总是弹出提示”开关，


![upload successful](/images/pasted-69.png)


![upload successful](/images/pasted-70.png)

请勿打开“‘仅充电’模式下允许ADB调试”，如果打开了，后续“USB连接方式”只能选择“仅充电”。

## 3 用数据线将手机连接到电脑上
选择“传输文件”，

允许USB调试。


![upload successful](/images/pasted-71.png)


![upload successful](/images/pasted-72.png)

## 4 微信上的网页还要开启微信debug模式
在微信中打开 http://debugxweb.qq.com/?inspector=true 页面，当页面上出现"执行成功"字样，说明微信调试模式打开。

![upload successful](/images/pasted-73.png)
## 5 在Android手机上打开一个页面
微信中打开网页，或者浏览器打开网页都可以。


![upload successful](/images/pasted-74.png)

## 6 在电脑Chrome浏览器上打开 chrome://inspect/#devices

![upload successful](/images/pasted-75.png)

点击inspect，就可以通过控制台看到里面的信息了。

![upload successful](/images/pasted-76.png)

## 7 失败的原因
如果按照上述步骤没有成功，很可能是中间的某些步骤跳过了，或者某些弹窗没有弹窗，或者某些选项没有开启，那就从头开始再重新设置一遍。