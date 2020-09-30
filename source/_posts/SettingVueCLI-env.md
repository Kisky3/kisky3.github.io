---
layout: post
title: Setting Vue CLI Modes and Environment Variables
date: 2020-03-03 19:15:13
tags:
- setting
- Vue CLI
- Environment Variables
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: System Setting
---
设置Vue CLI的模式和环境变量
<!--more-->
基本上参照官方文档设定就好 [Vue 官方文档](https://cli.vuejs.org/guide/mode-and-env.html#modes)

***
### 环境
- Vue CLI: 3.6.3
***
### 前提条件
本回创建本地local环境，开发环境，和生产环境

***
### .env文件的生成
- .env.local
- .env.development
- .env.prod
在创建好的每个文件下写入相应的API Base URL.
本地环境的情况下 `.local`最好在.gitignore里添加，不要push到生产环境里。
`VUE_APP_`这个接头词是必要的，漏掉的话build不成功的。。(之前卡在这了)

例：
.env.local
```
NODE_ENV='local'
VUE_APP_API_BASE_URL='https://localhost:8080/api/v1/xxx'
```

.env.development
```
NODE_ENV='development'
VUE_APP_API_BASE_URL='https://development/api/v1/xxx'
```

.env.prod
```
NODE_ENV='production'
VUE_APP_API_BASE_URL='https://production/api/v1/xxx'
```

***

### package.json的设定
在package.json里的scripts里设定你的模式

例：
package.json
```
"scripts": {
    "local-serve": "vue-cli-service serve --mode local",
    "dev-serve": "vue-cli-service serve --mode development",
    "prod-serve": "vue-cli-service serve --mode production",
    "dev-build": "vue-cli-service build --mode development",
    "prod-build": "vue-cli-service build --mode production",
  }
```
***
### 使用方法
根据你设定的package.json来运行
比如 yarn local-serve的时候，就会相应的获取.env.local里的环境变量。

这样你就能够根据不同的开发环境进行build,deploy了。

