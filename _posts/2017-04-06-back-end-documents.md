---
layout: post
title: back-end documents
excerpt: <a href="http://nodejs.cn/api/">node 文档</a><br><a href="https://www.npmjs.com.cn">npm 官网</a><br><a href="http://www.expressjs.com.cn/guide/routing.html">express 文档</a><br><a href="http://www.runoob.com/mysql/mysql-data-types.html">mysql</a><br>
category: docs
---

## NPM

[npm 官网](https://www.npmjs.com.cn)

NPM 是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：
- 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
- 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

## Node
[node 文档](http://nodejs.cn/api/)

基于 Chrome V8 引擎的 JavaScript 运行时。Node.js使用高效、轻量级的事件驱动、非阻塞 I/O 模型。它的包生态系统，npm，是目前世界上最大的开源库生态系统

## Express

[express 文档](http://www.expressjs.com.cn/guide/routing.html)

Express 是一个自身功能极简，完全是由路由和中间件构成一个的 web 开发框架：从本质上来说，一个 Express 应用就是在调用各种中间件。

从3.x升级到4.x，增加了Router

在3.x中只有最基础的`app[VERB](path, [callback...], callback);`

仅仅只是提供一个接口供调用。

而4.x的Router改动，更为灵活，可以将路由地址拆分为多段处理。比如，你可以把router相关的文件放入到一个文件夹下。然后直接引用：
```
var router = express.Router();

// 这个就是把/user开始的路径统一转发给./routes/user.js处理
router.use('/user', require('./routes/user'));
```
错误处理中间件有 4 个参数(err, req, res, next)，定义错误处理中间件时必须使用这 4 个参数。即使不需要 next 对象，也必须在签名中声明它，否则中间件会被识别为一个常规中间件

express.static(root, [options]) 是 Express 唯一内置的中间件。它基于 [serve-static](https://github.com/expressjs/serve-static)，负责在 Express 应用中提托管静态资源。

[第三方中间件](http://www.expressjs.com.cn/resources/middleware.html)，安装所需功能的 node 模块，并在应用中加载，可以在应用级加载，也可以在路由级加载。
```
var express = require('express');
var app = express();
var cookieParser = require('cookie-parser');

// 加载用于解析 cookie 的中间件
app.use(cookieParser());
```
[mysql 教程](http://www.runoob.com/mysql/mysql-data-types.html)

