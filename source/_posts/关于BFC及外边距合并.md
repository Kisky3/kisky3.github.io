---
title: About Block Formatting Context
date: 2019-01-16 23:40:57
tags:
- CSS
- BFC
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

关于BFC及外边距合并
<!--more-->

### BFC 是什么
BFC全称Block Formatting Context。中文为”块级格式化上下文”。

每个渲染区域用formatting context表示，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。在正常流中的盒子要么属于块级格式化上下文，要么属于内联格式化上下文。
也就是页面渲染时候所遵循的一种规则。

***
### BFC的特性有
- 内部的Box会在垂直方向按顺序放置
- Box垂直方向的距离由margin决定，属于同一个BFC的两个相邻Box的margin会发生重叠 ，每个元素的margin box的左边与包含块border box的左边相接触。
- BFC 区域不会与float box重叠
- BFC就是页面上的一个隔离的独立容器，容器里的子元素不会影响到外面的元素。
- BFC的高度也包括浮动元素的高度。

***
### BFC的产生

1. 根元素
- 撑满父容器，父容器能被子元素撑开
- 可能会产生外边局合并的情况
- 页面渲染时位置从上到下

2.float属性不为none
拥有float属性的元素，相当于脱离文档流，拥有自己专属空间，对外界元素没有影响的元素。

3.position为absolute或fixed
position属性值为absolute或fixed的，脱离了文档流的元素。

4.display为inline-block，flex，或者inline-flex

5.overflow不为visible

***
### BFC 有什么作用？举例说明。
BFC的作用有：
1. 自适应两栏布局
```CSS
<style>
    body {
        position: relative;
    }

    .aside {
        width: 90px;
        height: 150px;
        float: left;
        background: #f66;
    }

    .main {
        height: 200px;
        background: #fcc;
      
    }
</style>
<body>
    <div class="aside"></div>
    <div class="main"></div>
</body>
```

页面效果：

<img src="./1.png" style="width:500px">

根据BFC布局规则第三条：

每个元素的margin box的左边，与包含border box的左边相接触（对于从左往右的格式化，否则相反）。即使存在浮动也是如此。

因此，虽然存在浮动元素aslide，但main的左边依然会与包含块的左边接触。
根据BFC布局规则第四条：

BFC的区域不会与float box重叠。
我们可以通过除法main生成BFC，来实现自适应两栏布局。

```CSS
.main {
    overflow: hidden;
}
```
当除法main生成BFC后，这个新的BFC不会与浮动的aside重叠。因此会根据包含块的宽度和aside的宽度自动变窄。

页面效果：

<img src="./2.png" style="width:500px">

2.清除内部浮动

```CSS
<style>
    <style>
    .parent {
        border: 5px solid #fcc;
        width: 300px;
    }

    .child {
        border: 5px solid #f66;
        width:100px;
        height: 100px;
        float: left;
    }
</style>
<body>
    <div class="parent">
        <div class="child"></div>
        <div class="child"></div>
    </div>
</body>
```
页面效果：

<img src="./3.png" style="width:500px">

根据BFC布局规则：

计算BFC的高度时，浮动元素也参与计算
为达到清除内部浮动，我们可以除法parent生成BFC，那么parent在计算高度时，parent内的浮动元素child也会参与计算。

```CSS
.parent {
    overflow: hidden;
}
```

3.防止垂直margin重叠

```CSS
<style>
    p {
        color: #f55;
        background: #fcc;
        width: 200px;
        line-height: 100px;
        text-align:center;
        margin: 100px;
    }
</style>
<body>
    <p>Haha</p>
    <p>Hehe</p>
</body>
```

页面效果：

<img src="./4.png" style="width:500px">

两个p之间的距离为100px，发送了margin重叠。 根据BFC布局规则第二条：

Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠

我们可以在p外面包裹一层容器，并触发该容器生成一个BFC。那么两个P便不属于同一个BFC，就不会发生margin重叠了。

```CSS
<style>
    .wrap {
        overflow: hidden;
    }
    p {
        color: #f55;
        background: #fcc;
        width: 200px;
        line-height: 100px;
        text-align:center;
        margin: 100px;
    }
</style>
<body>
    <p>Haha</p>
    <div class="wrap">
        <p>Hehe</p>
    </div>
</body>
```

页面效果：
<img src="./5.png" style="width:500px">

***

### 总结
BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

因为BFC内部的元素和外部的元素绝对不会互相影响，因此， 当BFC外部存在浮动时，它不应该影响BFC内部Box的布局，BFC会通过变窄，而不与浮动有重叠。同样的，当BFC内部有浮动时，为了不影响外部元素的布局，BFC计算高度时会包括浮动的高度。避免margin重叠也是这样的一个道理。

### 外边距合并
块的顶部外边距和底部外边距有时候会被折叠为单个外边距，其大小为两值中的最大值，这种行为就被称为外边距合并。
一般发生外边距合并主要有以下三种情况：

- 相邻兄弟姐妹元素
- 父元素和子元素
- 空元素

#### 相邻兄弟姐妹元素
两个兄弟元素之间的外边距，会取两个元素外边距的最大值。

```CSS
<p style="margin-bottom: 30px;">这个段落的下外边距被合并...</p>
<p style="margin-top: 20px;">...这个段落的上外边距被合并。</p>
```

按照上面的例子可以得出，两个p元素之间距离为30px。


#### 父元素和子元素
这种情况又可以分为以下两种：

- 父元素的上外边距和第一个子元素的上外边距
- 父元素的下外边距和最后一个子元素的下外边距
这两种情况，最终显示出来的结果都是取二者中的最大值。

#### 空元素
自己的上外边距会和自己的下外边距合并
```CSS
<p style="margin-bottom: 0px;">这个段落的和下面段落的距离将为20px</p>

<div style="margin-top: 20px; margin-bottom: 20px;"></div>

<p style="margin-top: 0px;">这个段落的和上面段落的距离将为20px</p>
```
这样第一个p元素和第三个p元素之间距离为20px

#### 阻止合并方法

##### 通用方法
1. 处于静态流元素会发生合并，所以float和position:absolute都不会发生合并
2. 设置为inline-block ，也不会发生合并

##### 针对于父元素和子元素情况不合并方法
以下都不会发生合并

1. 设置了清除浮动属性
2. 因为margin需要直接接触才能合并，所以父元素或子元素中有border或padding，或者二者之间有元素

##### 注意
- 如果两个外边距值中有一个为0，也会发生合并。
- 如果有负外边距，合并后外边距为最大正边距加上最小负边距（绝对值最大的一个），如上面元素下边距为20px，下面元素上边距为-20px，则最后为0px

***
### 参考：

[外边距合并MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing)
[「CSS」Margin Collapsing - 外边距合并](https://segmentfault.com/a/1190000003712262)

