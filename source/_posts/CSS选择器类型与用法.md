---
title: About CSS Selectors
date: 2018-11-15 17:23:25
tags:
- CSS
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

CSS选择器类型与用法
<!--more-->
### 选择器类型
选择器分为基础选择器，组合选择器，属性选择器，伪类选择器，伪元素选择器

***
#### 基础选择器

| 选择器 |  含义  |
| ----| ---- |
|  *  |  通用元素选择器，匹配页面任何元素（这也就决定了我们很少使用）|
|  #id  |  id选择器，匹配特定id元素  |
|  .class  |  类选择器，匹配class包含（不是等于）特定类的元素  |
|  element |  标签选择器  |

例子：
```CSS
* {
 margin: 0;
padding:0;
}

#id-selector {
 color:#333;
}

.class-selector {
 background: #ccc;
}

p {
font-size: 20px;
}
```
***
### 组合选择器

| 选择器 |  含义  |
| ----| ---- |
|  E,F  |  多元素选择器，用逗号分隔，同时匹配元素E或元素F|
|  E F  |  后代选择器，用空格分隔，匹配E元素所有的后代（不只是子元素向下递归）元素F  |
|  E>F  |  子元素选择器，用>分隔，匹配E元素的所有直接子元素  |
|  E+F  |  直接相邻选择器，匹配E元素之后的相邻的同级元素F  |
|  E~F  |  普通相邻选择器(弟弟选择器)，匹配E元素之后的同级元素F(无论直接相邻与否)  |
|  .class1.class2  | id和class选择器和选择器连写的时候中间没有分隔符， 。和#本身充当分隔符的元素  |
|  element#id  |  id和class 选择器和选择器连写的时候中间没有分隔符，。和#本身充当分隔符的元素  |

例子：
```CSS
.p1,.p2 {
 color: red;
}

#ct .p2 {
 color:blue;
}

#ct >.p2 {
 color:blue;
}

.p1+p {
 color:red;
}

.p1~p {
 color:red;
}

.p2.active {
 color:red;
}

div#ct {
 background: red;
}
```
### 属性选择器

| 选择器 |  含义  |
| ----| ---- |
| E[attr]| 匹配所有具有属性attr的元素，div[id]就能取到所有有id属性的div |
| E[attr=value]| 匹配属性attr 值为value的元素，div[id=test]，匹配id=test的div |
| E[attr~=value]| 匹配所有属性attr具有多个空格分隔，其中一个值等于value的元素 |
| E[attr^=value]| 匹配属性attr的值以value开头的元素 |
| E[attr$=value]| 匹配属性attr的值以value结尾的元素 |
| E[attr*=value]| 匹配属性attr的值包含value的元素 |

***
### 伪类选择器
代表元素的一种状态

| 选择器 |  含义  |
| ----| ---- |
| E:first-child| 匹配其E的父元素的第一个子元素 |
| E:nth-child(n)| 匹配其E的父元素的第n个子元素(2n+1,2n) |
| E:first-of-type| E的同种类型下的第一个元素 |
| E:nth-of-type(n)| E的同种类型下的第n个元素 |
| E:link| 匹配所有未被点击的链接 |
| E:visited| 匹配所有已经被点击的链接 |
| E:active| 匹配鼠标已经其上按下还没有释放的E元素 |
| E:hover| 匹配鼠标悬停其上的E元素 |
| E:focus|  匹配获得当前焦点的E元素|
| E:enabled| 匹配表单中可用的元素 |
| E:disabled| 匹配表单中禁用的元素 |
| E:checked|  匹配表单中被选中的radio或checkoutbox元素|
| E:selection| 匹配用户当前选中的元素 |

a链接伪类选择器时要注意伪类的顺序

***

### 伪元素选择器

| 选择器 |  含义  |
| ----| ---- |
| E::first-line| 匹配E元素内容的第一行 |
| E::first-letter| 匹配E元素内容的第一个字母 |
| E::before| 在E元素之前插入生成的内容 |
| E::after| 在E元素之后插入生成的内容 |

***
### 选择器的优先级
<img src="./1.png" style="width:500px">

注意：

在一些复杂场景下，可以进行一些标记再进行比较
假如选择器有两次则下面的样式会覆盖上面的样式
<img src="./2.png" style="width:500px">

