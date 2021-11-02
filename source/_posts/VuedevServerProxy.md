---
title: Using Vue Proxy To Solve Axios CORS Problem
date: 2021-10-14 21:49:46
tags:
  - Vue
  - Proxy
  - CORS
clearReading: true
coverCaption: 'Hello World, Hello Programming'
coverSize: partial
comments: false
categories: Front-end Knowledge
---

利用 Vue 的反向代理来解决 Axios 的 CORS 问题

<!--more-->

## 问题

当在开发环境开发的时候,使用 axios 来向后端的 API 发送求情,
但是会被 Policy 绊住, 就是常见的 CORS 问题.

```
Access to XMLHttpRequest at 'http://localhost:3000/api/test' from origin 'http://localhost:8080' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource
```

因为 Origin 不同所以浏览器会将请求拒绝.

```
(example)Origin不同
Vue.js:http://localhost:8080
Golang:http://localhost:3000
```

## 解决方法

1.叫服务器端设定 CORS

2.前端的 Vue 使用 proxy 来回避此问题

1. 在 Vue 的根目录下生成 vue.config.js 文件

vue.config.js

```js
module.exports = {
  devServer: {
    proxy: {
      '/api/': {
        target: 'http://localhost:3000',
      },
    },
  },
}
```

像下面这样写的话

```js
axios.get('/api/test').then((res) => {
  console.log(res.data)
})
```

就可以在请求 Golang 的 API`/api/**`的时候, 转到`http://localhost:3000`,这样就可以回避同源策略了。

当然我们还可以利用环境参数来根据不同的开发环境设置不同的 proxy.

```js
module.exports = {
  devServer: {
    proxy: {
      '/api/': {
        target: process.env.USE_LOCAL_SERVER
          ? 'http://localhost:3000'
          : 'http://aaa.com',
      },
    },
  },
}
```

如果你想要看 log 的话可以加上

```js
module.exports = {
  devServer: {
    proxy: {
      '/api/': {
        target: process.env.USE_LOCAL_SERVER
          ? 'http://localhost:3000'
          : 'http://aaa.com',
        logLevel: 'debug', // 追加log
      },
    },
  },
}
```
