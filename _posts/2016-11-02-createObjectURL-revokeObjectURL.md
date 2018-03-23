---
layout: post
title: URL.createObjectURL and URL.revokeObjectURL
excerpt: URL.createObjectURL() 用力创建一个指向该参数对象(File或Blob)的URL，URL.revokeObjectURL() 用来把URL释放。
category: frontend
---

#### URL.createObjectURL
`URL.createObjectURL()`方法会根据传入的参数创建一个指向该参数对象的URL

这个URL的生命仅存在于它被创建的这个文档里

新的对象URL指向执行的File对象或者是Blob对象

参数: File对象或者Blob对象

- File对象：一个文件，比如用input type="file"标签来上传文件,那么里面的每个文件都是一个File对象.

- Blob对象：二进制数据，比如通过new Blob()创建的对象就是Blob对象.又比如,在XMLHttpRequest里,如果指定responseType为blob,那么得到的返回值也是一个blob对象.

#### URL.revokeObjectURL
`URL.revokeObjectURL()`方法会释放一个通过 `URL.createObjectURL()`创建的对象URL

当你已经用过了这个对象URL，然后要让浏览器知道这个URL已经不再需要指向对应的文件的时候，就需要调用这个方法把URL释放。

参数: `objectURL`
一个通过`URL.createObjectURL()`方法创建的对象URL.

HTML代码：
```
<div id="forAppend" class="demo"></div>
```
JS代码：
```
var eleAppend = document.getElementById("forAppend");
window.URL = window.URL || window.webkitURL;
if (typeof history.pushState == "function") {
    var xhr = new XMLHttpRequest();
    xhr.open("get", "/image/1.jpg", true);
    xhr.responseType = "blob";
    xhr.onload = function() {
        if (this.status == 200) {
            var blob = this.response;
            var img = document.createElement("img");
            img.onload = function(e) {
              window.URL.revokeObjectURL(img.src); // 清除释放
            };
            img.src = window.URL.createObjectURL(blob);
            eleAppend.appendChild(img);    
        }
    }
    xhr.send();
} else {
    eleAppend.innerHTML = '<p style="color:#cd0000;">浏览器不支持</p>';    
}
```
