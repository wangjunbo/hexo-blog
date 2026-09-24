title: sun.misc.BASE64Encoder找不到jar包的解决方法
tags:
  - Java
  - Java
categories: []
date: 2016-03-03 10:39:00
---
参考http://blog.csdn.net/jbxiaozi/article/details/7351768    
只需要在project build path中先移除JRE System Library，再添加库JRE System Library，重新编译后就一切正常了。