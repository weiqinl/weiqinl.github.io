---
title: 如何搭建博客
date: 2017-06-17 16:28:31
tags:
---
本人一直想搭建个人博客，于是在网上找了资料，自己动手搭建发现并不难。现在，把搭建过程记录下来。

hexo版本不同，有些配置也会不同。我把我机器配置发出来。

hexo-v.png

此教程针对的是Windows系统。

#配置环境

##安装Node

##安装Git

## 申请GitHub

## 正式安装HEXO

全局安装hexo, 打开Git 命令，执行如下命令

    $ npm install -g hexo 

这样，安装成功，就可以在全局直接使用hexo命令了。
比如，可以使用命令`hexo -v`查询当前运行的`hexo`版本.
图在一开始，已经发出来了。


搭建Hexo博客，所需要的配置基本完成。以下是涉及到HEXO的配置了。

# HEXO的配置

## 初始化
在本地新建一个文件夹，名字和GitHub上的地址名字一样的。比如： `weiqinl.github.io`。
然后，在该文件夹下执行初始化方法：

     $ hexo init

hexo-init.png

## 本地启动
运行下面的命令： 

    $ hexo server

表明`Hexo Server`已经在本地启动了，在浏览器中输入`http://localhost:4000/`,这个时候可以看到由`Hexo`搭建的博客，里面有一篇，系统自带的博文`Hello World`,介绍如何使用`Hexo`。
你可以按`Ctril + C` 停止`Server`。

## 创建一个新文章
使用命令`hexo new 'My New Post'`创建一个命名为`My-New-Post.md`的文件，里面默认标题为`My New Post`的文章。该文章创建在目录 `/source/_posts/`下。此时，就可以打开该文件，使用Markdown风格的编辑器来编辑。

    $ hexo new 'My New Post'
    INFO  Created: E:\project\weiqinl.github.io\source\_posts\My-New-Post‘.md


## 生成静态页面
执行以下命令，将markdown文件生成静态网页。

    $ hexo generate 

该命令执行完成后，会在根目录下生成文件夹`public`,里面是该站点的相关css和js。

## git部署

修改_config.yml文件，如下： 

    # Deployment
    ## Docs: https://hexo.io/docs/deployment.html
    deploy:
      type: git
      repository: git@github.com:weiqinl/weiqinl.github.io
      branch: master
执行命令：

    $ hexo deploy

  这里，如果遇到问题：
  
      $ hexo deploy
      ERROR Deployer not found: git
  
那么执行命令：

     $ npm install hexo-deployer-git --save

执行成功之后，再次运行命令`hexo deploy`

此时，博客就算是发布成功了。可以通过地址`weiqinl.github.io`查看。


##一些常用命令