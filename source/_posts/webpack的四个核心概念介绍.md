---
title: webpack的四个核心概念介绍
date: 2017-08-17 18:18:18
tags: webpack
---
## 前言
`webpack` 是一个当下最流行的前端资源的模块打包器。当 `webpack` 处理应用程序时，它会递归地构建一个依赖关系图(`dependency graph`)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成少量的` bundle` - 通常只有一个，由浏览器加载。

![webpack.png](http://images2017.cnblogs.com/blog/564792/201708/564792-20170817175752928-966850760.png)

它是高度可配置的，我们先理解四个核心概念：入口(`entry`)、输出(`output`)、loader、插件(`plugins`)
## 入口(entry)
webpack 创建应用程序所有依赖的关系图。图的起点被称之为入口起点(`entry point`)。入口起点告诉 webpack 从哪里开始，并根据依赖关系图确定需要打包的内容。可以将应用程序的入口起点认为是根上下文或 APP第一个启动文件。

简单规则：每个 HTML 页面都有一个入口起点。单页应用(SPA)：一个入口起点，多页应用(MPA)：多个入口起点。
###  简单语法
用法：`entry: string | Arrary<string>`
```
//webpack.config.js
module.exports = {
  entry:  './src/index.js'
};
```
`entry`属性的单个入口语法，在扩展配置的时候有失灵活性。它是下面的简写：
```
//webpack.config.js
module.exports = {
  entry: {
    main: './src/index.js'
  }
};

```
向`entry`传入一个数组时候，将创建"多个主入口(multi-main entry)"。在你想要多个依赖文件一起注入，并且将他们的依赖导向(graph)到一个"chunk"时候，传入数组的方式就很有用。
```
//webpack.config.js
module.exports = {
  entry:  [
    './src/index.js',
    'babel-polyfill',
  ]
};
```
### 对象语法
用法: `entry: {[entryChunkName: string]: string|Array<string>}`
```
//webpack.config.js
module.export = {
  entry: {
    app: './src/app.js',
    vendors:  './src/vendors.js'
  }
};
```
对象语法会比较繁琐。然而，这是应用程序中定义入口的最可扩展的方式。

 这个配置告诉我们 webpack 从 `app.js` 和 `vendors.js` 开始创建依赖图。这些依赖图是彼此完全分离、互相独立的。这种方式比较常见于，只有一个入口起点（不包括 `vendor`，`vendor`一般都是动态加载的第三方模块。动态加载的模块不是入口起点。）的单页应用程序(single page application)中。不过，为了支持提供更佳 `vendor` 分离能力的 `DllPlugin`。 官方现在不太建议将第三方模块放到`entry.vendors`中。  

对象语法，更常见的应该是多入口应用程序-多页应用(MPA)。
```
//webpack.config.js
module.export = {
  entry: {
    home: "./home.js",
    about: "./about.js",
    contact: "./contact.js"
  }
};
```
此配置，告诉 webpack，我们 需要 3 个独立分离的依赖关系图.
在多页应用中，每当页面跳转时，服务器将为你获取一个新的 HTML 文档。页面重新加载新文档，并且资源被重新下载。

> 根据经验：每个 HTML 文档只使用一个入口起点。

## 输出(output)
将所有的资源(assets)归拢在一起后，还需要告诉 webpack 在哪里打包应用程序。webpack 的 `output` 属性描述了如何处理归拢在一起的代码(bundled code)。`output`选项可以控制webpack如何向硬盘写入编译文件。注意，即使可以存在多个`entry`起点，但只指定一个`output`配置。

### 简单用法 
在 webpack 中配置` output` 属性的最低要求是，将它的值设置为一个对象，包括以下两点：
1. `filename` 用于输出文件的文件名。  
2. 目标输出目录 `path` 的绝对路径。  
 
```
//webpack.config.js
module.exports = {
  entry: './src/index.js',
  output: {
    path: './home/proj/public/assets',
    filename: 'bundle.js'
  }
};
```
此配置将一个单独的 `bundle.js` 文件输出到 `./home/proj/public/assets` 目录中。

### 多个入口起点
如果配置创建了多个单独的 `"chunk"`（例如，使用多个入口起点或使用像 `CommonsChunkPlugin` 这样的插件），则应该使用占位符来确保每个文件具有唯一的名称。

```
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js', //使用占位符
    path: __dirname + '/dist'
  }
}

// 写入到硬盘：./dist/app.js, ./dist/search.js
```

## loader
webpack的目标是，让webpack聚焦于项目中的所有资源(asset), 而浏览器不需要关注考虑这些。 webpack把每个文件(`.css`, `.html`, `.scss`, `.jpg`, `etc.`)都作为模块处理。然而webpack自身只理解JavaScript。  
webpack loader 在文件被添加到依赖图中时，将文件源代码转换为模块。  
  
`loader`可以使你在`import`或"加载"模块时预处理文件。因此，`loader`类似于其他构建工具中“任务(task)”，并提供了处理前端构建步骤的强大方法。

在更高层面，在webpack的配置中`loader`有两个目标：
1. 识别出应该被对应的`loader`进行转换的那些文件。(`test`属性)
2. 转换这些文件，从而使其能够被添加到依赖图中(并且最终添加到bundle中)。(`use`属性)

```
//webpack.config.js
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { 
        test: /\.txt$/,
        use: 'raw-loader' 
      }
    ]
  }
};
```

以上配置中，对一个单独的 `module` 对象定义了 `rules` 属性，里面包含两个必须属性：`test` 和`use`。这相当于告诉 webpack 编译器，当碰到「在 `require()/import` 语句中被解析为 `.txt` 的路径」时，在对它打包之前，先使用 `raw-loader` 转换一下。
> 在webpack配置中定义`loader`时，要定义在`module.rules`中，而不是`rules`。如果不这么做，webpack会给出严重警告

### 示例
例如，你可以使用`loader`告诉webpack加载`css`文件，或者将TypeScript转为JavaScript。为此，首先安装相对应的`loader`:

```
npm install -D css-loader
npm install -D ts-loader
```
然后指示webpack对每个`.css`使用`css-loader`, 以及对所有`.ts`文件使用`ts-loader`:
```
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'ts-loader' }
    ]
  }
};
```

根据配置选项，下面的规范定义了同等的`loader`用法:

```
{test: /\.css$/, use: 'css-loader'}
// 等同于
{test: /\.css$/, use: [{
  loader: 'css-loader'
}]}
// 等同于
{test: /\.css$/, loader: 'css-loader'}
```

`module.rules.loader` 是 `module.rules.use: [ { loader } ]` 的简写。

在应用程序中，有三种使用loader的方式：
- 配置（推荐）：在webpack.config.js文件中指定loader。
- 内联： 在每个`imort`语句中显示指定loader。
- CLI: 在`shell`命令中指定它们。

#### 配置
`module.rules` 允许你在 webpack 配置中指定多个 `loader`。 这是展示` loader `的一种简明方式，并且有助于使代码变得简洁。同时让你对各个` loader` 有个全局概览：

```
module: {
    rules: [{
        test: /\.css$/,
        use: [
            'style-loader',
            { loader: 'scss-loader' },
            {
                loader: 'css-loader',
                options: {
                    modules: true
                }
            }
        ]
    }]
}
```

#### 内联

可以在` import` 语句或任何等效于 "import" 的方式中指定 loader。使用 `!` 将资源中的 loader 分开。分开的每个部分都相对于当前目录解析。

```
 import Styles from 'style-loader!css-loader?modules!./styles.css'
```

通过前置所有规则及使用`!`, 可以对应覆盖到配置中的任意`loader`。
选项可以传递查询参数，例如 `?key=value&foo=bar`，或者一个 JSON 对象，例如 `?{"key":"value","foo":"bar"}`。
>尽可能使用 `module.rules`，因为这样可以减少源码中的代码量，并且可以在出错时，更快地调试和定位 loader 中的问题。
#### CLI
你也可以通过`CLI`使用loader:

```
webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
```

这会对` .jade`文件使用 `jade-loader`，对 `.css`文件使用 `style-loader`和 `css-loader`。
### Loader特性
- loader 支持链式传递。能够对资源使用流水线(pipeline)。一组链式的 loader 将按照先后顺序进行编译。loader 链中的第一个 loader 返回值给下一个 loader。在最后一个 loader，返回 webpack 所预期的 JavaScript。
- loader 可以是同步的，也可以是异步的。
- loader 运行在 Node.js 中，并且能够执行任何可能的操作。
- loader 接收查询参数。用于对 loader 传递配置。
- loader 也能够使用`options` 对象进行配置。
- 除了使用 `package.json` 常见的 `main` 属性，还可以将普通的 npm 模块导出为loader，做法是在 `package.json` 里定义一个 `loader` 字段。
- 插件(plugin)可以为 loader 带来更多特性。
- loader 能够产生额外的任意文件。
loader 通过（loader）预处理函数，为 JavaScript 生态系统提供了更多能力。

### 解析Loader
loader 遵循标准的模块解析。多数情况下，loader 将从模块路径（通常将模块路径认为是` npm install`, `node_modules`）解析。  
loader 模块需要导出为一个函数，并且使用 Node.js 兼容的 JavaScript 编写。通常使用 npm 来管理，也可以将自定义 loader 作为应用程序中的文件。按照约定，loader 通常被命名为 xxx-loader（例如 `json-loader`）。

## 插件-plugins
插件是 wepback 的支柱功能。webpack 自身也是构建于-你在 webpack 配置中用到的**相同的插件系统**之上！
插件目的在于解决 loader无法实现的**其他事**。
由于loader仅在每个文件的基础上执行转换，而插件(plugins) 常用于（但不限于）在打包模块的 “compilation” 和 “chunk” 生命周期执行操作和自定义功能。想要使用一个插件，你只需要 `require()` 它，然后把它添加到 `plugins` 数组中。  
多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 来创建它的一个实例。
### 剖析
webpack 插件是一个具有 `apply` 属性的 JavaScript 对象。`apply`
 属性会被 webpack compiler 调用，并且 `compiler` 对象可在整个编译生命周期访问。通过`Function.prototype.apply`方法，你可以把任意函数作为插件传递(`this` 将指向`compiler`)。我们可以在配置中使用这样的方式来内联自定义插件。

```
//ConsoleLogOnBuildWebpackPlugin.js
function ConsoleLogOnBuildWebpackPlugin() {

};

ConsoleLogOnBuildWebpackPlugin.prototype.apply = function(compiler) {
  compiler.plugin('run', function(compiler, callback) {
    console.log("webpack 构建过程开始！！！");

    callback();
  });
};
```

### 用法
由于插件可以携带参数/选项，你必须在webpack配置中，向`plugins`属性中传入`new`实例。
根据webpack的用法，可以有多种方式使用插件。但是，通过配置的方式使用是比较推荐的做法。
#### 配置 (推荐)
```
//webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]0
};

module.exports = config;
```

#### Node API(不推荐)
即便使用 Node API，用户也应该在配置中传入 plugins 属性。`compiler.apply `并不是推荐的使用方式。

```
// some-node-script.js
  const webpack = require('webpack'); //访问 webpack 运行时(runtime)
  const configuration = require('./webpack.config.js');

  let compiler = webpack(configuration);
  compiler.apply(new webpack.ProgressPlugin());

  compiler.run(function(err, stats) {
    // ...
  });
```

## 总结
以上就是[webpack](https://doc.webpack-china.org/)中比较重要的四个概念，在平时开发过程中会经常遇到。里面还可以有很多的详细配置，需要我们在项目开发的过程中慢慢了解。