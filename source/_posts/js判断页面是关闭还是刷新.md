title: js判断页面是关闭还是刷新
author: Peace
tags:
  - WEB
categories:
  - 编程
date: 2019年06月23日
---

1. 根据页面关闭时的事件判断
https://blog.csdn.net/u010175124/article/details/9092899
页面加载时只执行onload
页面关闭时只执行onunload
页面刷新时先执行onbeforeunload，然后onunload，最后onload

```
<html> 
<head> 
<title>判断页面是关闭还是刷新</title> 
</head> 
 
<body onunload="fclose();" onload="fload();" onbeforeunload="bfunload();"> 
<script language="javascript"> 
var s = "test"; 
function fclose() 
{ 
if(s=="no") 
alert(’unload me!=’+s+’这是刷新页面！’); 
else 
alert(’这是关闭页面’); 
} 
 
function fload() 
{ 
alert("load me!="+s); 
} 
 
function bfunload() 
{ 
s = "no"; 
} 
</script> 
</body> 
</html>
```

2. 根据 performance.navigation.type判断
https://www.zhihu.com/question/29036668
0表示从链接进去，1表示刷新

3. 根据window.name判断
https://www.zhihu.com/question/29036668
```js<script type="text/javascript">
           window.onload = function() {
              if (window.name == "") { // 直接进来才是空的
                alert('直接进来');
              } 
              window.name = "test"; // 刷新当前页面，window.name并不会销毁
           };
     </script>
```







