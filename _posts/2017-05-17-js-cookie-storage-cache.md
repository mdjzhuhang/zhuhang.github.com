---
layout: post
title: 浏览器缓存机制
excerpt: cookie，localStorage，sessionStorage，跨域，浏览器缓存
category: frontend
---

#### cookie
HTTP Cookie（也叫 Web Cookie 或浏览器 Cookie）是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie使基于无状态的HTTP协议记录稳定的状态信息成为了可能。

默认的有效期是浏览器会话期间，作用域是整个浏览器而不仅仅局限于窗口或标签页。若要延长cookie的有效期，可以设置expires\max-age属性。maxAge属性为–1，表示仅当前浏览器内有效。

默认的domain为用哪个域名访问就是哪个。

```
// 设置方法
document.cookie="username=John Doe; expires=Thu, 18 Dec 2013 12:00:00 GMT; path=/";
```
浏览器最多保存300个cookie;为单个web服务器的最多只能保存20个cookie;每个cookie不能超过4000个字节。一个站点在客户端存放的cookie不能超过3K

[理解Cookie和Session机制](https://www.cnblogs.com/andy-zhou/p/5360107.html)

Cookie能跨二级域名来访问，不能跨一级域名来访问。

顶级域名等页面只能设置domain为顶级域名，不能设置为二级域名或者三级域名等等，否则cookie无法生成。
如：

`yangbai.com`能设置domain为`yangbai.com`或者`www.yangbai.com`，但不能设置domain为`login.yangbai.com`，这样cookie不会生成

[Js跨一级域名同步cookie](http://www.cnblogs.com/zhhying/p/4167703.html) ：iframe，window.postMessage

[html5 postMessage解决跨域、跨窗口消息传递](https://www.cnblogs.com/dolphinX/p/3464056.html)


#### Web storage

- localStorage 生命周期：永久；同域下不同标签间共享；数据不会随着Http请求发送到后台服务器

[localstorage跨域解决方案](https://blog.csdn.net/sflf36995800/article/details/53290457) ：iframe，window.postMessage。

在A域和B域下引入C域，所有的读写都由C域来完成，本地数据存在C域下;

- sessionStorage 生命周期：当前窗口或标签页

#### [IndexedDB](http://www.tfan.org/using-indexeddb/)

#### [浏览器缓存机制](https://mangguo.org/browser-cache-mechanism-detailed/)

客户端缓存（即浏览器缓存）：打开一个网页，浏览器会自动下载副本到你电脑上。服务端代码上控制是否需要，即响应头控制。

- 缓存控制头 Cache-Control

- 过期头 (Expires)

- 控制文件是否有修改 Last-Modified/E-Tag

打开新窗口：如果指定cache- control的值为private、no-cache、must-revalidate,那么打开新窗口访问时都会重新访问服务器。而如果指定了 max-age值,那么在此值内的时间里就不会重新访问服务器,例如：Cache-control: max-age=5 表示当访问此网页后的5秒内再次访问不会去服务器

在地址栏回车：如果值为private或must-revalidate,则只有第一次访问时会访问服务器,以后就不再访问。如果值为no-cache,那么每次都会访问。如果值为max-age,则在过期之前不会重复访问。

按后退按扭：如果值为private、must-revalidate、max-age,则不会重访问,而如果为no-cache,则每次都重复访问.

按刷新按扭：无论为何值,都会重复访问 xenical 120mg.

