title: Mac使用iterm2使.bashrc永久生效
author: peace
tags:
  - Mac
categories:
  - 编程
date: 2018-08-21 22:08:40
---

由于在~/.bashrc中添加了几条alias，每次打开命令行窗口，都需要重新source ~/.bashrc才能生效。   
要想让添加的alias永久生效，方法如下：   
vim ~/.bash_profile，加入source ~/.bashrc解决问题。   
可能.bash_profile不存在，自己创建一个就好了。   

