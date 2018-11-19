title: Webpack 读书笔记
date: 2016-10-30 12:52:00
tags: [读书笔记, webpack]
categories: [读书笔记]

---

## 安装

```shell
$ npm install webpack -g
```

也可以安装在项目中

```shell
$ npm install webpack -save-dev
```
此时推荐设置shell命令别名

```shell
$ alias npm-exec="PATH=$(npm bin):$PATH"
$ alias webpack="npm-exec webpack"
```

## 基础打包命令

```shell
$ webpack ./entry.js bundle.js
```

## 使用Loader

多种方式使用Loader
+ 在require语句中指定
+ 配置在文件中,默认 webpack.config.js
+ 在命令中指定

### Require级别的Loader

先安装 css-loader 和 style-loader

```shell
npm install css-loader style-loader --save-dev
```

css-loader: CSS样式Loader
style-loader: 用于把样式用style标签的形式插入到dom中

js文件中可以这么写

```js
require("!style!css!./style.css");
```
以上代码,先使用css-loader处理,再传给style-loader

每个Loader用感叹号`!`隔离

Require级别的Loader可以重写, 配置文件中的Loader设定

### 配置文件指定Loader

```js
{
    module: {
        loaders: [
            { test: /\.jade$/, loader: "jade" },
            // => "jade" loader is used for ".jade" files

            { test: /\.css$/, loader: "style!css" },
            // => "style" and "css" loader is used for ".css" files
            // Alternative syntax:
            { test: /\.css$/, loaders: ["style", "css"] },
        ]
    }
}
```

### 命令中指定

```shell
webpack --module-bind jade --module-bind 'css=style!css'
```
这个使用jade-loader指定`.jade`文件, 使用style-loader和css-loader指定`.css`文件

### 查询参数
能以QueryString的方式传递查询参数给Loader自己

**在require中

```js
require("url-loader?mimetype=image/png!./file.png");
```

**在配置文件中**

```js
{ test: /\.png$/, loader: "url-loader?mimetype=image/png" }
```

或者

```js
{
    test: /\.png$/,
    loader: "url-loader",
    query: { mimetype: "image/png" }
}
```

**在命令中**

```shell
$ webpack --module-bind "png=url-loader?mimetype=image/png"
```

### Loader列表
**expose-loader**
暴露模块的 exports 对象到全局上下文

例如:

```js
require('expose?$!expose?jQuery!jquery');
```
暴露 $, jQuery 到全局上下文 window

## Webpack配置
http://webpack.github.io/docs/configuration.html

默认名称 webpack.config.js


```js
module.exports = {
    entry: "./entry.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style!css" }
        ]
    }
};
```
配置好之后, 执行`webpack`即可使用配置文件,

### 多个实体入口

```js
var path = require("path");
module.exports = {
	entry: {
		pageA: "./pageA",
		pageB: "./pageB"
	},
	output: {
		path: path.join(__dirname, "js"),
		filename: "[name].bundle.js",
		chunkFilename: "[id].chunk.js"
	}
}
```

### 查找依赖

Webpack 是类似 Browserify 那样在本地按目录对依赖进行查找的可以构造一个例子, 用 --display-error-details 查看查找过程,例子当中 resolve.extensions 用于指明程序自动补全识别哪些后缀,注意一下, extensions 第一个是空字符串! 对应不需要后缀的情况.

```js
// webpack.config.js
module.exports = {
  entry: './a.js',
  output: {
    filename: 'b.js'
  },
  resolve: {
    extensions: ['', '.js', '.jsx']
  }
}
```



## [bundle-loader](https://github.com/webpack/bundle-loader)
bundle-loader用于为指定的文件创建一个封装的模块, 并且按需加载指定的文件模块; 这个封装模块返回一个函数, 并且加载内部模块之后被调用

```js
require("bundle!./file.js")(function(fileJsExports) {
    console.log(fileJsExports);
});
```

## 使用插件

### CommonsChunkPlugin
把entry之间公用的依赖打包成一个common文件

```js
var webpack = require('webpack');
var CommonsChunkPlugin = new webpack.optimize.CommonsChunkPlugin; 

module.exports = {
    plugins: [
        new CommonsChunkPlugin({
          name: "commons",
          // (the commons chunk name)
        
          filename: "commons.js",
          // (the filename of the commons chunk)
        
          // minChunks: 3,
          // (Modules must be shared between 3 entries)
        
          // chunks: ["pageA", "pageB"],
          // (Only use these entries)
        })
    ]
}
```

高级用法:

```js
var path = require("path");
var CommonsChunkPlugin = require("../../lib/optimize/CommonsChunkPlugin");
module.exports = {
    entry: {
        pageA: "./pageA",
        pageB: "./pageB",
        pageC: "./pageC",
        adminPageA: "./adminPageA",
        adminPageB: "./adminPageB",
        adminPageC: "./adminPageC",
    },
    output: {
        path: path.join(__dirname, "js"),
        filename: "[name].js"
    },
    plugins: [
        new CommonsChunkPlugin({
            name: "admin-commons",
            chunks: ["adminPageA", "adminPageB"]
        }),
        new CommonsChunkPlugin({
            name: "commons",
            chunks: ["pageA", "pageB", "admin-commons"],
            minChunks: 2
        }),
        new CommonsChunkPlugin({
            name: "c-commons",
            chunks: ["pageC", "adminPageC"]
        }),
    ]
}
```
### 明确指定工具库的文件块
把第三方工具库从分离自己应用程序的代码分离开来 

```js
entry: {
  vendor: ["jquery", "other-lib"],
  app: "./entry"
}
new CommonsChunkPlugin({
  name: "vendor",

  // filename: "vendor.js"
  // (Give the chunk a different name)

  minChunks: Infinity,
  // (with more entries, this ensures that no other module
  //  goes into the vendor chunk)
})
```

Hint: In combination with long term caching you may need to use this plugin to avoid that the vendor chunk changes. You should also use records to ensure stable module ids.


## 监听模式

```shell
$ webpack --progress --colors --watch
```

## 开发服务器


```shell
$ npm install webpack-dev-server -g
$ webpack-dev-server --progress --colors
```

webpack-dev-server会在8080端口上绑定一个小型的express服务, 用于服务你的静态资源包括打包的资源文件(自动编译). 当一个打包的资源重新被编译它会自动刷新浏览器页面(SockJS), 可以访问 http://127.0.0.1:8080/webpack-dev-server/ 查看效果.

> 这个开发服务用于webpack的watch模式, 因此它也会阻止webpack把编译后的文件直接写到硬盘. 取而代之, 它会把结果保存在内存中, 并且提供8080端口的访问服务.


## Webpack中, require的五种用法
http://www.cnblogs.com/lvdabao/p/5953884.html


指定配置文件

```shell
webpack --config ./webpack.config.js
```