title: VSCode使用Remote VSCode编辑远程服务器文件
author: peace
tags:
  - Editor
categories:
  - 编程
date: 2019-01-20 10:34:00
---
### 本地的配置
1 在VSCode的扩展插件中找到Remote VSCode插件并进行安装；
![](https://img-blog.csdn.net/20180827183834418?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d1YmFvYmFvMTk5Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

2 配置ssh config文件，终端中输入：
```
vim ~/.ssh/config
```
粘帖一下内容（记得干掉注释内容）
```
Host dl_aws   # dl_aws 是一个名称，就类似于人类的名字一样
    HostName 18.212.101.21   # remote的地址
    User ubuntu    # remote的名称，这个需要登陆到远端亲自确认
    ForwardAgent yes     # 默认yes就行
    RemoteForward 52698 127.0.0.1:52698   # 前一个52698表示远端的端口，后一个是local的接收地址和端口，这个端口设置为和2步骤中的port一样，这样就可以在VSCode中进行上传和下载了
    IdentityFile /Users/xxx/.ssh/id_rsa.pub  # 这个表示是密匙文件，类比于平时接触的git下ssh的公匙
```
<!-- more -->
3 安装rmate，在终端输入下面的命令 
```
sudo wget -O /usr/local/bin/rmate https://raw.githubusercontent.com/aurora/rmate/master/rmate 
sudo chmod a+x /usr/local/bin/rmate 
```
安装完就可以了

4 此时在终端输入
```
ssh dl_aws
```
1
就可以进入到远端服务器中了，可以看到config其实就是给ssh -i file name@ip这样复杂的语句起了一个别名，让用户在使用的时候更加的方便

### 在远端服务器

1 在远端服务器中重复步骤3安装rmate（一定不要忘记）

2 在远端服务器终端中输入下面命令：
```
rmate -p 52698 PATH/TO/YOUR/FILE
```
 这个路径是服务器端文件的路径，不是本地端的，强烈建议在.bashrc中加入alias rcode=”rmate -p 52698”这样的宏

3 这个时候如果没有意外，你就可以看到VSCode中打开了对应的文件，这里建议进行上述步骤之前，VSCode中最好不要打开本地工程中别的文件，很容易混淆，但是因为这个文件是在临时文件夹中的(/tmp)，所以不必担心会覆盖掉本地的文件

4 最后就是正常编辑，保存就行，保存的时候VSCode的状态栏中会有一个Save xxx to ip@xxx这样的字样，消失之后就说明已经保存到远端了，值得庆幸的是，这样的方式支持同时修改多个文件，但是麻烦的地方在于，如果你关闭了VSCode端的远端文件，想再次编辑的话必须到远端再次执行rnate命令

补充：
在打开vscode之后，要先点击F1，输入remote后，点击start server之后开启服务之后才可以进行远端同步功能 
![](https://img-blog.csdn.net/20180827184750371?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d1YmFvYmFvMTk5Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

from https://blog.csdn.net/wubaobao1993/article/details/82117274