---
layout: post
title: node与node-sass版本问题
excerpt: Node Sass does not yet support your current environment：OS X 64-bit with Unsupported runtime
category: tools
---


node升级后编译报错：
```
Node Sass does not yet support your current environment: OS X 64-bit with Unsupported runtime
```
因为node-sass不支持Node v9 ，要升级：
```
npm update node-sass --save-dev
```
升级时报错：
```
path /Users/……/node_modules/.bin/node-sass
npm ERR! code EEXIST
npm ERR! Refusing to delete /Users/……/node_modules/.bin/node-sass: is outside /Users/……/node_modules/node-sass and not a link
```
因为.bin里有node-sass相关的日志，要手动删除后才能卸载`node-sass`

然后继续报错
```
……sassgraph……EEXIST
……node-gyp……EEXIST
```
因为node-sass依赖其他模块，所以.bin里相关的日志也要删除

然后重新 `npm update node-sass --save-dev`

成功！


[node-sass release](https://github.com/sass/node-sass/releases)


