---
title: CSS3 box-sizing属性的应用
date: 2017-09-21 12:26:48
tags: css
---

在一个文档中，每个元素都被表示为一个矩形的盒子。盒子模型具有4个属性['外边距(margin)','边框(border)','内边距(padding)','内容(content)']。
我们要设置某个元素的大小定位，肯定会和这四个元素打交道。只是元素的宽高计算有些默认值。
**box-sizing**属性用于更改用于计算元素宽度和高度的默认的 [CSS 盒子模型](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)。可以使用此属性来模拟不正确支持CSS盒子模型规范的浏览器的行为。
目前支持box-sizing的浏览器：
![支持box-sizing的浏览器][1]
就目前来看，大部分人是建议在初始化样式的时候，就设置为`border-box`，这样更方便设置元素的宽高
```
* {
  -webkit-box-sizing: border-box;
     -moz-box-sizing: border-box;
          box-sizing: border-box;
}
```
<!-- more -->
## 语法
```
box-sizing: content-box | border-box | inherit;
```
### 值
**content-box**
   默认值，标准盒子模型。`width` 和 `height` 只包括内容(`content`)的宽和高。在宽度和高度之外绘制元素的内边距和边框。  

尺寸计算公式:
<font color=red>width = 内容的宽度。  
height = 内容的高度。</font>  

**border-box**
`width`和`height`属性包括内容(`content`)、内边距(`padding`)、边框(`border`)，但是不包括外边距(`margin`)。在宽度和高度之内绘制元素的内容、内边距和边框。

尺寸计算公式： 
<font color=red>width = 内容的宽度 + 内边距的宽度 + 边框的宽度。  
height = 内容的高度 + 内边距的高度 + 边框的高度。</font>  


**inherit**
 规定应该从父元素继承 `box-sizing` 属性的值

## 例子

[在线例子](https://codepen.io/weiqinl/pen/qPNRgK)

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>box-sizing的使用</title>
  <style type="text/css">
  .box {
    width: 460px;
    height: 400px;
    border: 1px solid red;
    margin: 10px;
    background-color: gray;
  }

  .content {
    box-sizing: content-box;
    border: 10px solid blue;
    width: 300px;
    padding: 20px;
    margin: 30px;
    background-color: green;
  }

  .border {
    box-sizing: border-box;
    border: 10px solid blue;
    width: 300px;
    padding: 20px;
    margin: 30px;
    background-color: yellow;
  }

  .inherit {
    box-sizing: inherit;
    border: 10px solid red;
    width: 300px;
    padding: 20px;
    margin: 30px;
    background-color: red;
  }
  </style>
</head>

<body>
  <div class="box">
    <div class="content">
      我是content-box值(默认)
      <br/>box-sizing: content-box;
      <br/>border: 10px solid blue;
      <br/>width: 300px;
      <br/>padding: 20px;
      <br/> margin: 30px;
      <div class="inherit">我是inherit值</div>
    </div>
  </div>
  <div class="box">
    <div class="border">
      我是border-box值
      <br/>box-sizing: border-box;
      <br/>border: 10px solid blue;
      <br/>width: 300px;
      <br/>padding: 20px;
      <br/>margin: 30px;
      <div class="inherit">我是inherit值</div>
    </div>
  </div>
</body>

</html>
```
chrome截图：

![box-sizing的使用案例][2]

[1]: http://upload-images.jianshu.io/upload_images/1808701-e590a5511a91727a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240
[2]: http://upload-images.jianshu.io/upload_images/1808701-1187d8721cb515c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240
