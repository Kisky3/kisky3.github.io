---
layout: post
title: About Nuxt Router and Params
date: 2019-12-08 20:33:18
tags:
- Nuxt.js
- Router
clearReading: true
thumbnailImage: 20191209.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

关于Nuxt路由设置及传参
<!--more-->
首先在pages路径下建立about.vue，news.vue，和index.vue文件，并在index.vue里引用三个link
<br>
<img src="./1.png" style="width: 500px">

运行打开页面可以发现路由生效，并可以通过a标签进行跳转。

下面我们利用nuxt的写法，将a标签写成下面的形式：
```
<nuxt-link :to = "{name: 'index'}">HOME</nuxt-link>
```
<img src="./2.png" style="width: 500px">

打开画面验证，效果是一样的。

下面进行画面间的传参。在给news的link上传一个参数params的对象，newsId = 3306
```
<nuxt-link :to = "{name: 'news', params:{newsId:3306}}">HOME</nuxt-link>
```
<img src="./3.png" style="width: 500px">
然后在news.vue里接受参数，并显示。在这里是使用双花括号进行显示，也可以通过js里进行设置之后再显示。
```
{{$router.params.newsId}}
```

<img src="./4.png" style="width: 500px">

运行，打开页面，发现传递的参数newsId已经被news页面接受到并成功显示了。
<br>
<img src="./5.png" style="width: 500px">

以上!！
