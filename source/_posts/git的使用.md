title: git的使用
tags:
  - Git
categories: []
date: 2016-02-15 10:27:00
---
### git查看远程仓库地址
```
git remote -v
```

### 创建项目:在github上创建一个项目test

* git配置
  	git config --global user.name “WangJunbo"
  	git config —global user.email “11@q.com"
     
* 下载代码
		 git clone “https://github.com/xiaowang/test.git"
* 添加文件
     	git add news.txt
     	git add .  //添加全部修改或新增的文件
* 本地提交
     	git commit -am “提交测试代码"
* 将提交推送到远程服务器
     	git push origin master
* 修改本地文件
     	vim news.txt
* 提交修改 
     	git commit -an “提交修改"
* 将提交推送到远程服务器
     	git push
* 更新本地代码
     	git pull
* 删除目录
     	git rm --cached news.txt
     	git commit -m '删除news.txt'
     	git push origin master

### 其它
git remote add origin https://github.com/xiaoming/maple.git

1. 创建分支
       git branch v2
2. 将分支同步到服务器
       git push origin v2
3. 切换分支
    	git checkout v2
4. 修改v2分支上的文件
5. 提交本地修改
    	git commit -am “v2修改"
6. 将提交推送到服务器
    	git push origin v2
    	查看:v2内容修改, master文件不变
7. 将v2分支合并到主干
    	git checkout master
    	git merge v2
    	git push


* 版本还原
      git checkout v2
      git revert HEAD
      填写描述信息
      查看本地文件内容是否修改
      git push origin v2
* 还原到指定版本



### 在已经存在的文件夹中使用git
	git init
	git add . 
	git commit -am '初始化messageController2'
	git remote add origin 
	http://boom.qctt.cn:1234/wangjunbo/messagecontroller2.git
	git push -u origin master
    
 ### Git 提示fatal: remote origin already exists [错误解决办法](https://blog.csdn.net/top_code/article/details/50381432)
 1、先删除远程 Git 仓库
```
$ git remote rm origin
```
2、再添加远程 Git 仓库
```
$ git remote add origin git@github.com:FBing/java-code-generator
 ```
### [git 忽略已经被提交的文件](https://blog.csdn.net/weixin_37292229/article/details/79245462)
```
1.先把项目备份，以防万一。
2.git rm --cached app.iml //从版本库中rm 文件，working dicrectory中仍然保留，如果要删除目录下所有文件包括子目录中的 git rm -r --cached directory_name
3.在.gitignore中添加要忽略的文件
4.把修改的文件commit并且push到服务端
5.从git上重新拉取这个项目。
```