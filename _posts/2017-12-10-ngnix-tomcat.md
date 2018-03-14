---
layout: post
title:  Ngnix 和 tomcat 区别
excerpt: 严格的来说，Apache/Nginx 应该叫做 HTTP Server；而 Tomcat 则是一个 Application Server。
category: backend
---


## tomcat
Apache 的项目，有 Sun参与支持。免费的开放源代码的Web 应用服务器，属于轻量级应用服务器。

> 当配置正确时，Apache 为HTML页面服务，而Tomcat 实际上运行JSP 页面和Servlet。

Tomcat和IIS等Web服务器一样，具有处理HTML页面的功能，另外它还是一个Servlet和JSP容器，独立的Servlet容器是Tomcat的默认模式。

不过，Tomcat处理静态HTML的能力不如Apache服务器。目前Tomcat最新版本为9.0。

## ngnix
Nginx （engine x），俄罗斯人开发的高性能HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP服务器。

Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，并在一个BSD-like 协议下发行。

特点：占有内存少，并发能力强

## nginx和tomcat的区别
web 上的 server 都叫 web server，但分工不同。

严格的来说，Apache/Nginx 应该叫做「HTTP Server」；而 Tomcat 则是一个「Application Server」，或者更准确的来说，是一个「Servlet/JSP」应用的容器（Ruby/Python 等其他语言开发的应用也无法直接运行在 Tomcat 上）。

一个 HTTP Server 关心的是 HTTP 协议层面的传输和访问控制，所以在 Apache/Nginx 上你可以看到代理、负载均衡等功能。客户端通过 HTTP Server 访问服务器上存储的资源（HTML 文件、图片文件等等）。通过 CGI 技术，也可以将处理过的内容通过 HTTP Server 分发，但是一个 HTTP Server 始终只是把服务器上的文件如实的通过 HTTP 协议传输给客户端。

而应用服务器，则是一个应用执行的容器。它首先需要支持开发语言的 Runtime（对于 Tomcat 来说，就是 Java），保证应用能够在应用服务器上正常运行。其次，需要支持应用相关的规范，例如类库、安全方面的特性。对于 Tomcat 来说，就是需要提供 JSP/Sevlet 运行需要的标准类库、Interface 等。为了方便，应用服务器往往也会集成 HTTP Server 的功能，但是不如专业的 HTTP Server 那么强大，所以应用服务器往往是运行在 HTTP Server 的背后，执行应用，将动态的内容转化为静态的内容之后，通过 HTTP Server 分发到客户端。