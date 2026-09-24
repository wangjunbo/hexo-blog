title: 在Mac上使用iTerm2登录堡垒机jumpserver
author: peace
tags:
  - 运维
categories:
  - 编程
date: 2018-04-13 10:39:00
---
1. Profile -> Open Profiles... -> Edit Profiles...   
2. 点击左下角+号   
3. 输入Profile Name，比如jumper
4. 右边Command下选择Command，然后输入
```
ssh -i /Users/yourname/Documents/yourname.pem  yourname@12.26.20.16
```
5. 点击右上边Advanced菜单，然后点击Triggers下的Edit按钮   
6. 在打开的Triggers窗口中，点击左下角的+号
7. 在Regular Expression中输入 Enter*  
   Action选择Send Text...   
   Parameters写上你的堡垒机登录密码    
8. 关闭所有窗口
9. 在Iterm2的一个窗口中选择右键New Tab,选择刚创建的jumper，然后回车就登录上了。
