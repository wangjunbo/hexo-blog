title: Hexo一些配置
author: peace
tags:
  - Hexo
categories:
  - 博客
date: 2018-02-11 09:47:00
---
1.在设置Search url后，需要使用hexo generate重新生成文章，这样再使用Search的时，google的搜索框中才不会显示http://yoursite.com。   
2.图片放在public/css/images/下，使用/css/images/at.png的方式进行链接。
3.更换banner图片   
图片的位置为：public/css/images/banner.jpg，可以替换为你自己想要的图片。  
4.添加菜单   
在themes/landscape/_config.yml文件中添加菜单  
5.Google Analytics  
http://www.codeblocq.com/2015/12/Add-Google-Analytics-to-your-hexo-blog/   
6.如何在静态博客hexo中只显示摘要信息  
只要加入一个 more 占位符在文章正文里面即可：
```
这里是简介

<!-- more -->

这里是正文
```
7.修改文章的box-shadow属性   
在themes/landscape/source/css/_extend.styl文件的$block那里  
8.修改blockquote的样式,样式在_partial/article.styl中  

参考 https://www.jianshu.com/p/b96fd206571a