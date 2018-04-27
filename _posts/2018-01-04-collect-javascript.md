---
layout: post
title:  js 文章收藏
excerpt: to be continued… <br>错误处理和堆栈追踪，私有变量，数据绑定，隐式转换，数据类型
category: frontend
---

[JavaScript的隐形成本](https://mp.weixin.qq.com/s?__biz=MzUxMzcxMzE5Ng==&mid=2247485480&amp;idx=1&amp;sn=993c1bc6c1eeefcc60fe23525f92fd61&source=41#wechat_redirect)

[10 种最常见的 Javascript 错误](https://elevenbeans.github.io/2018/02/05/top-10-javascript-errors/): 各种未定义，空对象，跨域，死循环

[JavaScript错误处理和堆栈追踪](https://github.com/dwqs/blog/issues/49)

    (函数的)调用栈是怎么工作的？

    被调用时，被压入到堆栈的顶部，运行完成之后，从堆栈的顶部被移除

    Error对象和错误处理：try...catch，
    try...finally，
    try...catch...finally

    处理堆栈

[JavaScript 中的私有变量](https://juejin.im/post/5a8e9b6d5188257a5f1ed826)

    如何保护那些不应该在运行时被改变的数据？

    命名约定，WeakMap，Symbol，闭包，Proxy

[原生JS数据绑定](http://zcfy.cc/article/native-javascript-data-binding)：Object.defineProperty、get、set

[你所忽略的js隐式转换](https://juejin.im/post/5a7172d9f265da3e3245cbca)

[数据类型和Json格式](http://www.ruanyifeng.com/blog/2009/05/data_types_and_json.html)

    从结构上看，所有的数据（data）最终都可以分解成三种类型:

    标量（scalar）：string、numbers；

    序列（sequence：array、List；

    映射（mapping）：Name/value，又称作散列（hash）或字典（dictionary）。