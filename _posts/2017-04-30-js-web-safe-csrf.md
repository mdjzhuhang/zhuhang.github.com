---
layout: post
title: CSRF 跨站点请求伪造
excerpt: CSRF（Cross—Site Request Forgery）攻击是攻击者利用用户的身份操作用户帐户的一种攻击方式，通常使用Anti CSRF Token来防御CSRF攻击，同时要注意Token的保密性和随机性。
category: frontend
---

#### 攻击原理及过程

1. 用户C打开浏览器，访问受信任网站A，输入用户名和密码请求登录网站A；

2. 在用户信息通过验证后，网站A产生Cookie信息并返回给浏览器，此时用户登录网站A成功，可以正常发送请求到网站A；

3. 用户未退出网站A之前，在同一浏览器中，打开一个TAB页访问网站B；

4. 网站B接收到用户请求后，返回一些攻击性代码，并发出一个请求要求访问第三方站点A；

5. 浏览器在接收到这些攻击性代码后，根据网站B的请求，在用户不知情的情况下携带Cookie信息，向网站A发出请求。网站A并不知道该请求其实是由B发起的，所以会根据用户C的Cookie信息以C的权限处理该请求，导致来自网站B的恶意代码被执行。 


#### CSRF漏洞检测

检测CSRF漏洞是一项比较繁琐的工作，最简单的方法就是抓取一个正常请求的数据包，去掉Referer字段后再重新提交，如果该提交还有效，那么基本上可以确定存在CSRF漏洞。

检测的工具，如CSRFTester，CSRF Request Builder等。

#### 防御策略
- 尽量使用POST，限制GET--增加一点攻击难道
- 浏览器Cookie策略--有些浏览器会拦截cookie，降低风险
- 加验证码--辅助手段
- Referer Check--监控CSRF攻击的发生
- Anti CSRF Token--常用方法，前提是没xss漏洞（跨站脚本攻击 Cross Site Scripting）

#### Referer 是什么
HTTP 头中有一个字段叫 Referer，它记录了该 HTTP 请求的来源地址。

缺点：对于某些浏览器，比如 IE6 或 FF2，目前已经有一些方法可以篡改 Referer 值。

缺点：用户自己可以设置浏览器使其在发送请求时不再提供 Referer。

#### 在请求地址中添加 token

在 HTTP 请求中以参数的形式加入一个随机产生的 token，并在服务器端建立一个拦截器来验证这个 token，如果请求中没有 token 或者 token 内容不正确，则认为可能是 CSRF 攻击而拒绝该请求。

这种方法要比检查 Referer 要安全一些，token 可以在用户登陆后产生并放于 session 之中，然后在每次请求时把 token 从 session 中拿出，与请求中的 token 进行比对。

缺点：对于动态生成的 html 代码，需要程序员在编码时手动添加 token
，比较麻烦。

缺点：难以保证 token 本身的安全，点击外站链接，token可能会透过 Referer 泄露到其他网站。

#### 在 Ajax 的 HTTP 头中加token

通过 XMLHttpRequest 这个类，可以一次性给所有该类请求加上 csrftoken 这个 HTTP 头属性，并把 token 值放入其中。

这样解决了上种方法在请求中加入 token 的不便，同时，通过 XMLHttpRequest 请求的地址不会被记录到浏览器的地址栏，也不用担心 token 会透过 Referer 泄露到其他网站中去。

缺点：并非所有的请求都适合用这个类来发起。

参考：
[Web安全之CSRF攻击](http://www.cnblogs.com/lovesong/p/5233195.html)
[CSRF攻击与防御](http://blog.csdn.net/stpeace/article/details/53512283)