---
title: CSS3 box-shadow 内外阴影效果
date: 2017-12-14 15:54:45
tags: [css]
---
## 说明
box-shadow 属性可以给元素边框周围添加一个或者多个阴影效果。定义多个阴影，使用逗号分隔。

### 语法
`box-shadow: none | [inset? && [<offset-x> <offset-y> <blur-radius>? <spread-radius>? && <color>? ] ]`

<!-- more -->
### 解释

`none`：默认值为none，表示没有阴影  
`inset`：**可选**。将边框外阴影改为边框内阴影（即使是透明边框），背景之上内容之下。如果不写，默认为边框外阴影。inset只可写在最前或者最后。  
`offset-x`：**必需**。阴影水平方向的偏移量。 0，表示阴影在元素后面；正值，表示阴影在元素右边👉；负值，表示阴影在元素左边👈  
`offset-y`：**必需**。阴影垂直方向的偏移量。   0，表示阴影在元素后面；正值，表示阴影在元素下边☟；负值，表示阴影在元素上边☝  
`blur-radius`：**可选**。阴影的模糊距离。**不允许为负值**。如果值为0，则阴影的边缘清晰，否则，值越大，阴影的边缘越模糊。  
`spread-radius`：**可选**。默认为0，此时阴影与元素同样大；正直，阴影向各个方向延伸扩大；负值，阴影沿相反方向缩小。  
`color`：**可选**。如果没有指定，使用浏览器默认颜色--通常是`color`属性的值。  

## 举个例子
注意：以下例子不是截图
### 正常情况
`<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px blue'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px blue'>css阴影 box-shadow:15px 5px blue</p>

`<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px 10px blue'>css阴影</p>`  
<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px 10px blue'>css阴影 box-shadow:15px 5px 10px blue</p>

`<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px 10px 5px blue'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px 10px 5px blue'>css阴影 box-shadow:15px 5px 10px 5px blue</p>

`<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px 0px 5px blue'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px 0px 5px blue'>css阴影 box-shadow:15px 5px 0px 5px blue</p>

### 负值 (blur-radius不允许为负值，其他三个可以)
`<p style='width:80%; border: 1px solid #ccc; box-shadow:15px -5px blue'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:15px -5px blue'>css阴影 box-shadow:15px -5px blue</p>

`<p style='width:80%; border: 1px solid #ccc; box-shadow:-15px -5px blue'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:-15px -5px blue'>css阴影 box-shadow:-15px -5px blue</p>

`<p style='width:80%; border: 1px solid #ccc; box-shadow:-15px 5px blue'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:-15px 5px blue'>css阴影 box-shadow:-15px 5px blue</p>

`<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px 10px -5px blue'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px 10px -5px blue'>css阴影 box-shadow:15px 5px 10px -5px blue</p>

`<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px 0px -5px blue'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px 0px -5px blue'>css阴影 box-shadow:15px 5px 0px -5px blue</p>

### inset 内阴影

`<p style='width:80%; border: 1px solid #ccc; box-shadow:inset 15px 5px blue'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:inset 15px 5px blue'>css阴影 box-shadow:inset 15px 5px blue</p>

`<p style='width:80%; border: 1px solid #ccc; box-shadow:inset 15px 5px 10px blue'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:inset 15px 5px 10px blue'>css阴影 box-shadow:inset 15px 5px 10px blue</p>

`<p style='width:80%; border: 1px solid #ccc; box-shadow:inset 15px 5px 10px 5px blue'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:inset 15px 5px 10px 5px blue'>css阴影 box-shadow:inset 15px 5px 10px 5px blue</p> 

`<p style='width:80%; border: 1px solid #ccc; box-shadow:inset 15px 5px 0px 5px blue'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:inset 15px 5px 0px 5px blue'>css阴影 box-shadow:inset 15px 5px 0px 5px blue</p>

### 多个阴影
多个阴影，写在前面的权重大，阴影重合部分权重大的值在上面。

`<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px blue, -15px -5px red'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 5px blue, -15px -5px red'>css阴影 box-shadow:15px 5px blue, -15px -5px red</p>

`<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 0px 10px blue, -15px 0px 10px red, 0px 5px 10px yellow, 0px -5px 10px green '>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:15px 0px 10px blue, -15px 0px 10px red, 0px 5px 10px yellow, 0px -5px 10px green '>css阴影 box-shadow:15px 0px 10px blue, -15px 0px 10px red, 0px 5px 10px yellow, 0px -5px 10px green </p>

`<p style='width:80%; border: 1px solid #ccc; box-shadow:-15px -5px red inset,inset 15px 5px blue'>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:-15px -5px red inset,inset 15px 5px blue'>css阴影 box-shadow:-15px -5px red inset,inset 15px 5px blue</p>

`<p style='width:80%; border: 1px solid #ccc; box-shadow:inset 15px 0px 10px blue, inset -15px 0px 10px red, inset 0px 5px 10px yellow, 0px -5px 10px green inset '>css阴影</p>`
<p style='width:80%; border: 1px solid #ccc; box-shadow:inset 15px 0px 10px blue, inset -15px 0px 10px red, inset 0px 5px 10px yellow, 0px -5px 10px green inset '>css阴影 box-shadow:inset 15px 0px 10px blue, inset -15px 0px 10px red, inset 0px 5px 10px yellow, 0px -5px 10px green inset</p>

## 在线生成阴影值
1：F12![开发者工具直接调试](http://images2017.cnblogs.com/blog/564792/201712/564792-20171214154831888-1050223308.png)

2: [Box Shadow CSS Generator](https://cssgenerator.org/box-shadow-css-generator.html)

## 总结
以上，基本用法已经会了。更复杂的情景，理解了应该很好写出来。在翻阅资料的时候，还看到了另一个属性 `filter:drop-shadow`，也表示阴影，但是有区别。
[CSS3 filter:drop-shadow滤镜与box-shadow区别应用](http://www.zhangxinxu.com/wordpress/2016/05/css3-filter-drop-shadow-vs-box-shadow/)