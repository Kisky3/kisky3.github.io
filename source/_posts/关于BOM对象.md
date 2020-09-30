---
title: About Brower Object Model
date: 2019-04-25 21:22:10
tags:
- BOM
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

关于BOM对象 
<!--more-->
### BOM
BOM(Brower Object Model)是指浏览器对象模型，是用于描述这种对象与对象之间层次关系的模型,

浏览器对象模型提供了独立于内容的，可以与浏览器窗口进行互动的对象结构。BOM由多个对象组成，
其中代表浏览器窗口的window对象是BOM的顶层对象，其他对象都是该对象的子对象。   

***

### window
BOM的核心是window对象，它表示浏览器的一个实例。在浏览器中，即是javascript访问浏览器窗口的一个接口，
又是ECMAscript规定的Global对象，这就意味着在网页中定义的任意变量，函数，对象都是以window作为Global对象。

所有在全局作用域中声明的变量，函数，对象都会作为window的属性和方法。

```JS
var age = 24;

function printNamr() {
    console.log(age);
}

console.log(window.age);
window.printName();
```

***

### window对象属性

#### window.innerHeight属性，window.innerWidth属性

这两个属性返回网页的CSS布局占据的浏览器窗口的高度和宽度，单位为像素。
很显然，当用户放大网页的时候（比如将网页从100%的大小放大为200%），这两个属性会变小。

注意，这两个属性值包括滚动条的高度和宽度。

***

#### scrollX，scrollY

1. scrollX: 滚动条横向偏移
2. scrollY: 滚动条纵向偏移

***

#### scrollTo，scrollBy,scroll

我们也可以通过方法scrollTo或者scroll方法改变滚动条位置到指定坐标

```JS
window.scrollTo(0,300);// 滚动条移动到300px处
```

两个参数分别是水平，垂直方向偏移

scrollBy可以相对当前位置移动滚动条，而不是移动到绝对位置

```JS
scrollBy(0,100); //滚动条下移100px
```

***

#### navigator

Window对象的navigator属性，指向一个包含浏览器相关信息的对象。
navigator.userAgent属性返回浏览器的User-Agent字符串，用来标示浏览器的种类。
下面是Chrome浏览器的User-Agent。

```JS
navigator.userAgent
```

通过userAgent属性识别浏览器，不是一个好办法。因为必须考虑所有的情况（不同的浏览器不同的版本）
非常麻烦，而且步伐保证未来的适用性，更何况各种上网设备层出不穷。
所以，现在一般不再识别浏览器了，而是使用“功能识别”方法，即逐一测试当前浏览器是否支持要用到的JavaScript功能。

***

#### screen

screen对象包含了显示设备的信息。

```JS
// 显示设备的高度，单位为像素
screen.height
// 1920

// 显示设备的宽度，单位为像素
screen.width
// 1080
```

***

#### URL的编码/解码方法

Javascript提供四个URL的编码/解码方法

1. decodeURI（）
2. decodeURIComponent（）
3. encodeURI（）
4. encodeURIComponent（）

区别

encodeURI方法不会对下列字符编码

```
1. ASCII字母
2. 数字
3. 字符号
```

encodeURIComponent方法不会对下列字符编码

```
1. ASCII字母
2. 数字
3. ~!*()'
```

encodeURIComponent会把所以http://编码成http:%3A%2F%2F
而encodeURI不会。
encodeURIComponent比encodeURI编码的范围更大。