---
title: html5原生拖拽/拖放 drag & drop 详解
date: 2017-12-07 16:39:11
tags: [html, drag, drop]
---
## 前言
拖放（drap && drop）在我们平时的工作中，经常遇到。它表示：抓取对象以后拖放到另一个位置。目前，它是HTML5标准的一部分。我从几个方面学习并实践这个功能。
## 拖放的流程对应的事件
我们先看下拖放的流程：
```
选中  --->  拖动  ---> 释放 

```
然后，我们一步步看下这个过程中，会发生的事情。
### 选中
在HTML5标准中，为了使元素可拖动，把draggable属性设置为true。
文本、图片和链接是默认可以拖放的，它们的draggable属性自动被设置成了true。
图片和链接按住鼠标左键选中，就可以拖放。
文本只有在被选中的情况下才能拖放。如果显示设置文本的draggable属性为true，按住鼠标左键也可以直接拖放。

draggable属性：设置元素是否可拖动。
语法：`<element draggable="true | false | auto" >`
* true: 可以拖动  
* false: 禁止拖动  
* auto: 跟随浏览器定义是否可以拖动  

### 拖动
每一个可拖动的元素，在拖动过程中，都会经历三个过程，`拖动开始`-->`拖动过程中`--> `拖动结束`。

|针对对象|    事件名称    |说明|
| :---: | :--- | :---:|
|被拖动的元素    |dragstart    |在元素开始被拖动时候触发|
|  |drag    |在元素被拖动时反复触发|
|  |dragend    |在拖动操作完成时触发|
|| ||
|目的地对象    |dragenter    |当被拖动元素进入目的地元素所占据的屏幕空间时触发
| |dragover    |当被拖动元素在目的地元素内时触发|
| |dragleave    |当被拖动元素没有放下就离开目的地元素时触发|
dragenter和dragover事件的默认行为是拒绝接受任何被拖放的元素。因此，我们必须阻止浏览器这种默认行为。e.preventDefault();
    
### 释放
到达目的地之后，释放元素事件

|针对对象|    事件名称    |说明|
| :---: | :--- | :---:|
|目的地对象  |  drop |   当被拖动元素在目的地元素里放下时触发，一般需要取消浏览器的默认行为。|

