---
title: 使用Github+Hexo框架搭建部署自己的博客
date: 2017-06-21 14:05:48
tags:
---

## 前言
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown &#40;或其他渲染引擎 &#41;解析文章，
在几秒内，即可利用靓丽的主题生成静态网页。
## 安装
### 安装前提
安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：  
- Node.js  
- Git  
如果您的电脑中已经安装上述必备程序，那么恭喜您！接下来只需要使用 npm 即可完成 Hexo 的安装。

    $ npm install -g hexo-cli

如果您的电脑中尚未安装所需要的程序，请根据以下安装指示完成安装。
### 安装Git

- Windows：下载并安装 [git](https://git-scm.com/download/win).


- Mac：使用 [Homebrew](http://mxcl.github.com/homebrew/), [MacPorts](http://www.macports.org/) ：`brew install git`;或下载 [安装程序](http://sourceforge.net/projects/git-osx-installer/) 安装。


- Linux (Ubuntu, Debian)：`sudo apt-get install git-core`


- Linux (Fedora, Red Hat, CentOS)：`sudo yum install git-core`
### 安装Node.js
安装 Node.js 的最佳方式是使用 nvm。

cURL:

    $ curl https://raw.github.com/creationix/nvm/master/install.sh | sh
Wget:

    $ wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
安装完成后，重启终端并执行下列命令即可安装 Node.js。

    $ nvm install stable
或者您也可以下载 [安装程序](http://nodejs.org/) 来安装。
### 安装Hexo
所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。 

    $ npm install -g hexo-cli

## 建站
安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
    
    $ hexo init <folder>
    $ cd <folder>
    $ npm install

## 部署
Hexo 提供了快速方便的一键部署功能，让您只需一条命令就能将网站部署到服务器上。

    $ hexo deploy
在开始之前，您必须先在 `_config.yml` 中修改参数，一个正确的部署配置中至少要有 `type` 
参数，例如：
    
    deploy:
      type: git
您可同时使用多个 deployer，Hexo 会依照顺序执行每个 deployer。

    deploy:
    - type: git
      repo:
    - type: heroku
      repo:

 

>缩进  
YAML依靠缩进来确定元素间的从属关系。因此，请确保每个deployer的缩进长度相同，并且使用空格缩进。


### Git

如果在使用命令 `hexo deploy`的时候，报错：

    ERROR Deployer not found: git

安装 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)。

    $ npm install hexo-deployer-git --save
修改配置。

    deploy:
      type: git
      repo: <repository url>
      branch: [branch]
      message: [message]

|参数	|描述|  
|-----|----:|  
|repo	|库（Repository）地址|  
|branch	|分支名称。如果您使用的是 GitHub 或 GitCafe 的话，程序会尝试自动检测。|  
|message |自定义提交信息 (默认为  Site updated: &#123;&#123;  now &#40; 'YYYY-MM-DD HH:mm:ss' &#41;  &#125; &#125;) |

我自己的配置为： 
    
    # Deployment
    ## Docs: https://hexo.io/docs/deployment.html
    deploy:
      type: git
      repo: git@github.com:weiqinl/weiqinl.github.io
      branch: master
      message: '提交的消息'

那么，就可以在`weiqinl/weiqinl.github.io`库中，找到部署的文件，提交信息为：提交的消息。

这样，访问地址：`https://weiqinl.github.io`,hexo博客系统搭建完成。



## 参考连接
[hexo](https://hexo.io)
[异常处理](http://blog.csdn.net/chwshuang/article/details/52350559)