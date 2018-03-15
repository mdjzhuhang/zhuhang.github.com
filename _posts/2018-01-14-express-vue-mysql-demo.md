---
layout: post
title:  express demo
excerpt: 使用express，vue-cli, axios, echart，mysql
category: demos
---

[项目地址](https://github.com/mdjzhuhang/front-end/tree/master/page-analisis)

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
