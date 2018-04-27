---
layout: post
title: PWA and Service Worker
excerpt: （google）渐进式网络应用 ( Progressive Web Apps )
category: frontend
---

[PWA](https://juejin.im/entry/5a1c394a5188255851326da5) - 中文书

[Service Worker toolbox](https://github.com/GoogleChrome/sw-toolbox) - Service Worker 工具库

[Let’s Encrypt](https://letsencrypt.org) - 免费的 HTTPS 证书授权

[OneSignal](https://onesignal.com) - 第三方跨平台推送通知工具

#### Service Workers
事件驱动，能够拦截进出的 HTTP 请求，从而完全控制你的网站。

- 运行在它自己的全局脚本上下文中

- 不绑定到具体的网页

- 无法修改网页中的元素，因为它无法访问 DOM

- 只能使用 HTTPS

[Twitter Light](https://github.com/SangKa/PWA-Book-CN/blob/master/ch02/2.3.md) 项目

[Service Workers 缓存基础](https://github.com/SangKa/PWA-Book-CN/blob/master/ch03/3.2.md)

注册 Service Worker 文件
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Hello Caching World!</title>
  </head>
  <body>
    <!-- 引用 “hello” 图片 -->
    <img src="/images/hello.png" /> 
    <!-- 引用基础的 JavaScript 文件 -->
    <script async src="/js/script.js"></script>
    <script>
      // 注册 service worker
      // 检查当前浏览器是否支持 Service Workers
      if ('serviceWorker' in navigator) {
        navigator.serviceWorker.register('/service-worker.js').then(function (registration) {
          // 注册成功
          console.log('ServiceWorker registration successful with scope: ', registration.scope);
        }).catch(function (err) {
          // 注册失败
          console.log('ServiceWorker registration failed: ', err);
        });
      }
    </script>
  </body>
</html>
```
#### service-worker.js 

缓存资源，监听 install 安装事件
```
//指定一个缓存的名称
var cacheName = 'helloWorld';
//self 代表 ServiceWorkerGlobalScope
self.addEventListener('install', event => {
  event.waitUntil(
    //使用指定的缓存名称打开缓存
    caches.open(cacheName)
    //把 JavaScript 和 图片文件添加到缓存中
    .then(cache => cache.addAll([
      '/js/script.js',
      '/images/hello.png'
    ]))
  );
});
```
读取资源,监听 fetch
```
self.addEventListener('fetch', function (event) {
  event.respondWith(
    //检查传入的请求 URL 是否匹配当前缓存中存在的任何内容
    caches.match(event.request)
    .then(function (response) {
      // 有内容则匹配到了
      if (response) {
        return response;
      }
      // 否则通过网络请求资源
      return fetch(event.request);
    })
  );
});
```
页面和ServiceWorker质检可以通过postMessage方法发送消息，发送的消息可以通过message事件接收到。这是一个双向的过程。

#### manifest.json
[应用清单](https://github.com/SangKa/PWA-Book-CN/blob/master/ch05/5.1.md)，包含当前网站的信息
```
// html 中 head 标签引入
<link rel="manifest" href="/manifest.json">
```
[添加到主屏幕](https://github.com/SangKa/PWA-Book-CN/blob/master/ch05/5.2.md)

浏览器内置，当满足以下条件时，自动提示用户是否加到主屏幕：

    1、需要 manifest.json 文件
    2、清单文件需要启动 URL
    3、需要 144x144 的 PNG 图标
    4、网站正在使用通过 HTTPS 运行的 Service Worker
    5、用户需要至少浏览网站两次，并且两次访问间隔在五分钟之上

#### [推送](https://github.com/SangKa/PWA-Book-CN/blob/master/ch06/6.4.md)
发送推送通知需要三个步骤。首先我们需要提示用户并获得他们的订阅详细信息，然后将这些详细信息保存在服务器上，最后在需要时发送任何消息。

订阅通知
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Progressive Times</title>
    <link rel="manifest" href="/manifest.json">
  </head>
  <body>
    <script>
      var endpoint;
      var key;
      var authSecret;
      // 客户端和服务端都需要公钥，以确保消息是加密过的
      var vapidPublicKey = 'BAyb_WgaR0L0pODaR7wWkxJi__tWbM1MPBymyRDFEGjtDCWeRYS9EF7yGoCHLdHJi6hikYdg4MuYaK0XoD0qnoY';

      // VAPID 规范要求将 VAPID 钥从 base64 字符串转换成 Uint8 数组
      function urlBase64ToUint8Array(base64String) {
        const padding = '='.repeat((4 - base64String.length % 4) % 4);
        const base64 = (base64String + padding)
          .replace(/\-/g, '+')
          .replace(/_/g, '/');
        const rawData = window.atob(base64);
        const outputArray = new Uint8Array(rawData.length);
        for (let i = 0; i < rawData.length; ++i) {
          outputArray[i] = rawData.charCodeAt(i);
        }
        return outputArray;
      }
      if ('serviceWorker' in navigator) {
        navigator.serviceWorker.register('sw.js').then(function (registration) {
          // 获取任何已存在的订阅
          return registration.pushManager.getSubscription()
            .then(function (subscription) {
              if (subscription) {
                // 如果已经订阅过了，则无需再次注册
                return;
              }
              // 还没有订阅过，则显示一个提示
              return registration.pushManager.subscribe({
                  userVisibleOnly: true,
                  applicationServerKey: urlBase64ToUint8Array(vapidPublicKey)
                })
                .then(function (subscription) {
                  //需要从订阅对象中获取 key 和 authSecret
                  var rawKey = subscription.getKey ? subscription.getKey('p256dh') : '';
                  key = rawKey ? btoa(String.fromCharCode.apply(null, new Uint8Array(rawKey))) : '';
                  var rawAuthSecret = subscription.getKey ? subscription.getKey('auth') : '';
                  authSecret = rawAuthSecret ?
                    btoa(String.fromCharCode.apply(null, new Uint8Array(rawAuthSecret))) : '';
                  endpoint = subscription.endpoint;
                  // 最后，将详细信息发送给服务器以注册用户
                  return fetch('./register', {
                    method: 'post',
                    headers: new Headers({
                      'content-type': 'application/json'
                    }),
                    body: JSON.stringify({
                      endpoint: subscription.endpoint,
                      key: key,
                      authSecret: authSecret,
                    }),
                  });
                });
            });
        }).catch(function (err) {
          // 注册失败 :(
          console.log('ServiceWorker registration failed: ', err);
        });
      }
    </script>
  </body>
</html>
```
发送通知

以下服务端代码（Express），创建了一个端点来监听指向 /register 的 POST 请求。这段代码用来保存用户的订阅详情以及向他们发送感谢信息。
```
const webpush = require('web-push');
const express = require('express');
var bodyParser = require('body-parser');
const app = express();
// 设置 VAPID 详情
webpush.setVapidDetails(
  'mailto:contact@deanhume.com',
  'BAyb_WgaR0L0pODaR7wWkxJi__tWbM1MPBymyRDFEGjtDCWeRYS9EF7yGoCHLdHJi6hikYdg4MuYaK0XoD0qnoY',
  'p6YVD7t8HkABoez1CvVJ5bl7BnEdKUu5bSyVjyxMBh0'
);
// 监听指向 '/register' 的 POST 请求
app.post('/register', function (req, res) {
  var endpoint = req.body.endpoint;
  // 保存用户注册详情，这样我们可以在稍后阶段向他们发送消息
  saveRegistrationDetails(endpoint, key, authSecret);
  // 建立 pushSubscription 对象
  const pushSubscription = {
    endpoint: req.body.endpoint,
    keys: {
      auth: req.body.authSecret,
      p256dh: req.body.key
    }
  };
  var body = 'Thank you for registering';
  var iconUrl = 'https://example.com/images/homescreen.png';
  // 发送 Web 推送消息
  webpush.sendNotification(pushSubscription,
      JSON.stringify({
        msg: body,
        url: 'http://localhost:3111/',
        icon: iconUrl
      }))
    .then(result => res.sendStatus(201))
    .catch(err => {
      console.log(err);
    });
});
app.listen(3111, function () {
  console.log('Web push app listening on port 3111!')
});
```
前端监听 push 事件并读取来自服务端的有效载荷数据。

有了有效载荷数据后就可以使用 showNotification 函数来显示通知。

pushManager.getSubscription() 函数来检查用户是否已经订阅。

subscription.unsubscribe() 函数来取消订阅。

VAPID 是一个规范，它本质上定义了应用服务器和推送服务之间的握手，并允许推送服务确认哪个站点正在发送消息



