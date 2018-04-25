---
layout: post
title:  this 指向
excerpt: 全局范围内：Window<br>对象的构造函数内：new一个对象，this指向这个实例<br>对象的方法内：对象的任何方法内的this都是指向对象本身<br>简单函数内：Window，即使在对象的方法中调用简单函数<br>箭头函数内：与箭头函数所在作用域this相同 <br>事件侦听器内：触发这个事件的元素
category: frontend
---

#### 箭头函数的this
箭头函数不绑定自己的 this，arguments，super或 new.target。箭头函数会捕获其所在上下文的 this 值，作为自己的 this 值。

普通情况下，this指向调用函数时的对象。在全局执行时，则是全局对象。

箭头函数的this，因为没有自身的this，所以this只能根据作用域链往上层查找，直到找到一个绑定了this的函数作用域（即最靠近箭头函数的普通函数作用域，或者全局环境），并指向调用该普通函数的对象。

或者从现象来描述的话，即箭头函数的this指向声明函数时，最靠近箭头函数的普通函数的this。但这个this也会因为调用该普通函数时环境的不同而发生变化。导致这个现象的原因是这个普通函数会产生一个闭包，将它的变量对象保存在箭头函数的作用域中。

#### bind改变this指向
```
const sayThis = function() {
    console.log(this);
}
const boundFunc = sayThis.bind({hippy: 'hipster'});
boundFunc(); // {hippy: 'hipster'}


const sayParams = (...args) => console.log(...args);
const boundFunc = sayParams.bind(null, 1, 2, 3, 4, 5);
boundFunc(); // 1 2 3 4 5


function LeetSpeaker(elem) {
    return {
        listenClick() {
            // Binds this.speakLeet with a reference to the instance.
            // Sets bound function to this.listener, 
            // so we can remove it later.
            this.listener = this.speakLeet.bind(this);
            elem.addEventListener('click', this.listener);
        },
        speakLeet(e) {
            console.log(`1337 15 4W350M3`);

            // Gets the element so we can remove the event listener.
            const elem = e.currentTarget;

            // Removes the event listener.
            elem.removeEventListener('click', this.listener);
        }
    }
}
```