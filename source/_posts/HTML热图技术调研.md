title: HTML热图技术调研
author: peace
tags:
  - HTML
  - HTML热图
categories:
  - 编程
date: 2021-07-15 03:33:00
---
### heatmap.js
有一种比较成熟的热图技术heatmap.js，通过不同的颜色来标记不同位置(页面上的坐标点)的访问频率，但是不能够标识出页面上某个元素，比如链接，图片的点击次数。  
官网地址https://www.patrick-wied.at/static/heatmapjs/  
git地址https://github.com/pa7/heatmap.js   
使用demo参考 https://blog.csdn.net/Jermyo/article/details/110561098   

### 事件监听
给当前页面的window对象添加一个事件监听器。  
在用户点击的时候，上报被点击元素的信息(主要包括url和element path等)。  
在展示的时候，计算每个节点的点击PV和UV，并通过元素outline的宽度展示元素的点击量，当鼠标移动到某个元素上时，会出现tip提示框，显示具体的PV和UV数据。 
```
window.addEventListener("click", function(e){
    console.log(e.target.tagName);
});
```
参考 https://segmentfault.com/q/1010000005686667