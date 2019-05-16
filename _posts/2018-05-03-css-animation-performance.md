---
layout: post
title: CSS3 动画性能
excerpt: 浏览器怎么渲染页面流程，css3动画性能，GPU
category: frontend
---

- 尽可能多的利用硬件能力，如使用3D变形来开启GPU加速
- 尽可能少的使用box-shadows与gradients
- 尽可能的让动画元素不在文档流中，以减少重排

> 浏览器怎么渲染页面？

- 解析 HTML 标签，构造 DOM 树。
- 解析 CSS 样式，解析顺序：浏览器的样式 -> 用户自定义的样式 -> 页面的link标签等引进来的样式 -> 写在style标签里面的内联样式
- 构造 RenderObject 树，叠加 css 属性，在RENDER树中，DOM树中不可见的节点不会生成RenderObject，比如`head`及里面的内容，`display:none ` 的元素。
- Layout
- Paint
- 复合图层化 Composite
- 浏览器使用 GPU 对这些图层进行合成

> css 样式实现最终表现的步骤

Recalculate Style -> Layout -> Paint Setup and Paint -> Composite Layers

> 重排、重绘、重组

`重排(relayout)` ：当 DOM 变化影响了元素的几何属性（宽或高）和位置，浏览器会使渲染树中受影响的部分失效，并重新构造渲染树

`重绘(repaint)` ：浏览器重新绘制受影响的部分到屏幕，通常最花费性能

`重组(recomposite)`：

大多数浏览器通过队列化修改并批量执行来优化重排过程

重排必定导致重绘，而查询属性会强制发生重排

> 为什么推荐在 CSS 动画中使用 webkit-transform: translateX(500px) 的代替使用 left: 500px ？

一方面：transform 直接触发 Composite Layers，不会 Layout、Paint，而 width、left、margin 等触发 Layout

另一方面：浏览器针对 transform 等开启 GPU 加速。
[GPU (Graphics Processing Unit) 图形处理器](http://www.frontopen.com/1463.html)

> 强制触发硬件加速

使用`transform:translateZ(0)`

> 浏览器什么时候会创建一个独立的复合图层呢？

一般在以下几种情况下：
```
1、3D 或者 CSS transform
2、<video> 和 <canvas> 标签
3、CSS filters
4、元素覆盖时，比如使用了 z-index 属性
```

浏览器在页面渲染前为3D动画创建独立的复合图层，而在运行期间为2D动画创建。

父元素要拥有 perspective 属性才能显现3D的效果

[perspective 属性](http://www.jb51.net/css/453686.html)：设置镜头到元素平面的距离

[perspective-origin 属性](http://www.jb51.net/css/462429.html)：规定了镜头在平面上的位置

[backface-visibility  属性](http://www.htmleaf.com/ziliaoku/qianduanjiaocheng/201504151686.html)：定义当元素不面向屏幕时是否可见。

动画过程有闪烁（通常发生在动画开始的时候），可以尝试Hack：`backface-visibility: hidden;`

[css3动画的性能优化](https://www.cnblogs.com/leena/p/6930079.html)

[CSS动画之硬件加速](https://www.w3cplus.com/css3/introduction-to-hardware-acceleration-css-animations.html)

[CSS Animation性能优化](https://www.w3cplus.com/animation/animation-performance.html)

[High Performance Animations](https://www.html5rocks.com/en/tutorials/speed/high-performance-animations/)