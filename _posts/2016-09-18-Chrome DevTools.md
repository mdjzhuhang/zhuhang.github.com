---
layout: post
title: Chrome DevTools
excerpt:  调试移动设备Brower Page Tabs/WebViews，chrome://inspect（要翻墙），钩选中 Discover USB devices
Category: tools
---

#### 实时屏幕投影（Live screencasting）
screencast实现了移动设备屏幕与开发环境DevTools的同步，你可以通过screencast与移动设备上的内容进行交互式的操作。

Screencast只呈现页面的内容，而不显示工具条地址栏、设备键盘等其他设备接口，这些在Screencase中表现为透明部分。
#### 端口转发（Port forwarding）
用途：本地开服务，手机直接访问本地代码，进行调试。

在移动设备上创建一个监听TCP端口，该端口映射到开发机器特定的TCP端口，两个端口通过USB线路通信，所以这种连接并不依赖于所处网络环境的配置。


    1. 在开发机器的Chrome浏览器打开chrome://inspect
    2. 点击PortForwarding，弹出设置窗口
    3. 在设备端口输入框，填写移动设备要监听的端口号（默认为8080）
    4. 在Host主机输入域，填写运行web应用的当前ip地址（或主机名称，如localhost）加端口号。IP地址可以填写开发机器可访问的任何本地地址。当前，端口号范围必须在1024~65535之间（包括）
    5. 选中Enableport forwarding
    6. 点击Done完成。
当chrome://inspect窗口的端口号闪动为绿色时，表明该端口转发配置已生效。此时你可以在移动设备上打开本地页面进行调试了。

#### 虚拟主机映射（Virtual hostmapping）
用途：手机访问线上服务，代理本地代码。（主机要和移动设备处在同一网段的局域网内）

移动设备上所有请求被转发到主机的代理服务器，代理服务器代表安卓移动设备来发送请求，将移动设备上的请求映射到主机的正确位置。

搭建代理服务器的工具，例如：Mac上的Charles Proxy，Windows系统下的Fiddler，Linux下的Squid cache。

移动设备代理服务参数设置：

    1.  打开设置-WLAN（Settings> Wi-Fi）
    2.  长按当前连接的无线网络（代理服务设置适用于每个无线网络）
    3.  点击修改网络（Modify network）
    4.  勾选高级选项（Advanced options），完成设置项
    5.  点击“代理”（Proxy）菜单选择“手动”（Manual）
    6.  在“代理服务器主机名”（ Proxyhostname）处输入localhost或者127.0.0.1
    7.  在“代理服务器端口”（Proxy port）处输入端口号，如9000
    8.  点击保存（Save）
