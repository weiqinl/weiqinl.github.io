---
title: cursor属性-显示的光标的类型(形状)的用法和定义
date: 2017-07-04 10:09:56
tags: css
---
在网页布局的时候，在特定的地方，光标形状各有区别。这个时候，就需要用到css的cursor属性。根据自身需要选择设置鼠标指针样式。
## 定义和用法
cursor 属性规定要显示的光标的类型（形状）。
该属性定义了鼠标指针放在一个元素边界范围内时所用的光标形状（不过 CSS2.1 没有定义由哪个边界确定这个范围）。

|默认值：|    auto|
| :-| :- |
|继承性：|    yes|
|版本： |   CSS2|
|JavaScript 语法：|    object.style.cursor="crosshair"|


## 可能的值 
<!-- more -->
图片测试于： chrome 版本 56.0.2924.87  //   Firefor  版本 51.0.1 (32 位)   //   IE 8.0

|值 |   描述|
| :---- | :------------ |
|url    |需使用的自定义光标的 URL。注释：请在此列表的末端始终定义一种普通的光标，以防没有由 URL 定义的可用光标。|
|default    |默认光标（通常是一个箭头） ![cursor(default).png](http://upload-images.jianshu.io/upload_images/1808701-ea14d1b25c4074da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|auto |默认。浏览器设置的光标。![cursor(auto).png](http://upload-images.jianshu.io/upload_images/1808701-4cd164e263e7bede.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) |
|crosshair    |光标呈现为十字线。![cursor(crosshari)](http://upload-images.jianshu.io/upload_images/1808701-6b234e0f924d107c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|pointer|    光标呈现为指示链接的指针（一只手）  ![cursor(pointer).png](http://upload-images.jianshu.io/upload_images/1808701-0baeef4ab31bcd4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|move|    此光标指示某对象可被移动。  ![cursor(move).png](http://upload-images.jianshu.io/upload_images/1808701-a499daa5cb8ebdca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|e-resize|    此光标指示矩形框的边缘可被向右（东）移动。![cursor(e-resize).png](http://upload-images.jianshu.io/upload_images/1808701-5fdfe5e6e86a623b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|ne-resize|    此光标指示矩形框的边缘可被向上及向右移动（北/东）。  ![cursor(ne-resize).png](http://upload-images.jianshu.io/upload_images/1808701-3027469e8ef6c1f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|nw-resize|    此光标指示矩形框的边缘可被向上及向左移动（北/西）。![cursor(nw-resize).png](http://upload-images.jianshu.io/upload_images/1808701-3b7ae7e67afb3004.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|n-resize|    此光标指示矩形框的边缘可被向上（北）移动。 ![cursor(n-resize).png](http://upload-images.jianshu.io/upload_images/1808701-0fda13888417d026.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|se-resize|    此光标指示矩形框的边缘可被向下及向右移动（南/东）。![cursor(se-resize).png](http://upload-images.jianshu.io/upload_images/1808701-0af0018fa284258a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|sw-resize |   此光标指示矩形框的边缘可被向下及向左移动（南/西）。![cursor(sw-resize).png](http://upload-images.jianshu.io/upload_images/1808701-9375173a1ccb6f38.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|s-resize |   此光标指示矩形框的边缘可被向下移动（南）。 ![cursor(s-resize).png](http://upload-images.jianshu.io/upload_images/1808701-ff9d23221f2e5b9b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|w-resize|    此光标指示矩形框的边缘可被向左移动（西）。![cursor(w-resize).png](http://upload-images.jianshu.io/upload_images/1808701-349251248675553e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|text |   此光标指示文本。![cursor(text).png](http://upload-images.jianshu.io/upload_images/1808701-afc0e42e53129d1d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|wait |   此光标指示程序正忙（通常是一只表或沙漏）。 ![cursor(wait).png](http://upload-images.jianshu.io/upload_images/1808701-e649ede76f8f24be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|
|help |   此光标指示可用的帮助（通常是一个问号或一个气球）。 ![cursor(help).png](http://upload-images.jianshu.io/upload_images/1808701-774744be1ba4943b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)|


## 例子：
```
<!DOCTYPE html>
<html>
<head>
    <title>cursor属性</title>
</head>
<body>
    <p>请把鼠标移动到单词上，可以看到鼠标指针发生变化：</p>
    <span style="cursor:auto">Auto......</span><br />
    <span style="cursor:crosshair">Crosshair......</span><br />
    <span style="cursor:default">Default......</span><br />
    <span style="cursor:pointer">Pointer......</span><br />
    <span style="cursor:move">Move......</span><br />
    <span style="cursor:e-resize">e-resize......</span><br />
    <span style="cursor:ne-resize">ne-resize......</span><br />
    <span style="cursor:nw-resize">nw-resize......</span><br />
    <span style="cursor:n-resize">n-resize......</span><br />
    <span style="cursor:se-resize">se-resize......</span><br />
    <span style="cursor:sw-resize">sw-resize......</span><br />
    <span style="cursor:s-resize">s-resize......</span><br />
    <span style="cursor:w-resize">w-resize......</span><br />
    <span style="cursor:text">text......</span><br />
    <span style="cursor:wait">wait......</span><br />
    <span style="cursor:help">help......</span>
</body>
</html>
```
## 参考地址：
[我的博客地址weiqinl.com](http://weiqinl.com)
[简书](http://www.jianshu.com/p/d53ee728d15e)