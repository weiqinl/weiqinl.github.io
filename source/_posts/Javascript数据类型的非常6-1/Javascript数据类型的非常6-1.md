---
title: Javascript数据类型的非常6+1
date: 2018-04-17 11:16:01
tags: [javascript, 数据类型]
---
# 动态类型
JavaScript是一种弱类型或者说动态语言。这意味着你不用提前声明变量的类型，在程序运行过程中，类型会被自动确定。这也意味着你可以使用同一个变量保存不同类型的数据

![数据测试图1](/images/43331776.png)
![数据测试图2](/43331776.png)

# 数据类型 
最新的ECMAScript标准定义了7种数据类型；
其中六种基础数据类型(primitive type)：Undefined、Null、Boolean、Number、String、Symbol(es6 新增）。
一种引用类型：Object。
<!-- more -->

使用`typeof`运算符来判断一个值是否在某种类型的范围内。可以用这种运算符判断一个值是否表示一种原始类型：如果它是原始类型，还可以判断它表示哪种原始类型。
使用`instanceof`运算符用来测试一个对象在其原型链中是否存在一个构造函数的 `prototype` 属性。

以下，我们分别看看，基础数据类型有哪些特性，引用类型如何存储判断的。

## 原始值
**除 Object 以外的所有类型的值本身无法被改变，我们称这些类型的值为 `原始值`**。
大多数时候，原始值直接代表语言实现的最底层。
原始值是存储在栈(stack)中的简单数据段，也就是说，它们的值直接存储在变量访问的位置。

ECMA-262把术语类型(type)定义为值的一个集合，每种基础数据类型定义了它包含的值的范围及其字面量表示形式。

### Undefined 类型
Undefined类型只有一个值，即`undefined`。当声明的变量未初始化时，该变量的默认值为`undefined`。
![数据测试图-undefined](33567909.png)

声明的oTemp变量，没有初始值。该变量将被赋予值undefined，即undefined类型的字面量。
oTemp1没有定义，导致报 引用错误。

### Null类型
Null类型只有一个值： `null`。值`null`是一个JavaScript字面量，表示空值(null or an "empty" value)，即没有对象被呈现。
![数据测试图-null](52190561.png)

在这里要说明的是：在JavaScript中，null是基础数据类型，但是typeof判断为object，这是由于历史原因造成的。
在JavaScript最初的实现中，JavaScript中的值是由一个表示类型的标签和实际数据值表示的。对象的类型标签是0。由于`null`代表的是空指针(大多数平台下值为0x00），因此，null使用0作为类型标签，`typeof null`就返回了"object"。
### Boolean 布尔类型
布尔表示一个逻辑实体，可以有两个值： `true` 和 `false`。
![数据测试图-Boolean 布尔类型](55328415.png)

注意点：
由于js中区分大小写，因此`True` 和 `False` 都不是`Boolean`值。

任意的JavaScript的值都可以转换为布尔值。`undefined`,`null`, `0`,`-0`,`NaN`,`""`这6个值,再加上`false`本身共7个值都会转换为`false`

### Number 数字类型
在JavaScript中只有一种数字类型：基于IEEE 754 标准的双精度64位二进制格式的值（ -(253 -1) 到  253 -1）。这种类型可以表示32位的整数，也可以表示64位的浮点数，还能有一些带符号的值：`+Infinity`,`-Infinity`和`NaN`(非数值，Not-a-Number)。
直接输入的任何数字都可以看做Number类型的字面量。例如：
```
var iNum = 86;  // 此代码声明了存放整数值的变量，它的值由字面量86定义。
```
#### 进制数
通常，我们所说的整数都是以十进制来表示。但是，在计算机学科，也有其他进制，比如二进制、八进制、十六进制。
在JavaScript中，能准确识别十进制，十六进制整数。八进制整数要看情况识别。
八进制字面量的首数字必须是0，其后的数字可以是任何八进制数字(0-7)。
```
var iNum = 070; // 070 等于十进制的 56
```
虽然JavaScript的某些实现可以允许采用八进制形式表示整数。但是，ECMAScript 6的严格模式下，八进制字面量是明令禁止的。因此，最好不要使用以0位前缀的整型字面量。
十六进制字面量的首数字必须是0，后面接字母x，然后是任意的十六进制数字（0 到 9 和 A 到 F）。这些字母可以是大写的，也可以是小写的。
```
var iNum = 0x1f; // 0x1f 等于十进制的 31
var iMum = 0xAB; // 0xAB 等于十进制的 171
```
**注意**：尽管所有整数都可以表示为八进制或十六进制的字面量，但所有数学运算返回的都是十进制结果。

#### 浮点数
要定义浮点值，必须包括小数点和小数点后的一位数字（例如，用1.0而不是1）。这被看作浮点数字面量。
```
var fNum = 5.0;
```
**对于浮点字面量，在用它进行计算前，真正存储的是字符串 **

JavaScript中的数字具有足够的精度，并可以极其近似于0.1。但是，数字不能精确表述也会带来一些问题。
![数据测试图-浮点数](41773983.png)


由于四舍五入误差，上述代码中的"x" 实际上并不等于 "y"。这个问题，在任何使用二进制浮点数的编程语言中都会出现。不过，上述代码中的x和y的值非常接近彼此和最终的正确值。这种结果可以胜任大多数计算任务。不相等的问题只有在比较的时候才会出现。

#### 特殊的Number值 
`Number对象`是经过封装的能让你处理数字值的对象。它由`Number()`构造器创建。`Number对象`的属性都是`Number`类型。比如，前两个是`Number.MAX_VALUE` 和 `Number.MIN_VALUE`。它们定义了`Number`值集合的外边界。`Number.MIN_SAFE_INTEGER`和`Number.MAX_SAFE_INTEGER`，分别表示最小的安全整数，最大的安全整数。
`Number.NaN`表示特殊的"非数字"值。以下是一些例子：
![数据测试图-特殊的Number值](49428673.png)

#### NaN
`NaN` 是一个奇怪的特殊值。一般来说，这种情况发生在类型转换失败时。例如，要把单词 blue 转换成数值就会失败，因为没有与之等价的数值。与无穷大一样，`NaN`也不能用于算术计算。`NaN`的另一个奇特之处在于，它与自身不相等，这意味着下面的代码将返回false;
```
alert(NaN == NaN); // false
```
出于这个原因，不推荐使用  `NaN` 值本身。建议使用函数`Number.isNaN()`或者 `isNaN()`：
`isNaN()`是全局函数，会强制将参数转换为数字，用来确定一个值是否为`NaN`，
es6中定义的`Number.isNaN()`函数，不会强制将参数转换为数字，只有在参数是真正的数字类型，且值为`NaN`的时候才会返回`true`。
```
alert(Number.isNaN('blue')); // false
alert(isNaN('blue')); // true
alert(Number.isNaN(666)); // false
alert(isNaN(666)); // false
alert(Number.isNaN(0 / 0)); // true
alert(isNaN(0 / 0); // true
```
![数据测试图-NaN](58823483.png)


### String 字符串类型
JavaScript的字符串类型用于表示文本数据。它是一组16位的无符号整数值的“元素”。在字符串种的每个元素占据了字符串的位置。第一个元素的索引为0，下一个是索引1，依此类推。字符串的长度是它的元素的数量。
字符串类型有个特色叫`immutable`，这意味着字符串一旦被创建，本身是不能被修改的。当使用`String.prototype.methods`做任何操作都不会影响到原字符串，但是，可以基于对原始字符串的操作来创建新的字符串。
```
var str = "test"
str.len = 4   //  这里修改的只是临时对象，之后立马销毁
var t = str.len
console.log(t); // 由于str没有len这个属性，所有会输出 undefined
```
![数据测试图-String 字符串类型](36709921.png)


### Symbol 符号类型
符号(Symbols)是ECMAScript 6新定义的。`Symbol()`函数返回`symbol`类型的值，都是唯一的，不相等的，并且具有静态属性和静态方法。`symbol值`可以用来作为对象属性的标识符，这是`Symbol数据类型`的唯一目的。
在某些语言中也有类似原子类型(Atoms)，你也可以认为它们是C里面的枚举类型。
![数据测试图-Symbol 符号类型](48199609.png)


## Objects
与原始类型对应的是引用类型。引用类型通常叫做类(class)，也就是说，遇到引用值，所处理的就是对象。

引用值是存储在堆(heap)中的对象，也就是说，存储在变量处的值是一个指针(point)，指向存储对象的内存处。
在内存中可以被 标识符引用的一块区域。

ECMAScript中的所有对象都由这个对象继承而来，Object对象中的所有属性和方法都会出现在其他对象中。

**数组**、**对象**、**函数**等都被称为复杂对象。

参考： 

1. [JavaScript数据类型和数据结构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)
2. [ECMAScript原始类型](http://www.w3school.com.cn/js/pro_js_primitivetypes.asp)
3. [ECMAScript引用类型](http://www.w3school.com.cn/js/pro_js_referencetypes.asp)



