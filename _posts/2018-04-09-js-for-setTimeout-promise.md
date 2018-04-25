---
layout: post
title: for循环/setTimeout/Promise
excerpt: 
category: frontend
---


```
for (var i = 0; i < 5; i++) {
  console.log(i);
}
```

0到4

```
for (var i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000 * i);
}
```

5个5

```
for (var i = 0; i < 5; i++) {
  (function(i) {
    setTimeout(function() {
      console.log(i);
    }, i * 1000);
  })(i);
}
```

0到4，闭包

```
for (var i = 0; i < 5; i++) {
  (function(i) {
    setTimeout(function() {
      console.log(i);
    }, i * 1000);
  })();
}
```

5个5，内部没有对 i 保持引用

```
for (var i = 0; i < 5; i++) {
  setTimeout((function(i) {
    console.log(i);
  })(i), i * 1000);
}
```
立刻输出：0到4

立即执行函数，`setTimeout(undifined, i * 1000)`

```
setTimeout(function() {
  console.log(1)
}, 0);
new Promise(function executor(resolve) {
  console.log(2);
  for( var i=0 ; i<10000 ; i++ ) {
    i == 9999 && resolve();
  }
  console.log(3);
}).then(function() {
  console.log(4);
});
console.log(5);
```

setTimeout 在定时结束后将传递这个函数放到任务队列里面

然后是一个 Promise，里面的函数是直接执行的，因此应该直接输出 2 3 

然后，Promise 的 then 应当会放到当前 tick 的最后，但是还是在当前 tick 中。

因此，应当先输出 5，然后再输出 4 

最后在到下一个 tick，就是 1 

2，3，5，4，1

