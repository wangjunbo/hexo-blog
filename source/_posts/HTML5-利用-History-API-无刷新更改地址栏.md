title: HTML5 利用 History API 无刷新更改地址栏
author: peace
tags:
  - HTML
categories:
  - 编程
date: 2018-12-27 22:31:00
---

HTML5 新增的历史记录 API 可以实现无刷新更改地址栏链接，配合 AJAX 可以做到无刷新跳转。

简单来说：假设当前页面为`renfei.org/`，那么执行下面的 JavaScript 语句：

```javascript
window.history.pushState(null, null, "/profile/");
```

之后，地址栏的地址就会变成`renfei.org/profile/`，但同时浏览器不会刷新页面，甚至不会检测目标页面是否存在。
<!-- more -->

## pushState 方法

上面的语句实际上用到了 HTML5 的历史记录 API。这套 API 提供一种「人为操纵」浏览器历史记录的方法。

浏览器历史记录可以看作一个「栈」。栈是一种后进先出的结构，可以把它想象成一摞盘子，用户每点开一个新网页，都会在上面加一个新盘子，叫「入栈」。用户每次点击「后退」按钮都会取走最上面的那个盘子，叫做「出栈」。而每次浏览器显示的自然是最顶端的盘子的内容。

执行`pushState`函数之后，会往浏览器的历史记录中添加一条新记录，同时改变地址栏的地址内容。它可以接收三个参数，按顺序分别为：

1.  一个对象或者字符串，用于描述新记录的一些特性。这个参数会被一并添加到历史记录中，以供以后使用。这个参数是开发者根据自己的需要自由给出的。
2.  一个字符串，代表新页面的标题。当前基本上所有浏览器都会忽略这个参数。
3.  一个字符串，代表新页面的相对地址。

例如，我们可以这样写：

```javascript
var state = {
    id: 2,
    name: "profile"
};
window.history.pushState(state, "My Profile", "/profile/");
```

## popstate 事件

当用户点击浏览器的「前进」、「后退」按钮时，就会触发`popstate`事件。你可以监听这一事件，从而作出反应。

```javascript
window.addEventListener("popstate", function(e) {
    var state = e.state;
    // do something...
});
```

这里`e.state`就是当初`pushState`时传入的第一个参数。例如，在我们的例子中，有：

```javascript
e.state.id == 2;
e.state.name == "profile";
```

## replaceState 方法

有时，你希望不添加一个新记录，而是替换当前的记录（比如对网站的 landing page），则可以使用`replaceState`方法。这个方法和`pushState`的参数完全一样。

## 应用：全站 AJAX，并使浏览器能够抓取 AJAX 页面

这个可以干啥用？一个比较常用的场景就是，配合 AJAX。

假设一个页面左侧是若干导航链接，右侧是内容，同时导航时只有右侧的内容需要更新，那么刷新整个页面无疑是浪费的。这时我们可以使用 AJAX 来拉取右面的数据。但是如果仅仅这样，地址栏是不会改变的，用户无法前进、后退，也无法收藏当前页面或者把当前页面分享给他人；搜索引擎抓取也有困难。这时，就可以使用 HTML5 的 History API 来解决这个问题。

思路：首先绑定`click`事件。当用户点击一个链接时，通过`preventDefault`函数防止默认的行为（页面跳转），同时读取链接的地址（如果有 jQuery，可以写成`$(this).attr('href')`），把这个地址通过`pushState`塞入浏览器历史记录中，再利用 AJAX 技术拉取（如果有 jQuery，可以使用`$.get`方法）这个地址中真正的内容，同时替换当前网页的内容。

为了处理用户前进、后退，我们监听`popstate`事件。当用户点击前进或后退按钮时，浏览器地址自动被转换成相应的地址，同时`popstate`事件发生。在事件处理函数中，我们根据当前的地址抓取相应的内容，然后利用 AJAX 拉取这个地址的真正内容，呈现，即可。

最后，整个过程是不会改变页面标题的，可以通过直接对`document.title`赋值来更改页面标题。

## 其他说明

### URL 的限制

为了安全考虑，新 URL 必须和当前 URL 在同一个域名下。例如，你不能把地址改成 Google 的首页。否则不怀好心的人就可以把地址改成网银等关键网站的地址，来迷惑用户了。

但是，URL 允许使用 query string 的形式。例如：

```javascript
window.history.pushState(null, null, "?id=1");
```

在某些情况下可能比较方便。

### 浏览器兼容性

根据 [MDN 提供的信息](https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Manipulating_the_browser_history)，IE 10, Chrome 5, Firefox 4, Safari 5 开始支持这个特性。Fallback 可以采用替换 hash 的方法。另外，[History.js](https://github.com/browserstate/history.js/) 库也提供了对老版本浏览器的 history API 支持（同样是利用替换 hash）。为了搜索引擎收录，可能需要使用`#!`表示法。
