---
layout: post
title:  node相关资料
date:   2017-03-18
excerpt: node的require模块及路径，定时器，express的next函数，express与koa对比
category: backend
---

[express与koa对比](http://blog.csdn.net/k616358281/article/details/71602055)

[express的next函数](https://www.cnblogs.com/aishangliming/p/6115359.html)

[nodejs的require模块及路径](https://www.cnblogs.com/pigtail/archive/2013/01/14/2859929.html)


[nodejs中mysql用法](http://cache.baiducontent.com/c?m=9f65cb4a8c8507ed19fa950d100b92235c4380146d8b804b2281d25f93130a1c187bb4e86c635758ce87616006ae4f5bedf62172405966e8c5dccd179ded9d7472de7023716cde0005d368f08007669f37902be8ae1be3&p=8264c64ad49411a05bed9560615f88&newp=c064c54ad5c340be12be9b7c5c568b231610db2151d4da176b82c825d7331b001c3bbfb423251003d1c77d6505ae4b5ee8fa36753d0425a3dda5c91d9fb4c57479996d&user=baidu&fm=sc&query=nodejs+mysql+3306&qid=dc335bdf0001cea0&p1=2)

[Node 定时器详解](http://www.ruanyifeng.com/blog/2018/02/node-event-loop.html)

同步任务早于异步任务；

本轮循环（process.nextTick和Promise的回调函数）早于次轮循环（setTimeout、setInterval、setImmediate的回调函数）；

同步任务 > process.nextTick()任务 > 微任务(Promise)

同步任务 > 发出异步请求 > 规划定时器生效的时间 > 执行process.nextTick()等等 > 事件循环(6阶段)

- timers，定时器阶段，处理setTimeout()和setInterval()的回调函数
- I/O callbacks，其他的回调函数
- idle, prepare，只供 libuv 内部调用
- poll，轮询时间
- check，执行setImmediate()的回调函数
- close callbacks，比如socket.on('close', ...)
