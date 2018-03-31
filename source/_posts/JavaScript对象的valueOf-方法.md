---
title: JavaScript对象的valueOf()方法
date: 2018-03-31 10:08:52
tags:
---

js对象中的`valueOf()`方法和`toString()`方法非常类似，但是，当需要返回对象的原始值而非字符串的时候才调用它，尤其是转换为数字的时候。如果在需要使用原始值的上下文中使用了对象，JavaScript就会自动调用`valueOf()`方法。

`valueOf()`方法是`Object`的原型方法，每个对象都具有该方法，但是各对象返回的值有一定的区别。我们一起来看看。 

## Object.prototype.valueOf()

JavaScript调用`valueOf()`方法将对象转换为原始值。你很少需要自己调用`valueOf()`方法； 

默认情况下，`valueOf()`方法由Object后面的每个对象继承。每个内置的核心对象都会覆盖此方法以放回适当的值。
如果对象没有原始值，则`valueOf()`将返回对象本身。

你可以在自己的代码中使用`valueOf()`将内置对象转换为原始值。创建自定义对象时，可以覆盖`Object.prototype.valueOf()`来调用自定义方法，而不是默认`Object`方法。

### 覆盖自定义对象的`valueOf()`方法
你可以创建一个取代`valueOf()`方法的函数，你的方法必须不能传入参数。
假设你有个对象叫`MyNumberType`而你想为它创建一个`valueOf()`方法。下面的代码为`valueOf()`方法赋予了一个自定义函数:
```
MyNumberType.prototype.valueOf = function() { return customPrimitiveValue; };
```
有了这样的一个方法，下一次每当`MyNumberType`要被转换为原始类型值时，JavaScript在此之前会自动调用自定义的`valueOf()`方法。
`valueOf()`方法一般都会被JavaScript自动调用，但你也可以像下面代码那样自己调用：
```
myNumberType.valueOf()
```

## String.prototype.valueOf()
语法：`strObj.valueOf()`
返回值：表示给定`String`对象的原始值
说明：`valueOf()`方法返回一个`String`对象的原始值，该值等同于`String.prototype.toString()`。
该方法通常在JavaScript内部被调用，而不是在代码里显示调用。
```
let x = new String('Hello world')
console.log(x.valueOf())   // Hello world
```
![String.prototype.valueOf()](http://images2017.cnblogs.com/blog/564792/201801/564792-20180129235451546-2056772811.png)


## Date.prototype.valueOf()
语法：`dataObj.valueOf()`
返回值：表示给定`Date`对象的原始值
说明：`valueOf()`方法返回以数值格式表示的一个`Date`对象的原始值。该值从1970年1月1日0时0分0秒（UTC，即协调世界时）到该日期对象所代表时间的毫秒数。
该方法的功能和`Date.prototype.getTime()`方法一样。
该方法通常在JavaScript内部调用，而不是在代码中显示调用。

```
var x = new Date(2018, 1, 12)
var myVar = x.valueOf()
console.log(myVar) // 1518364800000
```

## Number.prototype.valueOf()
语法： `numObj.valueOf()`
返回值：表示给定`Number`对象的原始值。
说明：该方法通常在JavaScript内部调用，而不是在代码中显示调用。覆盖`Object.prototype.valueOf()`方法
案例：
![Number.prototype.valueOf()](http://images2017.cnblogs.com/blog/564792/201801/564792-20180129235430343-552143799.png)


## Boolean.prototype.valueOf()
语法：`bool.valueOf()`
返回值： 返回给定`Boolean`对象的原始值
说明： `Boolean`的`valueOf()`方法返回一个`Boolean`字面量的原始值作为布尔数据类型。该方法通常在JavaScript内部调用，而不是在代码中显示调用。
案例：
![Boolean.prototype.valueOf()](http://images2017.cnblogs.com/blog/564792/201801/564792-20180129235413140-100092005.png)

## Symbol.prototype.valueOf()
语法： `Symbol().valueOf()`
返回值：返回给定`Symbol`对象的原始值
说明：`Symbol`的`valueOf()`方法返回`Symbol`对象的原始值作为`Symbol`数据类型。JavaScript调用`valueOf()`方法将对象转换为原始值。你很少需要自己调用`valueOf()`方法。当遇到期望有原始值的对象时，JavaScript会自动调用它。
案例：
![ Symbol.prototype.valueOf()](http://images2017.cnblogs.com/blog/564792/201801/564792-20180129235355750-1139685875.png)
[完]