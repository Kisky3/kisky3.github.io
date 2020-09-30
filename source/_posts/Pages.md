---
title: Deploying React Applications to Github Pages
date: 2019-09-30 01:46:01
tags:
- react
- github pages
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

把React项目部署到Github Page线上环境
<!--more-->

#### 1. 在github上新建仓库
貌似必须要新建仓库，如果在已有仓库的分支下想预览不成功（因为并不是master，所以也会对别的分支有影响.

***

#### 2. 将本地代码同步
参照新建仓库里的说明初始化并push就好.

***

#### 3. 修改本地React项目的 package.json文件

##### 配置homepage

这里需要把你的github仓库地址稍微修改一下，例如我的"homepage": "https://Kisky3.github.io/react-todolist".

##### 配置发布选项

在scripts里添加以下两行

predeploy:是将你的项目预编译成静态文件放在build文件夹
deploy:是使用gh-pages 部署你的build文件夹下的内容.

```JS
  "scripts": {
    ...
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build"
  },
```

修改后的package.json
```JS
{
  "name": "todolist",
  "version": "0.1.0",
  "homepage": "https://Kisky3.github.io/react-todolist",
  "private": true,
  "dependencies": {
    "react": "^16.4.1",
    "react-dom": "^16.4.1",
    "react-scripts": "1.1.4"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject",
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build"
  },
  "devDependencies": {
    "gh-pages": "^2.0.1"
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
```

##### 安装 gh-pages

```
npm install gh-pages --save-dev
```

##### 部署项目到github page上
```
npm run deploy
```

***

#### 4. GIthub上的分支切换

配置完之后，打开github上的仓库，你会发现原先的项目多了一个gh-pages分支，里面存放的是我们打包编译完成之后的静态文件。
一定要手动切换到gh-pages分支 而不是master！

<img src="./1.png" style="width:500px;margin:40px 0">

再切换到setting下，我们可以看到现在项目已经被成功部署到 https://Kisky3.github.io/react-todolist 上了

<img src="./2.png" style="width:500px;margin:40px 0">

打开 https://Kisky3.github.io/react-todolist 检验是否能预览

<img src="./3.png" style="width:500px;margin:40px 0">
