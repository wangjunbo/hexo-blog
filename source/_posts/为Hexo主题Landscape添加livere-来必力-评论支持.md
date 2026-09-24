title: 为Hexo主题Landscape添加livere(来必力)评论支持
author: peace
tags:
  - Hexo
categories:
  - 编程
date: 2018-04-25 22:22:00
---
 
From http://www.zhoujy.me/2017/07/16/livere/

### 注册 livere 

[livere][1] 有两个版本： 

   [1]: https://livere.com/

  * City 版：是一款适合所有人使用的免费版本
  * Premium 版：是一款能够帮助企业实现自动化管理的多功能收费版本

使用City版可以满足基本需求了。  
安装，获取livere安装代码：[https://livere.com/insight/myCode。][2]  


   [2]: https://livere.com/insight/myCode%E3%80%82

### 添加livere插件 

首先在 themes/landscape/_config.yml 文件中添加如下配置： 
```
livere: true 
```

然后\layout\_partial\post目录下添加livere.ejs，把livere安装代码复制进去，文件内容如下，其中data-uid就是你自己的data-uid。 
```
<!-- 来必力City版安装代码 -->
<div id="lv-container" data-id="city" data-uid="your data-id">
<script type="text/javascript">
   (function(d, s) {
       var j, e = d.getElementsByTagName(s)[0];

       if (typeof LivereTower === 'function') { return; }

       j = d.createElement(s);
       j.src = 'https://cdn-city.livere.com/js/embed.dist.js';
       j.async = true;

       e.parentNode.insertBefore(j, e);
   })(document, 'script');
</script>
<noscript>为正常使用来必力评论功能请激活JavaScript</noscript>
</div>
<!-- City版安装代码已完成 -->
```

最后在layout\_partial\article.ejs中<% if (!index && post.comments){ %>后面添加如下代码： 
```

<% if (theme.livere){ %>

<%- partial('post/livere', { 

key: post.slug, 

title: post.title, 

url: config.url+url_for(post.path) 

}) %>

<% } %>
```

至此，为landscape主题添加livere评论插件完成。  
效果见下方评论栏，觉得还行的可以留言啊。 
