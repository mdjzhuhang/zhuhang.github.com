---
layout: post
title: uglifyjs
excerpt: <a href="https://github.com/mishoo/UglifyJS">uglifyjs</a>：压缩混淆JS文件，同时会对代码尽可能的优化，使得最后产出的js代码非常小。
category: tools
---

- [token扫描，词法分析](http://rapheal.sinaapp.com/2014/05/14/uglifyjs-tokenizer/) 
- [语法分析，得到一棵AST树](http://rapheal.sinaapp.com/2014/05/15/uglifyjs-ast-parse/#more-680)

AST树的结构是数组嵌套数组，parse-js 移植于一个 Lisp 的项目，获得的AST树类似 Lisp 里边的表，数组套数组：["toplevel", ["stat", [xxxx]]]。

- [Javascript 变量名混淆](http://rapheal.sinaapp.com/2014/05/17/uglifyjs-var-mangle/#more-689)

在语法分析之后得到AST树，UglifyJS 就会开始遍历AST树，然后为某些节点生成作用域信息。接着重新遍历AST树，再混淆里边的变量名.
- [Javascript 代码压缩](http://rapheal.sinaapp.com/2014/05/22/uglifyjs-squeeze/#more-705)


#### 混淆规则
##### 1. 变量名、函数名、标签名，什么时候混淆？
- 只有在作用域链上声明过的名字才可以混淆
- 函数声明时的参数名可以混淆
- catch后边参数列表的名字可以混淆

    静态分析的时候可以知道 catch 块作用域里边声明的变量，所有可以混淆。
- 在使用了with的作用域链上的所有变量名都不能混淆

    with 是在运行时改变了当前作用域链，UglifyJS 在静态分析源代码的时候没法得知运行时的信息。

- 在使用了eval的作用域链上的所有变量名都不能混淆。

##### 2. 怎么混淆
- 考虑到gzip后的JS文件更小，其使用的混淆名字的顺序是：”etnrisouaflchpdvmgybwESxTNCkLAOM_DPHBjFIqRUzWXV$JKQGYZ0516372984″
- 当前作用域的变量名混淆后不能重复，不能是关键字
- 作用域嵌套时，内外层变量名混淆后也不重复


##### 3. UglifyJS混淆的过程
`with_new_scope`函数为AST树枝生成作用域信息。

一个文件就是处于一个全局域的语句块。整个JS文件处于一个全局作用域，UglifyJS 称之为 toplevel。

`ast_walker`函数遍历AST树，调用者可以传递不同的树枝遍历器给它，以实现不同的遍历效果。

`ast_walker`对象是通过`with_walkers`这个API来重写遍历器。

`with_walkers`有四个对AST树的操作，其底层都是需要遍历AST树的，而且每次对树的处理不一样；

`ast_add_scope`为树枝绑定作用域信息，`ast_mangle`把叶子节点名字混淆掉，`ast_squeeze`优化树枝大小，`gen_code`把树枝输出成字符串。

#### 代码压缩规则
##### 1. 压缩表达式

- 表达式预计算;   
- 优化 true 跟 false，用 1，0 替换  
- 根据 && || 短路的特性压缩表达式;  

##### 2. 运算符缩短

- 对于二元操作符 === 以及 !== ，其两个操作数都是string类型或者都是布尔类型的，可以缩短成==以及!=
- 缩短赋值表达式，例如：a = a + b，可以缩短成 a += b
- 非!的压缩

##### 3. 去除没用的声明/引用
- 去除重复的指示性字符串
- 去除没有使用的函数参数
- 去除函数表达式冗余的函数名；
例如：函数体没有引用自身名字递归调用，那么这个函数名可以去除，使之变为匿名函数。
- 去除没用的块
- 去除没有使用的 break
- 去除没有引用的 label
- 去除没有引用的 toString

##### 4. while 压缩
- 去除根本不会执行的 while 循环：while(false){}
- while(true) 变成 for(;;)，可以缩短4个字符

##### 5. 简化条件表达式
##### 6. 压缩语句块
- 连续的表达式语句可以合并成一个逗号表达式
- 逗号 var 声明
- 去除 return，throw，break，continue 之后的非变量声明以及非函数声明的语句
- 合并块末尾的return语句及其前边的多条表达式语句

##### 7. 优化 if/else
- 去除没用的、空的 if/else 分支
- 反转 if/else，看是否缩短
- 内层没有 else 时，合并嵌套的 if
- 如果 if 最后一个语句是跳出控制语句，那么可以把else块的内容提到else外边，然后去掉else
- 简化成条件表达式
- 简化成 || 或者 && 表达式