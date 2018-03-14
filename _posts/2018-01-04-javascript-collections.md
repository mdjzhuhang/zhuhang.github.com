---
layout: post
title:  javascript 收藏
excerpt: to be conyinued… <br>常见的js错误, 错误处理和堆栈追踪，私有变量，隐式转换，原生JS数据绑定，代理和反射，ES6十大特性，闭包
category: frontend
---

- [10种最常见的Javascript错误](https://elevenbeans.github.io/2018/02/05/top-10-javascript-errors/)

各种未定义，空对象，跨域，死循环


- [JavaScript错误处理和堆栈追踪](https://github.com/dwqs/blog/issues/49)

(函数的)调用栈是怎么工作的？

被调用时，被压入到堆栈的顶部，运行完成之后，从堆栈的顶部被移除。

Error对象和错误处理：
```
try...catch，
try...finally，
try...catch...finally
```

- [JavaScript中的私有变量](https://juejin.im/post/5a8e9b6d5188257a5f1ed826)

如何保护那些不应该在运行时被改变的数据？

命名约定，WeakMap，Symbol，闭包，Proxy

[你所忽略的js隐式转换](https://juejin.im/post/5a7172d9f265da3e3245cbca)

[原生JS数据绑定](http://zcfy.cc/article/native-javascript-data-binding)

- [代理(Proxy)和反射(Reflection)](http://web.jobbole.com/92921/)

调用 new Proxy() 可创建代替其他目标(target)对象的代理，它虚拟化了目标，所以二者看起来功能一致。

代理可以拦截JS引擎内部目标的底层对象操作，这些底层操作被拦截后会触发响应特定操作的陷阱函数。

反射API以 Reflect 对象的形式出现，对象中方法的默认特性与相同的底层操作一致，而代理可以覆写这些操作，每个代理陷阱对应一个命名和参数都相同的 Reflect 方法

- [ES6十大特性](http://web.jobbole.com/86984/)

Default Parameters（默认参数）

Template Literals （模板文本）

Multi-line Strings （多行字符串）

Destructuring Assignment （解构赋值）

Enhanced Object Literals （增强的对象文本）

Arrow Functions （箭头函数）

Promises in ES6

Block-Scoped Constructs Let and Const（块作用域构造Let and Const）

Classes（类） in ES6

Modules（模块） in ES6


- [JavaScript 和 ECMAScript 的区别](http://web.jobbole.com/92968/)

JavaScript，一种通用目的的脚本语言，遵循 ECMAScript 规范。它是 ECMAScript 语言的一个分支版本。

JavaScript 实现了多数 ECMA-262 中描述的 ECMAScript 规范，但存在少数差异。

JavaScript引擎是能够理解和执行 JavaScript 代码的程序或解释器。


- [闭包的十种形式](https://www.cnblogs.com/xiaohuochai/p/6834565.html)

闭包就是能够读取其他函数内部变量的函数，可以理解成“定义在一个函数内部的函数“。在本质上，闭包是将函数内部和函数外部连接起来的桥梁。