### 选中拖动释放例子
```
<!DOCTYPE HTML>
<html>

<head>
    <title>拖放示例-文本</title>
</head>
<style>
.src {
    display: flex;
}

.dropabled {
    flex: 1;
}

.txt {
    color: green;
}

.img {
    width: 100px;
    height: 100px;
    border: 1px solid gray;
}

.target {
    width: 200px;
    height: 200px;
    line-height: 200px;
    text-align: center;
    border: 1px solid gray;
    color: red;
}
</style>

<body>
    <div class="src">
        <div class="dragabled">
            <div class="txt" id="txt">
                所有的文字都可拖拽。
                <p draggable="true">此段文字设置了属性draggable="true"</p>  
            </div>
            <div class="url" id="url">
                <a href="http://weiqinl.com" target="_blank">我是url:http://weiqinl.com</a>
            </div>
            <img class="img" id="tupian1" src="img1.png" alt="图片1" />
            <img class="img" id="tupian2" src="img2.png" alt="图片2" />
        </div>
        <div id='target' class="dropabled target">Drop Here</div>
    </div>
    <script>
        var dragSrc = document.getElementById('txt')
        var target = document.getElementById('target')

        dragSrc.ondragstart = handle_start
        dragSrc.ondrag = handle_drag
        dragSrc.ondragend = handle_end

        function handle_start(e) {
          console.log('dragstart-在元素开始被拖动时候触发')
        }

      function handle_drag() {
            console.log('drag-在元素被拖动时候反复触发')
        }

      function handle_end() {
            console.log('dragend-在拖动操作完成时触发')
        }


        target.ondragenter = handle_enter
        target.ondragover = handle_over
        target.ondragleave = handle_leave

        target.ondrop = handle_drop

        function handle_enter(e) {
            console.log('handle_enter-当元素进入目的地时触发')
            // 阻止浏览器默认行为
            e.preventDefault()
        }

        function handle_over(e) {
            console.log('handle_over-当元素在目的地时触发')
            // 阻止浏览器默认行为
            e.preventDefault()
        }

        function handle_leave(e) {
            console.log('handle_leave-当元素离开目的地时触发')
            // 阻止浏览器默认行为
            // e.preventDefault()
        }

        function handle_drop(e) {
            console.log('handle_drop-当元素在目的地放下时触发')
            var t = Date.now()
            target.innerHTML = ''
            target.append(t + '-拖放触发的事件。')
            e.preventDefault()
        }
    </script>
</body>

</html>
```
[drag-drop事件触发](https://codepen.io/weiqinl/pen/XzRYOw)

在整个拖放过程中，我们以上说的是表面现象，事件过程内部还会发生什么事情呢？请看下面👇的DataTransfer对象。

## DataTransfer对象
与拖放操作所触发的事件同时派发的对象是DragEvent，它派生于MouseEvent，具有Event与MouseEvent对象的所有功能，并增加了dataTransfer属性。该属性用于保存拖放的数据和交互信息，返回DataTransfer对象。
// DataTransfer dataTransfer = DragEvent.dataTransfer
DataTransfer对象定义的属性和方法有很多种，我们看下列入标准的几个。

|属性 |   说明|
| :---| :--- |
|[types](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer/types)    |只读属性。它返回一个我们在dragstart事件中设置的拖动数据格式的数组。 格式顺序与拖动操作中包含的数据顺序相同。IE10+、Edge、safari3.1、Firefox3.5+ 和Chrome4以上支持该属性|
|[files](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer/files)|    返回拖动操作中的文件列表。包含一个在数据传输上所有可用的本地文件列表。如果拖动操作不涉及拖动文件，此属性是一个空列表。|
|[dropEffect](https://developer.mozilla.org/en-US/docs/Web/API/DataTransfer/dropEffect) |   获取当前选定的拖放操作的类型或将操作设置为新类型。它应该始终设置成effectAllowed的可能值之一【none、move、copy、link】。dragover事件处理程序中针对放置目标来设置dropEffect。|
|[effectAllowed](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer/effectAllowed)    |指定拖放操作所允许的效果。必须是其中之一【 none, copy, copyLink, copyMove, link, linkMove, move, all, uninitialized】默认为uninitialized 表示允许所有的效果。ondragstart处理程序中设置effectAllowed属性|

|方法   | 说明|
| :--- | :--- |
|[void setData(format, data)](https://developer.mozilla.org/en-US/docs/Web/API/DataTransfer/setData) |将拖动操作的拖动数据设置为指定的数据和类型。format可以是MIME类型|
|[String getData(format)](https://developer.mozilla.org/en-US/docs/Web/API/DataTransfer/getData)    |返回指定格式的数据，format与setData()中一致|
|[void clearData([format])](https://developer.mozilla.org/en-US/docs/Web/API/DataTransfer/clearData)    |删除给定类型的拖动操作的数据。如果给定类型的数据不存在，此方法不执行任何操作。如果不给定参数，则删除所有类型的数据。|
|[void setDragImage(img, xOffset, yOffset)](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer/setDragImage)    |指定一副图像，当拖动发生时，显示在光标下方。大多数情况下不用设置，因为被拖动的节点被创建成默认图片。x,y参数分别指示图像的水平、垂直偏移量|
```
//IE10及之前版本，不支持扩展的MIME类型名
//Firefox 5版本之前，不能正确的将url和text映射为text/uri-list 和text/plain
var dataTransfer = event.dataTransfer;
//读取文本，
var text = dataTransfer.getData("Text");
//读取URL,
var url = dataTransfer.getData("url") || dataTransfer.getData("text/uri-list");
```
[drag-drop-dataTransfer各属性方法示例](https://codepen.io/weiqinl/pen/aVEard)
## 浏览器支持程度

说了这么多，如果浏览器不支持，也是白扯。

Method of easily dragging and dropping elements on a page, requiring minimal JavaScript.
要求最少的js，实现拖拽页面元素的简单方法
![Drag and Drop 浏览器兼容性.png](http://images2017.cnblogs.com/blog/564792/201711/564792-20171123172447852-1258046031.png)

[drag之浏览器支持程度--caniuse](http://caniuse.com/#search=drag)
￼
**note**
* `dataTransfer.items` 只有Chrome支持
* `dropzone`属性，目前没有浏览器支持
* Firefox支持`.setDragImage`任何类型的DOM元素。Chrome必须有`HTMLImageElement`或者任何DOM元素，该DOM元素附加到DOM 和浏览器的`.setDragImage`视口(viewport)内。
    1.部分支持是指不支持`dataTransfer.files` 或者 `.types`对象
    2.部分支持是指不支持`.setDragImage`
    3.部分支持是指`dataTransfer.setData / getData` 的有限支持格式

以下，我在实际中遇到的情况，各浏览器对标准的实现还是有差异的。
* `getData()`在chrome 62.0浏览器中，只能在`drop`事件中生效。
* 如果使用`setDragImage`方法，指定的图像不存在，则拖动过程：
    1. safari 11.0.1 浏览器，只会触发`dragstart`和`dragend`事件。
    2. chrome、opera 和 Firefox会正常触发其他事件。
* 每一次拖放操作，Firefox都会执行一次新开一个页面的动作，并且自动搜索`dataTransfer.getData()`得到的内容。
解决方法，在`drop`事件中，添加： `e.stopPropagation();// 不再派发事件。解决Firefox浏览器，打开新窗口的问题`。
* opera 49版本中，链接默认不可以拖动，必须把`draggable`属性设置为`true`，才可以拖动。
* 关于 `dropEffect` 和 `effectAllowed` 。
    1. `effectAllowed` 允许拖放操作的效果，最多不会超过那么几种。`dropEffect` 设置拖放操作的具体效果，只能是四种可能之一。
    2. 如果`effectAllowed`设置为`none`，则不允许拖放元素。但是各个浏览器能触发的事件不一样。（注意：safari可以拖放元素，而且会触发所有事件）
    3. 如果`dropEffect`设置为`none`，则不允许被拖放到目的地元素中。
    4. 如果设置了`effectAllowed`的值，那么如果要设置`dropEffect`的值，其值必须和`effectAllowed`的值一致，否则拖动效果无效，而且不允许将被拖放元素放到目的地元素中。(注：safari11.0.1有效果，而且也能拖动到目的地元素中，但是这不符合标准)。

## 示例
[drag-drop-dataTransfer各属性方法示例](https://codepen.io/weiqinl/pen/aVEard) 
[drag-drop事件触发](https://codepen.io/weiqinl/pen/XzRYOw)
## 总结
原生HTML5拖拽API，drag && drop 在实际工作中，还是有很多情况下会遇到的。
以上，我只介绍了部分常用API。API不复杂，多看会儿，实践就知道了。各个浏览器，可能会在表现上，稍有不同，但我相信大家还是会向着标准发展的。