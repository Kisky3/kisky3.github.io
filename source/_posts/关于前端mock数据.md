---
title: Data Mocking – Ways To Fake A Backend
date: 2019-05-21 19:52:09
tags:
- Mock
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

关于mock数据 - 怎样模拟后端数据
<!--more-->

### 1. 搭建一个静态服务器
#### http-server


比如在本地写一个http-server，可以在本地将需要的数据，做成一个文件，然后将该数据返回。

也可以在github上建立一个项目，创建首页，之后再创建一个json文件，将我们需要的数据写入此文件。

例：

<img src="./1.png" style="width:500px">

<br>
<img src="./2.png" style="width:500px">

之后打开页面就可以看到我们所取得的json数据了

<img src="./3.png" style="width:500px">


### 2. 线上mock数据
在下面的网站里添加接口和数据，再将生成的数据URL写入ajax即可取得在线模拟的数据

1.使用http://easy-mock.com

2.使用http://rapapi.org/org/index.do

3.使用server-mock