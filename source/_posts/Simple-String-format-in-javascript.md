title: Simple String.format() in javascript
author: peace
tags:
  - WEB
categories:
  - 编程
date: 2018-07-22 17:52:00
---

```
String.prototype.format = function() {
  a = this;
  for (k in arguments) {
    a = a.replace("{" + k + "}", arguments[k])
  }
  return a
}
```
Usage:
```
console.log("Hello, {0}!".format("World"))
```

https://coderwall.com/p/flonoa/simple-string-format-in-javascript