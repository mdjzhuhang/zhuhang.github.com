---
layout: post
title:  express demo
excerpt: Use express, vue-cli, axios, echart, mysql, convert csv file to echart.
category: demos
---

[To github: ](https://github.com/mdjzhuhang/front-end/tree/master/page-analisis)

#### front-end：

- 使用 vuecli 创建项目

- 用 webpack-dev-serve 的 proxy 代理

  ```
    proxyTable: {
      '/api': {
        target: 'http://localhost:3000',
        changeOrigin: true,
      }
    }
  ```

- 注册 axios 和 echart 插件

  ```
    import echarts from 'echarts'
    import axios from 'axios'

    Vue.prototype.$echarts = echarts
    Vue.prototype.$http = axios
  ```

#### back-end
使用 express，链接 mysql 数据库


#### axios

axios 是通过 promise 实现对 ajax 技术的一种封装，就像 jQuery 实现ajax封装一样。

ajax 技术实现了网页的局部数据刷新，axios 实现了对ajax的封装。axios是ajax，ajax不只是axios。
