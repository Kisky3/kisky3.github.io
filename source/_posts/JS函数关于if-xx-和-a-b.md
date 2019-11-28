---
title: JS About if-xx- And -a-b
date: 2019-02-26 18:35:02
tags:
- JS
clearReading: true
thumbnailImage: 20190226.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

JS函数关于if-xx-和-a-b
<!--more-->

### JS函数:f(xx)
JS对于括号里的表达式，会被强制转换成布尔类型

基本原理如下表：

| type |  result  |
| ----| ---- |
|  Undefined | false  |
|  Null  |  false  |
|  Boolean  | 直接判断  |
|  Number |  +0，-0，或者NaN为false，其他为true  |
|  String |  空字符串为false，其他都为true  |
|  Object |  true  |

***

### JS函数：a==b

JS对于a==b类型会进行变形比较，原理如下表：
基本上遇到数字以外的类型可以先考虑往数字上转型

| x |  y  |  结果  |
| ----| ---- | ---- |
|  null | undefined  | true  |
|  Number  |  String  | x == toNumber(y)  |
|  Boolean  | (any)  | toNumber(x) == y |
|  Object |  String or Number  | toPrimitive(x) == y |
|  otherwise |  otherwise  | false |

***

### toNumber

其他类型转换成数字用toNumber：

| type |  result  |
| ----| ---- |
|  Undefined | NaN |
|  Null  |  0 |
|  Boolean  | true -> 1, false -> 0  |
|  String |  “abc” -> NaN, “123” -> 123  |

***

### toPrimitive

对于Object类型，先尝试调用.v3alueOf方法获取结果。如果没定义，再尝试调用.toString方法获取结果。
