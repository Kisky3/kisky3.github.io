---
title: About CSS Float
date: 2018-12-30 23:00:12
tags:
- CSS
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

关于CSS浮动
<!--more-->

### 浮动元素的特征
一个浮动元素会向左或向右移动，直到其外边（outer edge）挨到包含块边沿或者另
一个浮动盒的外边。
如果存在行盒，浮动盒的外top会与当前行盒的top对齐。
如果没有足够的水平空间来浮动，它会向下移动，直到空间合适或者不会再出现其他浮动了。
块级元素设置浮动后会呈现出inline-block的感觉，宽度会收缩。
行内元素设置浮动后会呈现块级的特性。比如可以设置宽高margin等。

***

### 浮动元素的效果及对其他元素的影响
下面为浮动最简单的一个左浮动效果。
有3个box，分别向左浮动。
可以看到box原来为块级元素，本应该单独占据一行。但是设置左浮动之后，脱离文档流，依次向左移动。由于没有足够空间来让box3移动，便到了下一行。
<img src="./1.png" style="width:500px">

<br>
如果改为右浮动的话，效果将变成box2，box1，box3。
<img src="./2.png" style="width:500px">

这个效果是因为浏览器渲染时，从上到下渲染代码。第一个是box1，便向右浮动，直至碰到外边缘便停止下来。第二个是box2接着向右浮动，直至碰到了box1的外边缘便停止浮动。最后是box3，向右浮动时没有足够空间便移到了下一行。

有时浮动还会出现如下图卡住的现象。
这是因为box3在向左浮动时，第一个碰到的是box1的外边缘，便停下来卡住了。

<img src="./3.png" style="width:500px">

当浮动元素与普通元素和文本有交集的时候会是什么情况呢。
我们在box1下面加一个文本p作为文字，p背景色设置为黄色作为普通元素。其余box元素不变向左浮动。最终呈现效果如下图:

<img src="./4.png" style="width:500px">
<br>
可以得出结论，box1把普通元素p遮挡住了，普通元素看不见浮动元素。但是普通元素内的文字是可以看到浮动元素的，所以就会围绕浮动元素显示。

***

### 关于如何清除浮动

##### clear: left/clear right定义：
要求该盒的top border边位于源文档中在此之前的元素形成的所有左/右浮动盒的bottom外边下方。

也就是说当我们给一个元素设置了clear:left之后，它的文档流上方如果有左浮动元素，它就要位于该左浮动元素的下方。如果前面没有左浮动元素则不起效果。
clear:right也同理。
还有clear：both 就是该元素之前有左浮动元素或者右浮动元素都生效。

##### 方法一

在浮动元素的最后加上一个普通元素，设置clear:left,达到撑开容器的效果。

<img src="./5.png" style="width:500px">

##### 方法二

给包含浮动元素的容器设置一个class伪元素来清除浮动。相当于在最下方添加了一个内容为空的块级元素。

```CSS
.clearfix::after {
 content:'';
display: block;
clear: both;
}
```
