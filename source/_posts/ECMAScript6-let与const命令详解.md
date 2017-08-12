---
title: ECMAScript6-let与const命令详解
date: 2017-08-12 11:13:52
tags: [javascript]
---
## 前言
《ECMAScript入门》是一本开源的JavaScript语言教程，全面介绍ECMAScript6新引入的语法特性。
let和const命令，是第一章开始介绍，也是比较基础的知识。我在学习之后，把它总结记录下来，以便自己以后复习查看。 
**以下代码，于Chrome57 DevTools运行**
 **node为6.3版本**

## 先总结
 先总结区别，再分别阐述
### let 与 const 相同点和区别
1：`let`命令用来声明变量，用法类似于`var`，但是所声明的变量。
   `const`声明一个只读的常量，一旦声明，常量的值就不能改变。
   `const`保证的是内存地址不得改动。常量（内存地址保存的是值），复合类型的数据（内存地址保存的是指针）
2：两者都不存在变量提升。
3：两者都有暂时性死区(TDZ)。
4：两者都不允许重复声明。
5：两者为JavaScript新增了块级作用域。两者作用域，只在声明所在的块级作用域有效。
<!-- more -->
## 基本用法
### let命令介绍
`let`命令，用来声明变量。它的用法类似于`var`, 但是所声明的变量，只在`let`命令所在的代码块内有效。
浏览器中的结果 ![let命令](http://upload-images.jianshu.io/upload_images/1808701-f3a757e07cfaec42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码在代码块之中，分别用`let`和`var`声明了两个变量。然后在代码块之外调用这两个变量，结果`let`声明的变量报错，`var`声明的变量返回了正确的值。这表明，`let`声明的变量只在它所在的代码块有效。上面代码在代码块之中，分别用`let`和`var`声明了两个变量。然后在代码块之外调用这两个变量，结果`let`声明的变量报错，`var`声明的变量返回了正确的值。这表明，`let`声明的变量只在它所在的代码块有效。
### let实用举例
 `for`循环的计数器
```
for (let i =0; i < 10; i++) {}
console.log(i) // ReferenceError: i is not defined
```
 浏览器中的结果![for循环的计数器](http://upload-images.jianshu.io/upload_images/1808701-b40ad1bad6eb2dc8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 上面代码中，计数器`i`只在`for`循环体内有效，在循环体外引用就会报错。
 
 下面的代码如果使用`var`, 最后输出的是`10`。
```
var  a  =   [];
for  (var  i  =  0;  i  <  10;  i++)  {
  a[i]  =   function ()  {
    console.log(i);
  };
}
a[6](); // 10
```
浏览器中的结果 ![var- for循环](http://upload-images.jianshu.io/upload_images/1808701-11e13d162f5e3a7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

   上面代码中，变量`i`是`var`命令声明的，在全局范围内都有效，所以全局只有一个变量`i`。每一次循环，变量`i`的值都会发生改变，而循环内被赋给数组`a`的函数内部的`console.log(i)`，里面的`i`指向的就是全局的`i`。也就是说，所有数组`a`的成员里面的`i`，指向的都是同一个`i`，导致运行时输出的是最后一轮的`i`的值，也就是`10`。
 
   如果使用`let`, 声明的变量仅在[块级作用域](#blockscope)内有效，最后输出的是`6`。
 
```
var  a  =   [];
for  (var  i  =  0;  i  <  10;  i++)  {
  a[i]  =   function ()  {
    console.log(i);
  };
}
a[6](); // 6
```
浏览器结果![let-for循环](http://upload-images.jianshu.io/upload_images/1808701-a556a84c1e41a951.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码中，变量`i`是`let`声明的，当前的`i`只在本轮循环有效，所以每一次循环的`i`其实都是一个新的变量，所以最后输出的是`6`。你可能会问，如果每一轮循环的变量`i`都是重新声明的，那它怎么知道上一轮循环的值，从而计算出本轮循环的值？这是因为 JavaScript 引擎内部会记住上一轮循环的值，初始化本轮的变量`i`时，就在上一轮循环的基础上进行计算。
 
另外，`for`循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。
 
```
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```
浏览器结果 ![let-for循环-父子作用域](http://upload-images.jianshu.io/upload_images/1808701-5a6f224a1dc3e5b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码正确运行，输出了3次`abc`。这表明函数内部的变量`i`与循环变量`i`不在同一个作用域，有各自单独的作用域。
 
## 不存在变量提升
`var`命令会发生`变量提升`现象，即变量可以在声明之前使用，值为`undefined`。这种现象多多少少是有些奇怪的，按照一般的逻辑，变量应该在声明语句之后才可以使用。
 
为了纠正这种现象，`let`命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。
 
```
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;
 
// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2; 
```
浏览器结果![不存在变量提升](http://upload-images.jianshu.io/upload_images/1808701-4eb9702e386ba651.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
上面代码中，变量`foo`用`var`命令声明，会发生变量提升，即脚本开始运行时，变量`foo`已经存在了，但是没有值，所以会输出`undefined`。变量`bar`用`let`命令声明，不会发生变量提升。这表示在声明它之前，变量`bar`是不存在的，这时如果用到它，就会抛出一个错误。
## 暂时性死区（TDZ）
只要块级作用域内存在`let`命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。
```
var tmp = 123;
 
if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```
浏览器中的结果![暂时性死区(TDZ)](http://upload-images.jianshu.io/upload_images/1808701-759e74b94c15487a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码中，存在全局变量`tmp`,但是块级作用域内`let`又声明了一个局部变量`tmp`,导致后者绑定这个块级作用域，所以在`let`声明变量前，对`tmp`赋值会报错。
 
ES6明确规定，如果区块中存在`let`和`const`命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。
 
总之，在代码块内，使用`let`命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。
 
```
if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError
 
  let tmp; // TDZ结束
  console.log(tmp); // undefined
 
  tmp = 123;
  console.log(tmp); // 123
} 
```
浏览器结果
![ReferenceError](http://upload-images.jianshu.io/upload_images/1808701-49873f4ad0ada734.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 
![ReferenceError](http://upload-images.jianshu.io/upload_images/1808701-b73cc279b0d4184e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![正常情况](http://upload-images.jianshu.io/upload_images/1808701-078381fe53f0cf35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


上面代码中，在`let`命令声明变量`tmp`之前，都属于变量`tmp`的“死区”。
 
“暂时性死区”也意味着`typeof`不再是一个百分之百安全的操作。
 
```
typeof x; // ReferenceError
let x;
 
```
浏览器结果![typeof不再是一个百分之百安全的操作](http://upload-images.jianshu.io/upload_images/1808701-bd84a9111223a679.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码中，变量`x`使用`let`命令声明，所以在声明之前，都属于`x`的“死区”，只要用到该变量就会报错。因此，`typeof`运行时就会抛出一个`ReferenceError`。
 
作为比较，如果一个变量根本没有被声明，使用`typeof`反而不会报错。
 
```
typeof undeclared_variable // "undefined"
 
```
浏览器结果![typeof对未声明的变量有用](http://upload-images.jianshu.io/upload_images/1808701-9d7d593750050616.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码中，`undeclared_variable`是一个不存在的变量名，结果返回“undefined”。所以，在没有`let`之前，`typeof`运算符是百分之百安全的，永远不会报错。现在这一点不成立了。这样的设计是为了让大家养成良好的编程习惯，变量一定要在声明之后使用，否则就报错。
 
有些“死区”比较隐蔽，不太容易发现。
 
```
function bar(x = y, y = 2) {
  return [x, y];
}
 
bar(); // 报错
 
```
浏览器结果![函数参数TDZ](http://upload-images.jianshu.io/upload_images/1808701-103ff020e54d8964.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码中，调用`bar`函数之所以报错（某些实现可能不报错），是因为参数`x`默认值等于另一个参数`y`，而此时`y`还没有声明，属于”死区“。如果`y`的默认值是`x`，就不会报错，因为此时`x`已经声明了。
 
```
function bar(x = 2, y = x) {
  return [x, y];
}
bar(); // [2, 2]
 
```
浏览器结果![](http://upload-images.jianshu.io/upload_images/1808701-3a5e506b1c8b26fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

另外，下面的代码也会报错，与`var`的行为不同。 
```
// 不报错
var x = x;
 
// 报错
let x = x;
// ReferenceError: x is not defined
 
```
浏览器结果![let定义变量的TDZ](http://upload-images.jianshu.io/upload_images/1808701-519edc14946bb783.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码报错，也是因为暂时性死区。使用`let`声明变量时，只要变量在还没有声明完成前使用，就会报错。上面这行就属于这个情况，在变量`x`的声明语句还没有执行完成前，就去取`x`的值，导致报错”x 未定义“。
 
ES6 规定暂时性死区和`let`、`const`语句不出现变量提升，主要是为了减少运行时错误，防止在变量声明前就使用这个变量，从而导致意料之外的行为。这样的错误在 ES5 是很常见的，现在有了这种规定，避免此类错误就很容易了。
 
总之，暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。
 
## 不允许重复声明
`let`不允许在相同作用域内，重复声明同一个变量。
浏览器结果
![let不允许重复声明1](http://upload-images.jianshu.io/upload_images/1808701-fc62f8b9a63d3720.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![let不允许重复声明2](http://upload-images.jianshu.io/upload_images/1808701-c3ba51139ac69619.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![let不允许重复声明3](http://upload-images.jianshu.io/upload_images/1808701-6b01394911253746.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



 因此，不能在函数内部重新声明参数。
```
function func(arg) {
  let arg; //在chrome浏览器下，不报错。使用node执行，会报错。
}

function func(arg) {
  {
    let arg;//在chrome浏览器下，不报错。使用node执行，也不报错。
  }
}
```
浏览器环境和node执行情况

![浏览器环境下执行](http://upload-images.jianshu.io/upload_images/1808701-69e0b945a528bd31.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![node6.3环境下执行](http://upload-images.jianshu.io/upload_images/1808701-ff50e440ab0de95b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 块级作用域
块级作用域，没有返回值。
### 为什么需要块级作用域?

ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。  

第一种场景，内层变量可能会覆盖外层变量。  
```
var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined

```
浏览器结果![内层变量可能会覆盖外层变量](http://upload-images.jianshu.io/upload_images/1808701-55389e99c599d827.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码的原意是，`if`代码块的外部使用外层的`tmp`变量，内部使用内层的`tmp`变量。但是，函数`f`执行后，输出结果为`undefined`，原因在于变量提升，导致内层的`tmp`变量覆盖了外层的`tmp`变量。

第二种场景，用来计数的循环变量泄露为全局变量。

```
var s = 'hello';

for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}

console.log(i); // 5

```
浏览器结果![用来计数的循环变量泄露为全局变量](http://upload-images.jianshu.io/upload_images/1808701-e89626346bdfaefd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码中，变量`i`只用来控制循环，但是循环结束后，它并没有消失，泄露成了全局变量。

### ES6的块级作用域
`let`实际上为 JavaScript 新增了块级作用域。

```
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}

```
浏览器结果![let块级作用域](http://upload-images.jianshu.io/upload_images/1808701-63ef326fdc2aa06e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面的函数有两个代码块，都声明了变量`n`，运行后输出5。这表示外层代码块不受内层代码块的影响。如果两次都使用`var`定义变量`n`，最后输出的值才是10。

浏览器结果![let块级作用域](http://upload-images.jianshu.io/upload_images/1808701-2c1adffb868fd796.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ES6 允许块级作用域的任意嵌套。

```
{{{{{let insane = 'Hello World'}}}}};

```

上面代码使用了一个五层的块级作用域。外层作用域无法读取内层作用域的变量。

```
{{{{
  {let insane = 'Hello World'}
  console.log(insane); // 报错
}}}};

```
浏览器结果![let块级作用域](http://upload-images.jianshu.io/upload_images/1808701-60b9f71a77f25a33.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

内层作用域可以定义外层作用域的同名变量。

```
{{{{
  let insane = 'Hello World';
  {let insane = 'Hello World'}
}}}};

```

块级作用域的出现，实际上使得获得广泛应用的立即执行函数表达式（IIFE）不再必要了。

```
// IIFE 写法
(function () {
  var tmp = ...;
  ...
}());

// 块级作用域写法
{
  let tmp = ...;
  ...
}
```
### 块级作用域与函数声明

*   允许在块级作用域内声明函数。
*   函数声明类似于`var`，即会提升到全局作用域或函数作用域的头部。
*   同时，函数声明还会提升到所在的块级作用域的头部。

## const命令
### 基本用法
`const`也是用来声明变量，但是声明的是一个只读的常量。一旦声明，常量的值就不能改变。

```
const PI = 3.1415;
PI // 3.1415

PI = 3;
// TypeError: Assignment to constant variable.

```
浏览器结果![const声明之后不可变](http://upload-images.jianshu.io/upload_images/1808701-a0ac032f8df48178.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码表明改变常量的值会报错。

`const`声明的变量不得改变值，这意味着，`const`一旦声明变量，就必须立即初始化，不能留到以后赋值。

```
const foo;
// SyntaxError: Missing initializer in const declaration
```
浏览器结果![const声明之后需要立即初始化](http://upload-images.jianshu.io/upload_images/1808701-c94b2d1e8ca383c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码表示，对于`const`来说，只声明不赋值，就会报错。

`const`的作用域与`let`命令相同：只在声明所在的块级作用域内有效。

```
if (true) {
  const MAX = 5;
}

MAX // Uncaught ReferenceError: MAX is not defined

```
浏览器结果![const声明只在块级作用域有效](http://upload-images.jianshu.io/upload_images/1808701-5345ea3235c3fab7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`const`命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。

```
if (true) {
  console.log(MAX); // ReferenceError
  const MAX = 5;
}

```
浏览器结果![const命令声明的常量不提升](http://upload-images.jianshu.io/upload_images/1808701-6713c6f28482ea8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码在常量`MAX`声明之前就调用，结果报错。

`const`声明的常量，也与`let`一样不可重复声明。

```
var message = "Hello!";
let age = 25;

// 以下两行都会报错
const message = "Goodbye!";
const age = 30;
```
浏览器结果

![const声明的常量不可重复声明1](http://upload-images.jianshu.io/upload_images/1808701-4b2aa30ee52fad97.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![const声明的常量不可重复声明2](http://upload-images.jianshu.io/upload_images/1808701-46e1ae1b8211487c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对于复合类型的变量，变量名不指向数据，而是指向数据所在的地址。 const命令只是保证变量名指向的地址不变，并不保证该地址的数据不变，所以，将一个对象声明为常量必须非常小心。

```
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError:  Assignment to constant variable.
```
浏览器结果
![将一个对象声明为常量需要小心](http://upload-images.jianshu.io/upload_images/1808701-2f8aee36ccadb4c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


上面代码中，常量`foo`储存的是一个地址，这个地址指向一个对象。不可变的只是这个地址，即不能把`foo`指向另一个地址，但对象本身是可变的，所以依然可以为其添加新属性。

## 跨模块常量
上面说过，const声明的常量只在当前代码块有效。如果想设置跨模块的常量，可以采用下面的写法。
```
// constants.js 模块
export const A = 1;
export const B = 3;
export const C = 4;
```
```
//test1.js 模块
import * as constans from './constants';
console.log(constants.A); // 1
console.log(constants.B); // 3
```
```
// test2.js 模块
import {A, B} from './constants';
console.log(A); //1
console.log(B); //3
```
这里其实用到了Module（模块）的导入导出功能。以后会讲到。

## 全局对象的属性
全局对象是最顶层的对象，在浏览器环境指的是`window`对象，在Node指的是`global`对象。ES5之中，顶层对象的属性与全局变量是等价的。
```
window.a = 1;
a //1

a = 2;
window.a //2
```
浏览器结果![全局对象属性](http://upload-images.jianshu.io/upload_images/1808701-f4d82eff1d3b4305.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码中，全局对象的属性赋值与全局变量的赋值，是同一件事。（对于Node.js来说，这一条只对REPL环境适用，模块环境中，全局变量必须显示声明成global对象的属性。）这种规定被市委JavaScript语言的一大问题。因为很容易不知不觉就创建了全局变量。
ES6为了改变这一点，一方面规定，为了保持兼容性，`var`命令和`function`命令声明的全局变量，依旧是全局对象的属性；另一方面规定，`let`命令、`const`命令和`class`命令声明的全局变量，不属于全局对象的属性。也就是说，从ES6开始，全局变量将逐步与顶层对象的属性脱钩。
```
var a = 1;
// 如果在Node的REPL环境，可以写成global.a
// 或者采用通用方法，写成this.a
window.a // 1

let b = 1;
window.b // undefined
```
浏览器结果![全局对象属性看由谁定义](http://upload-images.jianshu.io/upload_images/1808701-69073bbb184afa91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面代码中，全局变量`a`由`var`命令声明，所以它是全局对象的属性；全局变量`b`由`let`命令声明，所以它不是全局对象的属性，返回`undefined`。
 
## 参考
[let 和 const 命令](http://es6.ruanyifeng.com/#docs/let)