---
layout: post
title:  node-xlsx demo
excerpt: 创建node项目步骤，node-xlsx使用demo
category: demos
---

[To github: ](https://github.com/mdjzhuhang/front-end/tree/master/xlsx-demo)

使用的 node-xlsx 模块，获取 excel 里的数据，使用js合并数据，再生成新的 excel。

在项目根目录中新建dist目录，放待整理的excel文件，格式例如：example.xlsx。

#### 创建node项目步骤

1、创建项目文件夹，`$ npm init`，生成  package.json 文件

2、`$ npm install <模块名> --save`，安装 node 模块，使用`--save`会在 package 中加入依赖

3、创建`index.js` 默认入口文件，编写代码，可以 require 引人模块

4、`$ npm uninstall <模块名>`，卸载模块
