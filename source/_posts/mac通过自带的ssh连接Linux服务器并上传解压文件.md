---
title: mac通过自带的ssh连接Linux服务器并上传解压文件
date: 2018-01-27 13:48:46
tags: [mac, linux, 上传文件]
---
# 需求：
1：mac连接linux服务器
2：将mac上的文件上传到linux服务器指定位置
3：解压文件
mac上使用命令，推荐使用 [iterm2](https://www.iterm2.com/downloads.html) 。当然，也可以使用mac自带的终端工具。
<!-- more -->

# 操作过程：
## 一： mac连接linux服务器

输入命令连接Linux服务器：
```
ssh username@ip 
```
其中: username为登录Linux服务器所需的用户名，ip为服务器的地址。默认端口号为22，如果要指定端口号，使用 -p port
```
// 以下两种方式都可以
ssh username@ip -p port 
ssh -p port username@ip 
```
回车，它会要求输入密码，说明以上步骤没有错。输入密码，如果顺利，连上了Linux服务器。
以下，我第一次输入错误密码的提示。第二次密码正确。
![mac通过ssh连接linux服务器](http://images2017.cnblogs.com/blog/564792/201801/564792-20180127132751803-1806625861.png)

此时，就算是连上了linux服务器。

## 二： 上传文件到linux服务器
**新开一个窗口** 使用scp命令
scp是secure copy的缩写，scp是linux系统下基于ssh登录进行安全的远程文件拷贝命令。
介绍一个命令：
```
scp [-r] [-P port] local_file_address username@ip:remote_file_address
```
命令解释：
-r: 递归复制整个目录
-P port: 注意是大写的P，port是指定的端口号。
local_file_address:  本地文件地址。[地址是根据当前命令所在目录来编写的]
remote_file_address: 远程服务器地址。 
输入完以上命令，回车，之后会让你输入服务器密码。成功之后，就开始复制了。以下是我的一个操作。
![scp远程文件拷贝命令](http://images2017.cnblogs.com/blog/564792/201801/564792-20180127133041256-482599300.png)

## 三：解压文件
连接好Linux服务器后，找到要解压的文件目录地址。
`unzip`命令用于解压缩由`zip`命令压缩的`.zip`压缩包

```
unzip test.zip  // 将压缩文件test.zip在当前目录下解压缩。
unzip -n test.zip -d /tmp // 将压缩文件text.zip在指定目录/tmp下解压缩，如果已有相同的文件存在，要求unzip命令不覆盖原先的文件。
```


参考：
1：主要参考：http://blog.csdn.net/fuyujiaof/article/details/56487525
2：scp 命令： http://www.runoob.com/linux/linux-comm-scp.html
3：unzip命令： http://man.linuxde.net/unzip