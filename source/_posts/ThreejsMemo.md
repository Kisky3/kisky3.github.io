---
title: Three.js Memo
date: 2021-03-10 15:22:42
tags:
 - three.js
 - WebGL
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
Three.js学习笔记
<!--more-->
### 1. 什么是WebGL
你可以把WebGL简单地认为是一种网络标准，定义了一些较底层的图形接口。在这些标准被定义之后，Chrome、Firefox之类的浏览器实现了这些标准。然后，程序员就能通过JavaScript代码，在网页上实现三维图形的渲染了。

### 2. 什么是Three.js
>Three.js是一个3D JavaScript库。

### 3. Three.js能用来干什么
Three.js封装了底层的图形接口，使得程序员能够在无需掌握繁冗的图形学知识的情况下，也能用简单的代码实现三维场景的渲染。我们都知道，更高的封装程度往往意味着灵活性的牺牲，但是Three.js在这方面做得很好。几乎不会有WebGL支持而Three.js实现不了的情况，而且就算真的遇到这种情况，你还是能同时使用WebGL去实现，而不会有冲突。

### 4. WebGL vs. Three.js
对于一个简单的功能:渲染黑色背景下的白色正方形和三角形,使用原生WebGL接口实现同样功能需要5倍多的代码量，而且很多代码对于没有图形学基础的程序员是很难看懂的。由这个例子我们可以看出，使用Three.js开发要比WebGL更快更高效。尤其对图形学知识不熟悉的程序员而言，使用Three.js能够降低学习成本，提高三维图形程序开发的效率。

### 5. 开发环境
Three.js是一个JavaScript库，所以，你可以使用平时开发JavaScript应用的环境开发Three.js应用。
推荐：Chrome
repo：https://github.com/mrdoob/three.js/tree/master/build

引用：
```
<script type="text/javascript" src="three.js"></script>
```
然后就能通过全局变量THREE访问到所有属性和方法了。

### 6. 练习1: Hello, world!
详细coding请参照我的GitHub repo.

首先，在HTML的<head>部分，需要声明外部文件three.js。
```html
<head>
    <script type="text/javascript" src="js/three.js"></script>
</head>
```
WebGL的渲染是需要HTML5 Canvas元素的，你可以手动在HTML的<body>部分中定义Canvas元素，或者让Three.js帮你生成。这两种选择一般没有多大差别，我们在此手动在HTML中定义：
```html
<body onload="init()">
    <canvas id="mainCanvas" width="400px" height="300px" ></canvas>
</body>
```
在JavaScript代码中定义一个init函数，在HTML加载完后执行：
```html
function init() {
    // ...
}
```
一个典型的Three.js程序至少要包括渲染器（Renderer）、场景（Scene）、照相机（Camera），以及你在场景中创建的物体。
