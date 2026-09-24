title: HTML如何让子元素居中对齐
author: peace
date: 2023-04-02 02:27:25
tags:
---
父元素设置为相对定位`position:relative`,子元素使用如下代码设置：
```
position: absolute;
top: 50%;
left: 50%;
transform: translate(-50%, -50%);
```