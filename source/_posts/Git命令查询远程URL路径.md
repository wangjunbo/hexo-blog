title: Git命令查询远程URL路径
author: peace
tags:
  - Git
categories:
  - 编程
date: 2018-11-28 13:54:00
---
If you want only the remote URL, or referential integrity has been broken:
> git config --get remote.origin.url

If you require full output or referential integrity is intact:
> git remote show origin

When using git clone (from GitHub, or any source repository for that matter) the default name for the source of the clone is "origin". Using git remote show will display the information about this remote name. The first few lines should show:
```
C:\Users\jaredpar\VsVim> git remote show origin
* remote origin
  Fetch URL: git@github.com:jaredpar/VsVim.git
  Push  URL: git@github.com:jaredpar/VsVim.git
  HEAD branch: master
  Remote branches:
 ```
If you want to use the value in the script, you would use the first command listed in this answer.