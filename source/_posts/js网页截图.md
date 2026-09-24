title: js网页截图
author: peace
tags:
  - WEB
categories:
  - 编程
date: 2018-05-17 10:36:00
---
先看一个可以运行的示例(可能需要翻墙)：     
[example](http://googleapi.cc/static/page/cut.html)
```
<!DOCTYPE html>
<html>
    <head>
        <meta name="layout" content="main">
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />  
        <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.8.3/jquery.min.js"></script>
        <script type="text/javascript" src="https://html2canvas.hertzen.com/dist/html2canvas.js"></script>
        <script type="text/javascript" src="http://hongru.github.io/proj/canvas2image/canvas2image.js"></script>
         
        <script  type="text/javascript" >
        $(document).ready( function(){
                $(".example1").on("click", function(event) {

                        html2canvas(document.querySelector("#capture")).then(canvas => {
                            document.body.appendChild(canvas);
                        });

                }); 

                $("#download").on("click", function(event) {
                        html2canvas(document.querySelector("#capture")).then(canvas => {
                            Canvas2Image.saveAsImage(canvas, canvas.width, canvas.height,"png","hello")
                        });

                });              
        });
         
        </script>
    </head>
    <body>
         
         <div id="capture" style="padding: 10px; background: #f5da55">
            <h3>这真是一个强大的工具</h3>
            <h4 style="color: #000; ">Hello world!</h4>
            <h3>这真是一个强大的工具</h3>
        </div>
        <input id="download" type="button" value="下载">
        <input class="example1" type="button" value="截图">
        生成界面如下：
    </body>
</html>
```
由于官方的Canvas2Image代码不能设置下载的文件名,有人提了一个PR，但是好像没有合并到主分支，所以用的时候可以用那个可以设置下载图片文件名的分支,[这是可以设置图片文件名的分支](https://github.com/hongru/canvas2image/blob/df1d31941b89de4f122f493c45a424f5b5ab3dab/canvas2image.js)
### 下边的参考网页很重要
https://segmentfault.com/a/1190000011478657
https://html2canvas.hertzen.com/  
[设置图片文件名](https://github.com/hongru/canvas2image/blob/df1d31941b89de4f122f493c45a424f5b5ab3dab/canvas2image.js)  
[网页截图、涂鸦的js库](https://github.com/node-modules/webcamera)   
