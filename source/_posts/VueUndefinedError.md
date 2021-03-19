---
layout: post
title: Vue TypeError:「Cannot read property ..... of undefined"」
date: 2020-09-12 23:36:05
tags:
- Vue
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
Vue类型错误：无法阅读未定义属性
<!--more-->

当我想取得*data[index]["hoge"]*这样当数据的时候，vue出现了一下当报错。
(但是在console里打出来看看又发现是可以取到数据的)

```
 Cannot read property 'hoge' of undefined
```

理由是因为data[index]为undefined的时候无法取得下一级的元素，
在vue上加上一个v-if条件就可以了。

例子：
```html
<span v-if="data[index]">{{data[index]["hoge"]}}</span>
```
