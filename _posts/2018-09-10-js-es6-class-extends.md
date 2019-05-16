---
layout: post
title:  ES6 - class & extends
excerpt: 类（class）和继承（extends），摘自 <a href="http://es6.ruanyifeng.com">es6入门</a>
category: frontend
---

#### 类
es5
```
function Point(x, y) {
    this.x = x;
    this.y = y;
}

Point.prototype.toString = function () {
    return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```
es6
```
//定义类
class Point {
    constructor(x, y) {    //constructor 构造方法
        this.x = x;
        this.y = y;
    }

    toString() {
        return '(' + this.x + ', ' + this.y + ')';
    }
}


var p = new Point(1, 2);
```

类的所有方法都还是定义在类的prototype属性上面;

一个类必须有constructor方法，如果没有显式定义，一个空的constructor方法会被默认添加.

#### 继承

```
class ColorPoint extends Point {
    constructor(x, y, color) {
        super(x, y); // 调用父类的constructor(x, y)
        this.color = color;
    }

    toString() {
        return this.color + ' ' + super.toString(); // 调用父类的toString()
    }
}
```

super关键字，在这里表示父类的构造函数，用来新建父类的this对象。

子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。

#### 原生构造函数继承
```
class MyArray extends Array {
    constructor(...args) {
        super(...args);
    }
}

var arr = new MyArray();
arr[0] = 12;
arr.length // 1

arr.length = 0;
arr[0] // undefined
```

#### Generator
方法之前加上星号*
```
class Foo {
    constructor(...args) {
        this.args = args;
    }
    * [Symbol.iterator]() {
        for (let arg of this.args) {
            yield arg;
        }
    }
}

for (let x of new Foo('hello', 'world')) {
    console.log(x);
}
// hello
// world
```

#### static
如果在一个方法前，加上static关键字，称为“静态方法”;

不可以被`实例`继承，而是直接通过类来调用。

可以被`子类`继承。

```
class Foo {
    static classMethod() {
        return 'hello';
    }
}

Foo.classMethod() // 'hello'

class Bar extends Foo {}

Bar.classMethod(); // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```
