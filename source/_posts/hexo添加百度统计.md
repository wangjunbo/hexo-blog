title: hexo添加百度统计
author: peace
tags:
  - Hexo
categories:
  - 博客
date: 2018-03-06 22:43:00
---
From: <https://www.cnblogs.com/fazero/p/7976651.html>

litten的主题yilia 

  * 编辑文件 `themes/yilia/_config.yml`,添加一行配置，可以删除原来的google analytics

`baidu_tongji: true`

  * 新建 `themes/yilia/layout/_partial/baidu_tongji.ejs`,内容如下
    
    <% if (theme.baidu_tongji) { %>
    <script type="text/javascript">
    #申请的百度统计代码
    </script>
    <% } %>

  * 编辑`themes/yilia/layout/_partial/head.ejs` 在 `</head>` 前添加`<%- partial("baidu_tongji") %>`
  * 重新生产部署站点即可。

[参考][1]

   [1]: http://caoyudong.com/2015/09/16/hexo%E6%B7%BB%E5%8A%A0%E7%99%BE%E5%BA%A6%E7%BB%9F%E8%AE%A1/

