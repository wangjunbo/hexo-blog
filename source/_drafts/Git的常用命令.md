title: Git的常用命令
author: peace
date: 2020-01-09 18:14:39
tags:
---
### 配置git命令输出的字体颜色

```
git config --global color.ui auto
```
https://stackoverflow.com/questions/10998792/how-to-color-the-git-console
### git reset 
git reset 操作只对本地提交有效, 对于已经push到服务器的提交是无效的。

举例：

##### git reset --soft commit_id_xxx
向test1.txt文件中加入下面一行。
```
aaa
```
然后提交
```
[root@test_2_83 test]# git add test1.txt
[root@test_2_83 test]# git commit -am 'add file test1.txt and add aaa'
[root@test_2_83 test]# git log
commit 07d87cb5348f84296cd412557bc3f3e4799e5125
Author: root 
Date:   Fri Jan 10 09:41:12 2020 +0800

    add file test1.txt and add aaa
```


git reset --soft 07d87cb5348f84296cd412557bc3f3e4799e5125 表示撤销07d87cb5348f84296cd412557bc3f3e4799e5125版本之后的所有提交记录,但是修改的代码保留了下来。 
git reset --soft 07d87cb5348f84296cd412557bc3f3e4799e5125

15b936d077e5752f49f5e48745c129aea162ee18提交记录已经不存在了
![upload successful](/images/pasted-18.png)

testreset.txt文本中的内容没有变，说明soft方式不会导致修改内容丢失
![upload successful](/images/pasted-19.png)

##### git reset --hard commit_id_xxx
git reset --hard commit_id_xxx 表示撤销commit_id_xxx版本之后的所有提交记录和代码修改。

向testreset.txt文件中再加入下面一行。
```
bbb
```
git commit -am 'add bbb'

向testreset.txt文件中再加入下面一行。
```
ccc
```
git commit -am 'add ccc'
现在testreset.txt中的内容如下：
```
aaa
bbb
ccc
```

![upload successful](/images/pasted-20.png)

git reset --hard 66ef9987b823cec88aa4707205e0e00a19faf87e

现在testreset.txt中的内容如下：
```
aaa
bbb
```
https://blog.csdn.net/yangfengjueqi/article/details/61668381