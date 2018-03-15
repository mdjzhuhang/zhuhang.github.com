---
layout: post
title: Web端即时通讯技术
excerpt: 主流的Web端即时通讯方案 Ajax，Comet，WebSocket，SSE。Websocket是基于HTTP协议的，或者说借用了HTTP的协议来完成一部分握手。
category: frontend
---


http 协议被动性，服务端不能主动联系客户端，只能有客户端发起。非状态性，每次都要重新传输identity info（鉴别信息），来告诉服务端你是谁。

keep-alive connection 是指在一次 TCP 连接中完成多个 HTTP 请求，但是对每个请求仍然要单独发 header；

polling 是指从客户端（一般就是浏览器）不断主动的向服务器发 HTTP 请求查询是否有新数据；

#### 要解决3个问题：

1、双全工通信：

即达到浏览器拉取（pull）服务器数据，服务器推送（push）数据到浏览器；

2、低延迟：

即浏览器A发送给B的信息经过服务器要快速转发给B，同理B的信息也要快速交给A，实际上就是要求任何浏览器能够快速请求服务器的数据，服务器能够快速推送数据到浏览器；

3、支持跨域：

通常客户端浏览器和服务器都是处于网络的不同位置，浏览器本身不允许通过脚本直接访问不同域名下的服务器，即使IP地址相同域名不同也不行，域名相同端口不同也不行，这方面主要是为了安全考虑。


[新手入门贴：详解Web端即时通讯技术的原理](http://www.52im.net/thread-338-1-1.html)

#### 主流的Web端即时通讯方案大致有4种

- 传统Ajax短轮询

脚本发送的http请求

- Comet技术

基于http长连接的“服务器推” hack 技术。

实现主要有两种方式，基于Ajax的长轮询（long-polling）方式和基于 Iframe 及 htmlfile 的流（http streaming）方式。

- WebSocket

一个全新的、独立的协议，基于TCP协议，与http协议兼容、却不会融入http协议，仅仅作为html5的一部分。

Websocket在建立连接之前有一个Handshake（Opening Handshake）过程，在关闭连接前也有一个Handshake（Closing Handshake）过程，建立连接之后，双方即可双向通信。
[WebSocket 是什么原理？为什么可以实现持久连接？](https://www.zhihu.com/question/20215561)

- SSE（Server-sent Events）

服务器推送事件，是 HTML 5 规范中的一个组成部分，可以用来从服务端实时推送数据到浏览器端。相对于与之类似的 COMET 和 WebSocket 技术来说，服务器推送事件的使用更简单，对服务器端的改动也比较小。

[Ajax，Comet，WebSocket，SSE](http://www.52im.net/thread-336-1-1.html)

对于现代浏览器（IE10+，chrome14+，Firefox10+，Safari5+以及Opera12+）都是能够很好的支持WebSocket的，其余低版本浏览器通常使用基于XHR（XDR）的polling（streaming）或者是基于iframe的的polling（streaming），对于IE6\7来讲，它不仅不支持XDR跨域，也不支持XHR跨域，所以只能够采取jsonp-polling的方式



### SSE 是什么

SSE （ Server-sent Events ）是 WebSocket 的一种轻量代替方案，使用 HTTP 协议。

严格地说，HTTP 协议是没有办法做服务器推送的，但是当服务器向客户端声明接下来要发送流信息时，客户端就会保持连接打开，SSE 使用的就是这种原理。

### SSE & WebSocket

WebSocket：双工通道，二进制协议

SSE：单向通道，文本协议（通常使用UTF-8编码）

> 便利，不需要添加任何新组件，用任何习惯的后端语言和框架就能继续使用，不用为新建虚拟机弄一个新的IP或新的端口号而劳神。

> 服务器端的简洁。因为SSE能在现有的HTTP/HTTPS协议上运作，所以它能够直接运行于现有的代理服务器和认证技术。


[SSE：使用 HTTP 做数据推送应用](https://www.v2ex.com/t/379577)