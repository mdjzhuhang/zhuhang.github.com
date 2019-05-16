---
layout: post
title: Markup Language
excerpt: 如今的HTML5标准制定了两种实现语法HTML和XHTML。HTML不再基于任何特定的标记语言系统，它有自己完整的标准。而XHTML是XML的一个应用。
category: frontend
---

[SGML(Standard Generalized Markup Language,标准通用标记语言)](https://blog.csdn.net/github_39030531/article/details/72854443)，源于GML，是国际上定义电子文档和内容描述的标准。

[SMIL(Synchronized Multimedia Integration Language，同步多媒体集成语言)](https://wiki.mbalib.com/wiki/SMIL)，将多个位于网络不同位置的媒体文件通过它们的URL关联起来形成多媒体文件，在播放时，播放器会自动从它们的存放位置上调用它们。

[SGML/HTML/XML之间的关系详情](https://www.2cto.com/kf/201801/713962.html)
```
SGML：在高端复杂的结构中应用，在规范的制定上很牛逼，和正常人没关系。

XML：是SGML的一个精简的子集，只有SGML 20% 的标记，能完成 80% 的功能，只关心数据以及它的结构的本身，是普遍的数据存储，操作，和传输的工具。

HTML(1-4)：是SGML的一个应用，需要声明DTD，html关注的是浏览器上的显示效果。

XHTML：是用xml重新定义的HTML语言,更加严禁，跨平台，兼容性好，如手机，各浏览器。
```

当HTML5文档使用text/html MIME类型传输时，它将被Web浏览器是为HTML文档处理。   
当HTML5文档使用XML MIME类型，例如application/xhtml+xml传输时，它将被Web浏览器视为XML文档，由XML处理器进行分析。