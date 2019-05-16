---
layout: post
title: webpack
excerpt: 
category: tools
---

### 一、系统环境
1、已经安装了 node，和 npm

2、全局安装webpack
```
$ npm  install  webpack -g
```
3、全局安装 webpack-dev-server
```
$ npm install webpack-dev-server -g
```


### 二、构建步骤

1、创建package.json文件
```
$ npm init
```
2、在项目中局部安装Webpack作为依赖包
```
$ npm install --save-dev webpack
```
局部安装的webpack，在项目根目录下，`$ ./webpack.js -v`

3、项目根目录下新建webpack.config.js。我们通过这个文件来处理控制webpack，给出我们想要的输出。
- 进行 [config 配置](http://www.cnblogs.com/wmhuang/p/6935361.html)：entry、ouput、module、plugins等属性。
- 安装 loaders（加载器）
```
$ npm install sass-loader --save-dev
$ npm install style-loader --save-dev
$ npm install css-loader --save-dev
$ npm install extract-text-webpack-plugin --save-dev
```

4、开发项目内容

- 创建 src 文件夹：原始数据、js模块
    
- 创建 dist 文件夹：webpack打包生成的js文件
    
- 创建 index.html 作为入口，并在 webpack.config 中配置入口所在目录
    
- 在 index.html 中通过 script 标签引入 bundle.js，因为webpack打包后生产的是bundle.js文件
    
- 在 src 文件夹中写一个业务模块，例如：Greeter.js，返回包含问候信息的html元素的函数
    
- 在 src 文件夹中创建 main.js 作为打包的入口，这 main.js 中引入所需要的模块，如：import './Greeter.js'

5、webpack 打包

- 使用全局安装的 webpack
```
$ webpack src/main.js dist/bundle.js
```
- 使用非全局安装的 webpack
```
$ node_modules/.bin/webpack src/main.js dist/bundle.js
```

- 带--watch，则会动态监听文件的改变并实时打包，输出新bundle.js文件；但不能做到hot replace，即每次webpack编译之后，还需手动刷新浏览器
```
$ webpack --watch
```

6、开本地服务

```
// 开服务
$ webpack-dev-server --open
// 浏览器监听代码的修改，实时刷新
$ webpack-dev-server --hot --inline
```

### 三、webpack-dev-server

webpack-dev-server 是一个小型node.js express服务器

启动后，在目标文件夹中是看不到编译后的文件的，实时编译后的文件都保存到了内存当中。只有执行`$ webpack`才会打包到dist文件夹。

webpack-dev-server 和 webpack-dev-middleware 里 watch 是默认开启的。

[devServer 官方文档](https://webpack.js.org/configuration/dev-server/)

#### 常用配置
可以写在 webpack.config.js 的 devServer中
```
--open 是否打开浏览器

--hot 热替换

--inline 支持dev-server自动刷新的配置

--content-base 在哪个目录启动本地服务器

--quiet 控制台中不输出打包的信息

--compress 开启gzip压缩

--progress 显示打包的进度


```
Webpack1 升级到 Webpack2 之后，要去掉 hot:true，否则开发模式下热加载不起作用：控制台显示“App hot update...”，但页面不自动刷新。

#### 热替换HMR
修改文件后页面是进行局部刷新而不是重载页面。

命令行带--hot时，plugin 加会自动带 webpack.HotModuleReplacementPlugin


#### 两种自动刷新方式：

- iframe mode

在网页中嵌入了一个 iframe ，将我们自己的应用注入到这个 iframe 当中去，因此每次你修改的文件后，都是这个 iframe 进行了 reload

命令行：webpack-dev-server，无需--inline

浏览器访问：http://localhost:8080/webpack-dev-server/index.html

- inline mode

命令行：webpack-dev-server --inline
浏览器访问：http://localhost:8080

### [原理](http://www.cnblogs.com/wmhuang/p/6935361.html)
打包工具的结构应该是tool+plugins的结构。tool提供基础能力，即文件依赖管理和资源加载管理；在此基础上通过一系列的plugins来丰富打包工具的能力。plugins类似互联网+的概念，文件经plugins处理之后，具备了web渲染中的某种优势。

webpack处理文件的过程可以分为两个维度：文件间的关系(加载顺序)和文件的内容（代码优化）

#### 文件内容处理

主要通过loaders和plugins来处理

#### 文件间的关系

主要是通过文件和模块标记方法来实现

理清这个过程得倒推，先看一下经webpack处理后的js文件，下面的例子中主要涉及3个产出文件，manifest是webpack的module管理代码，vendor是第三方类库文件，collection是入口文件，加载的顺序是manifest-》vendor-》collection

