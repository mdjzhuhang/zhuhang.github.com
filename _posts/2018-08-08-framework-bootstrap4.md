---
layout: post
title: bootstrap 4
excerpt: <a href="https://getbootstrap.com/docs/4.1/getting-started/introduction/">bootstrap 4</a>：Build responsive, mobile-first projects ... toolkit for developing with HTML, CSS, and JS.
category: frontend
---

#### 引入 css

before all other stylesheets。
```
<link rel="stylesheet"
href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css"
integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO"
crossorigin="anonymous">
```

[integrity 属性](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity/)

指定浏览器提取的资源（文件）的base64编码的加密哈希值。可以防止资源被篡改。

integrity 的值为：特定散列算法（前缀sha256，sha384和sha512），短划线，实际base64编码散列。

[crossorigin 属性](https://www.chrisyue.com/what-the-hell-is-crossorigin-attribute-in-html-script-tag.html)

"anonymous"会发起一个跨域请求(即包含 Origin: HTTP 头). 但不会发送任何认证信息 (即不发送 cookie, X.509 证书和 HTTP 基本认证信息).

如果服务器没有给出源站凭证 (不设置 Access-Control-Allow-Origin: HTTP 头), 这张图片就会被污染并限制使用.

#### 引入 js

jQuery, [Popper.js](https://www.cnblogs.com/kidsitcn/p/8987715.html), bootstrap.min.js，顺序不可变。

right before the closing `</body>` tag
#### template
[Layout docs](https://v4.bootcss.com/docs/4.0/layout/overview/) ，
[official examples](https://v4.bootcss.com/docs/4.0/examples/)

使用 HTML5 doctype

响应式 meta 标签
```
<meta name="viewport" content="width=device-width,
initial-scale=1, shrink-to-fit=no">
```

盒模型，全局 [box-sizing](https://css-tricks.com/box-sizing/) 使用`border-box`，padding 不影响计算。

[Normalize.css](http://necolas.github.io/normalize.css/) 统一设备和浏览器之间的样式

[reboot](https://v4.bootcss.com/docs/4.0/content/reboot/) 修正跨浏览器和设备之间的不一致性，同时对常用的 HTML 元素设置统一的效果。

