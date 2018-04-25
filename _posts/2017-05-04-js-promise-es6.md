---
layout: post
title:  promise 异步编程
excerpt: Promise 对象用于表示一个异步操作的最终状态（完成或失败），以及其返回的值。pending 初始状态，既不是成功，也不是失败状态；fulfilled 成功完成（settled，resolved）；rejected 操作失败（settled）。
category: frontend
---

```
// 属性
Promise.prototype
// 方法
Promise.prototype.catch()
Promise.prototype.finally()
Promise.prototype.then()
// 参数为Promise对象数组
Promise.all() // 全 resolved，或任意一个 regected
Promise.race() // 任意一个改变状态
// 用来包装一个现有对象，将其转变为Promise对象
Promise.reject()
Promise.resolve()
```
[javascript promise 迷你书](http://liubin.org/promises-book/#chapter1-what-is-promise)

- then() 返回一个 forked Promise(分叉的 Promise)
```
var p = new Promise(/*——*/)
p.then(func1); p.then(func2); 
// 不同于
p.then(func1).then(func2);
```
- 第一个 then() 中 return 的会传递给第二个 then() 作参数
```
var p = new Promise(function(resolve, reject){resolve('hello')});
p.then(function(str){}).then(function(str){alert(str);})  //undifined
```

- 只能捕获来自上一级的异常


```
new Promise(function(resolve, reject){resolve('hello')});
.then(
	function (str) {
		throw new Error(‘no’); // 第一个 then 抛出异常时
	},
	undifined
).then(
	undefined,
	function (error) {
		alert (error);  // no 第二个 then 能捕获到该异常
	}
)
```

- 错误回调只有在 promise 被 rejected 或者 promise 自身抛出一个异常时才会被执行
```
new Promise(function(resolve, reject){resolve('hello')});
.then(
	function (str) {
		throw new Error(‘no’); // 抛出异常
	},
	function (error) {
		alert (error);  // undefined 捕获不到 no
	}
)
```
- 错误能被恢复
```
var p = new Promise(function(resolve, reject){
	reject( new Error(‘pebkac’))
});
p.then(
	undefined,
	function (error) { } 
	// 没有重新抛出异常, promise 会认为你已经恢复了该错误
).then(
	function (str) {
		alert (‘I am saved’);  // 弹出
	},
	function (error) {
		alert (‘bad computer’);  // 不弹
	}
)
```
- Promise 能被暂停

在 then() 函数中返回另一个 promise，直到新的 promise 的状态是 resolved解析后，才继续执行旧的

6、resolved 状态的 Promise 不会立即执行
```
var I = 0;
new Promise(function(resolve, reject){
	resolve(); // 立刻resolve
});
.then(
	function () {
		I += 2; // 因为回调是异步的
	}
)
alert(i); // 0
```