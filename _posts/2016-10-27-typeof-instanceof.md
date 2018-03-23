---
layout: post
title: typeof and instanceof
excerpt: 常用来判断一个变量是否为空，或者是什么类型的。Array、Null、dom元素 等用 typeof 一律返回 object；instanceof 可以用于判断一个变量是否某个对象的实例。
category: frontend
---

### typeof
typeof 是一个一元运算，放在一个运算数之前，运算数可以是任意类型。

它返回值是一个字符串，该字符串说明运算数的类型。typeof 一般只能返回如下几个结果：
`number,boolean,string,function,object,undefined`。

对于 Array、Null 等特殊对象使用 typeof 一律返回 object，这是 typeof 的局限性。

可以使用 typeof 来获取一个变量是否存在，不要用 `if (a)` ，因为 a 不存在（未声明）时会出错。

```
if(typeof a!="undefined"){alert("ok")}
```

### instanceof
```
a instanceof b?alert("true"):alert("false"); //a是b的实例？真:假
```
instanceof 用于判断一个变量是否某个对象的实例，如 
```
var a=new Array();
alert(a instanceof Array); // true
alert(a instanceof Object); // true 因为 Array 是 object 的子类

function test(){};
var a=new test();
alert(a instanceof test); // true
``` 

#### function 的 arguments
大家也许都认为 arguments 是一个 Array，但如果使用 instaceof 去测试会发现 arguments 不是一个 Array 对象，尽管看起来很像。

#### 对于 dom 对象
```
var a=new Array();
if (a instanceof Object) {
    alert('Y')
} else {
    alert('N');
}
```
得'Y’
```
if (window instanceof Object) {
    alert('Y');
} else {
    alert('N');
}
```
得'N'

所以，这里的 instanceof 测试的 object 是指 js 语法中的 object，不是指 dom 模型对象。

使用 typeof 会有区别
```
alert(typeof(window)) // object
```
