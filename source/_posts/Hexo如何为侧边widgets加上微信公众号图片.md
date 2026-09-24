title: Hexo如何为侧边widgets加上微信公众号图片
author: peace
tags:
  - Hexo
categories:
  - 编程
date: 2018-08-04 14:19:00
---
这篇文章中，我们来在 landscape 主题的侧边栏中添加微信公众号的二维码。

1 找到 themes/landscape 下的配置文件 _config.yml，添加 weixin 变量配置为二维码地址。

```
# 配置关注微信公众号
weixin: http://www.hohode.com/css/images/qrcode_for_gh_0360e257312d_258.jpg
```
2 在 themes/landscape/layout/_widget 目录下新建 weixin.ejs 文件，添加如下代码

```
<% if (theme.weixin){ %>
  <div class="widget-wrap">
    <img src="<%= theme.weixin %>" width="100%" height="100%"/>  
  </div>
<% } %>
```
这里根据是否存在1中的微信二维码链接来控制这个模块的显示。可以根据实际需要设置样式。

3 修改 themes/landscape/_config.yml , 在 widgets 添加 weixin。

```
widgets:
- weixin
- recent_posts
- tagcloud
- category
- archive
- tag
- links
```
总结：
本节中通过将微信公众账号的二维码作为一个组件( weixin.ejs )，利用 hueman 主题已有的侧边栏配置，非常方便的实现了微信公众号二维码的添加。

