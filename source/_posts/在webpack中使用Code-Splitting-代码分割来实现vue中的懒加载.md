---
title: 在webpack中使用Code Splitting--代码分割来实现vue中的懒加载
date: 2017-08-07 17:05:59
tags: [vue, webpack, javascript]
---
当[Vue](https://cn.vuejs.org/index.html)应用程序越来越大，使用[Webpack的代码分割](https://doc.webpack-china.org/guides/code-splitting/)来[懒加载](https://doc.webpack-china.org/guides/lazy-loading/)组件，路由或者[Vuex](https://vuex.vuejs.org/zh-cn/)模块, 只有在需要时候才加载代码。

我们可以在Vue应用程序中在三个不同层级应用懒加载和代码分割:
- 组件，也称为[异步组件](https://cn.vuejs.org/v2/guide/components.html#异步组件)
- 路由
- Vuex 模块

但是他们都有一些共同之处：自webpack2.0版本之后,他们都使用[动态导入](https://github.com/tc39/proposal-dynamic-import)[译者注：也可以参考[这个](https://doc.webpack-china.org/guides/code-splitting/#dynamic-imports)]。
<!-- more -->
## Vue组件中的懒加载
这在Egghead上的["使用Vue异步组件按需加载组件"](https://egghead.io/lessons/load-components-when-needed-with-vue-async-components)视频中有很好的解释。
这就像在注册组件时使用`import`函数一样简单：
```
 Vue.component('AsyncCmp', () => import('./AsyncCmp') 
```
并在本地注册使用:
```
new Vue({
	// ...
	components: {
		'AsyncCmp': () => import('./AsyncCmp')
	}
})
```
通过包装`import`函数进入到箭头函数，Vue将会在它被请求的时候执行加载这个模块。

如果组件导入使用[命名导出](http://2ality.com/2014/09/es6-modules-final.html#named-exports-several-per-module)[译者注：模块的导入导出，可以参考[阮老师的文章](http://es6.ruanyifeng.com/#docs/module)]，则可以在返回的Promise上使用对象解构。
例如，对于来自[KeenUI](https://github.com/JosephusPaye/Keen-UI)的UiAlert组件：
```
...
components: {
  UiAlert: () => import('keen-ui').then(({ UiAlert }) => UiAlert)
}
...
```

## Vue路由中的懒加载
Vue路由器内置支持[懒加载](https://router.vuejs.org/zh-cn/advanced/lazy-loading.html)。这就像用`import`函数导入组件一样简单。看个例子，我们想在 */login* 路由中使用懒加载一个Login组件：
```
// Instead of: import Login from './login'
// 替换: import Login from './login'
const Login = () => import('./login')

new VueRouter({
  routes: [
    { path: '/login', component: Login }
  ]
})
```
 
## 懒加载Vuex模块
Vuex有一种`registerModule`方法可以让我们动态地创建Vuex模块。如果我们考虑到该`import`功能将ES模块作为有效载荷返回promise[原文： If we take into account that import function returns a promise with the ES Module as the payload]，我们可以使用它来懒加载模块：
```
const store = new Vuex.Store()

...

// Assume there is a "login" module we wanna load
// 设想 我们需要加载一个"login"模块
import('./store/login').then(loginModule => {
  store.registerModule('login', loginModule)
})
```
## 结论
Vue和Webpack使用懒加载非常简单。使用您刚刚阅读的内容，您可以从不同方面开始分割应用程序，并在需要时加载应用程序，从而减轻应用程序的初始加载。

ps: 这篇文章，基本上是翻译过来的。感谢作者[Alex Jover](https://alexjoverm.github.io)
原文链接：
[Lazy Loading in Vue using Webpack's Code Splitting](https://alexjoverm.github.io/2017/07/16/Lazy-load-in-Vue-using-Webpack-s-code-splitting/)