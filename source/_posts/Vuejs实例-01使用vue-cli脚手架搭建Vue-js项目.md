---
title: Vuejs实例-01使用vue-cli脚手架搭建Vue.js项目
date: 2017-06-29 10:54:36
tags: [vue, vue-cli]
---
 
## 1. 前言 
 
`vue-cli` 一个简单的构建`Vue.js`项目的命令行界面
 
整体过程：
 
    $ npm install -g vue-cli 
    $ vue init webpack vue-admin 
    $ cd vue-admin 
    $ npm install 
    $ npm run dev 
 
后面分步说明。 
<!-- more -->
## 2. 安装
 
前提条件，Node.js >=4.x版本，建议使用6.x版本。npm版本 > 3.x 
全局安装vue-cli
 
    $ npm install -g vue-cli 
 
![vue-cli-install](http://images2015.cnblogs.com/blog/564792/201705/564792-20170518211531385-7860564.png "vue-cli-install")

... 
![vue-cli-install](http://images2015.cnblogs.com/blog/564792/201705/564792-20170518211516994-1560191826.png "vue-cli-install")


 
## 3. 使用
     $ vue init <template-name> <project-name> 
 
vue官方提供了多个打包工具版本的模版。我们可以使用`vue list`命令查看，当前可以使用的模版。
 
    $ vue list 
![vue list](http://images2015.cnblogs.com/blog/564792/201705/564792-20170518211446869-690496824.png  "vue list")



我们在这里，使用`webpack`模版。 功能齐全的`webpack`& `vue-loader` 提供热加载、代码检查、单元测试和css分离
 
    $ vue init webpack vue-element-admin
 
![vue init](http://images2015.cnblogs.com/blog/564792/201705/564792-20170519101621432-221179883.png "vue init")

 
之后，在`E：\project`文件夹下面，会有刚初始化的构建的项目`vue-element-admin`

![project file](http://images2015.cnblogs.com/blog/564792/201705/564792-20170518211346853-1070360488.png "project file")

 
 
## 4. 运行结果
 
项目基础结构已经搭建好了，现在来启动它。
进入项目文件:
 
    $ cd vue-element-admin 
 
安装依赖：
 
中国行情原因，直接安装，有时候会失败。我们一般使用npm的淘宝镜像cnpm。
先安装cnpm：
 
    $ npm install -g cnpm --registry=https://registry.npm.taobao.org
之后，使用:
 
    $ cnpm install 
你直接安装也可以的：
 
    $ npm install 
运行：
 
    $ npm run dev

![npm run dev](http://images2015.cnblogs.com/blog/564792/201705/564792-20170518211208697-1183076460.png "npm run dev")

 
启动之后，自动打开默认浏览器 

![admin page](http://images2015.cnblogs.com/blog/564792/201705/564792-20170518211307666-440031481.png "主界面")
 
之后，就可以进行本地开发，实时预览开发效果。
 
## 5. 打包部署
项目开发完成之后，可以使用`npm run build`进行打包工作
 
    $ npm run build
打包完成之后，会生成`dist`文件夹，项目上线时候，只需要将`dist`文件夹放到服务器就行了。
 
## 6. 项目结构
![项目结构](http://images2015.cnblogs.com/blog/564792/201705/564792-20170518211847322-1043427083.png "项目结构")
 
    vue-element-admin:             项目名称 
    |   .babelrc                   babel的配置文件 
    |   .editorconfig              编辑器的配置文件 
    |   .gitignore                 git的忽略文件 
    |   .postcssrc.js              编译css的工具 
    |   index.html                 网站主页 
    |   package.json               项目依赖哪些包的文件 
    |   README.md                  说明文档 
    |  
    +---build                      构建的配置文件夹 
    |       build.js               项目构建配置入口
    |       check-versions.js
    |       dev-client.js
    |       dev-server.js
    |       utils.js
    |       vue-loader.conf.js
    |       webpack.base.conf.js
    |       webpack.dev.conf.js
    |       webpack.prod.conf.js
    |       webpack.test.conf.js
    |      
    +---config                      整体的配置文件夹
    |       dev.env.js
    |       index.js                配置文件入口
    |       prod.env.js
    |       test.env.js
    |      
    +---src                         源文件夹
    |   |   App.vue                 布局组件
    |   |   main.js                 入口文件
    |   |  
    |   +---assets                  静态文件夹
    |   |       logo.png
    |   |      
    |   +---components              组件文件夹
    |   |       Hello.vue           单个组件
    |   |      
    |   \---router                  路由文件夹
    |           index.js            路由主页
    |          
    +---static                      静态文件夹
    |       .gitkeep                空文件（Git本身不允许全空目录提交至版本库，一个办法是在目录下创建一个文件，取名为.gitkeep是习惯用法）
    |      
    \---test                        测试文件夹

## 7. 总结
  万事开头难，前期项目搭建可能会遇到一些问题，冷静分析慢慢都能解决的。

## 8. 项目源码 


[Vuejs实例-Vuejs2.0全家桶结合ELementUI制作后台管理系统](http://www.cnblogs.com/weiqinl/p/6873761.html)
[git项目源码](https://github.com/weiqinl/vue-element-admin)


项目首发于博客园：[Vuejs实例-01使用vue-cli脚手架搭建Vue.js项目](http://www.cnblogs.com/weiqinl/p/6875645.html)