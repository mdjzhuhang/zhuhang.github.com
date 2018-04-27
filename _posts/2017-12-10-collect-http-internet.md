---
layout: post
title: HTTP 文章收藏
excerpt: to be continued…<br> 网络相关知识。
category: frontend
---

[HTTP 协议入门](http://www.ruanyifeng.com/blog/2016/08/http.html) 

[TCP 协议简介](http://www.ruanyifeng.com/blog/2017/06/tcp-protocol.html)

[HTTPS 升级指南](http://www.ruanyifeng.com/blog/2016/08/migrate-from-http-to-https.html)

[HTTPS加密原理](https://www.cnblogs.com/Yfling/p/6670495.html)

[HTTP、SSL/TLS和HTTPS协议的区别与联系](http://www.mahaixiang.cn/internet/1522.html)

[HTTP/2 服务器推送（Server Push）教程](http://www.ruanyifeng.com/blog/2018/03/http2_server_push.html)

HTTP/2 协议的主要目的是提高网页性能。

头信息（header）原来是直接传输文本，现在是压缩后传输。原来是同一个 TCP 连接里面，上一个回应（response）发送完了，服务器才能发送下一个，现在可以多个回应一起发送。

服务器推送（server push）是 HTTP/2 协议里面，唯一一个需要开发者自己配置的功能。其他功能都是服务器和浏览器自动实现

[图解SSL/TLS协议](http://www.ruanyifeng.com/blog/2014/09/illustration-ssl.html)

[HTTPS的七个误解](http://www.ruanyifeng.com/blog/2011/02/seven_myths_about_https.html)

[DNS 原理入门](http://www.ruanyifeng.com/blog/2016/06/dns.html) -- 根据域名查出IP地址

[互联网协议入门（一）](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)

[互联网协议入门（二）](http://www.ruanyifeng.com/blog/2012/06/internet_protocol_suite_part_ii.html)

[理解ARP协议以及IP与MAC地址的关系](http://blog.csdn.net/mbuger/article/details/73861017)

[关于URL编码](http://www.ruanyifeng.com/blog/2010/02/url_encoding.html)

HTTP并不是一种传输层的“传输协议”（第四层），而是一种应用层的“转移协议”（最高层）。

[SOAP](http://baijiahao.baidu.com/s?id=1576952989073495443&wfr=spider&for=pc) ：面向活动，HTTP + XML = SOAP。

为了包装RPC(Remote Procedure Call) 的请求信息，推出了XML-RPC，但XML-RPC只能使用有限的数据类型种类和一些简单的数据结构。于是就出现了SOAP

SOAP（Simple Object Access Protocol）简单对象存取协议，是基于 XML 的结构化数据交换。SOAP可以和多种传输协议绑定（Binding），如包括超文本传输协议（ HTTP），简单邮件传输协议（SMTP），多用途网际邮件扩充协议（MIME）使用底层协议交换信息，如： HTTP。

理论上，SOAP就是一段xml，你可以通过HTTP，SMTP等发送它(复制到软盘上，叫快递公司送去也行)。

WSDL是基于SOAP通信时的描述语言，WSDL是用来描述SOAP的，也是一段xml

[SOAP webserivce 和 RESTful webserivce](https://blog.csdn.net/miqi770/article/details/51556279)

[WebService的两种方式Soap和Rest比较](https://www.cnblogs.com/yourshj/p/5968871.html)

[RESTful](https://blog.csdn.net/wangyanchao000/article/details/55047806)：Representational State Transfer（表现层状态转化）。面向资源，必须通过统一的接口来对资源执行各种操作。对于每个资源只能执行一组有限的操作。