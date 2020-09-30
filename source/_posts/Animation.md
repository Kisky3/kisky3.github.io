---
title: About jQuery animation
date: 2019-08-30 19:37:21
tags:
- jQuery
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
关于jQuery动画API
<!--more-->

### jQuery 主要的动画控制API

1 .show()
2 .hide()
3 .slideUp
4 .slideDown
5 .slideToggle
6 .fadeIn
7 .fadeOut
8 .animate

如上函数如何使用？演示使用方式

***

#### .hide()
.hide([duration ] [,easing ] [,complete ])
用于隐藏元素，没有参数的时候等同于直接设置display属性,当我们给hide设置事件时：hide(2000)会有一个消失的过程
示例：
```JS
<style>
  div {
    background:#ccc;
    width: 100px;
    height: 100px;
    border: 1px solid;
  }
  </style>
</head>
<body>
<div> 
</div>
</body>
<script>
$('div').hide()  // ==  $('.target').css('display', 'none')
</script>

```

***

#### .show()
.show( [duration ] [, easing ] [, complete ] )
用于显示元素，用法和hide类似给显示元素设置一个现实的时间
```JS
$('div').hide(2000)  // 隐藏时间为2s
$('div').show(2000) //显示时间为2s
```

#### .slideUp
.slideUp( [duration ] [, easing ] [, complete ] )
用滑动动画隐藏一个匹配元素，方法将给匹配元素的高度的动画，这会导致页面的下面部分滑上去，当一个隐藏动画后，高度值达到0的时候，display 样式属性被设置为none，以确保该元素不再影响页面布局。
效果：（代码基于hide）

```
$('div').slideUp()
```

***

#### .slideDown
用滑动动画显示一个匹配元素，方法将给匹配元素的高度的动画，这会导致页面的下面部分滑下去，弥补了显示的方式
效果：（代码基于slideUp）

***

#### .slideToggle
用滑动动画显示或隐藏一个匹配元素，方法将给匹配元素的高度的动画，这会导致页面中，在这个元素下面的内容往下或往上滑。display属性值保存在jQuery的数据缓存中，所以display可以方便以后可以恢复到其初始值。

如果一个元素的display属性值为inline，然后是隐藏和显示，这个元素将再次显示inline。当一个隐藏动画后，高度值达到0的时候，display 样式属性被设置为none，以确保该元素不再影响页面布局。

效果：
连续调用两次，和使用.slideUp()、.slideDown效果相同

***

#### .fadeIn
.fadeIn( [duration ] [, easing ] [, complete ] )
通过淡入的方式显示匹配元素，参数含义和上面相同
给div的css属性设置display:none

$('div').fadeIn(2000)

***

#### .fadeOut
.fadeOut( [duration ] [, easing ] [, complete ] )
通过淡出的方式隐藏匹配元素
取消div的css中的display:none

***

上面几个简单的动画不能满足需求的时候，jquery提供了自定义动画行为的方法

#### .animate
.animate( properties [, duration ] [, easing ] [, complete ] )
```JS
<div id="clickme">
  Click here
</div>
<img id="book" src="book.png" alt="" width="100" height="123"
  style="position: relative; left: 10px;">
```

```JS
$( "#clickme" ).click(function() {
  $( "#book" ).animate({
    opacity: 0.25,  // 图片透明度  渐变
    left: "+=50",   //向左移动距离 -- 原基础上+50px;
    height: "toggle"  // 
  }, 5000, function() {
    // Animation complete.
  });
});
```
height属性的目标值是'toggle'。由于之前图像是可见的，因此动画会将高度缩小为0以隐藏它。第二次点击然后反转此转换

***

### jQuery动画队列
jQuery提供了以下几种方法来操作动画队列。

- stop([clearQuery],[gotoEnd]):停止当前jQuery对象里每个DOM元素上正在执行的动画。

- queue([queueName,]callback):将callback动画数添加到当前jQuery对象里所有DOM元素的动画函数队列的尾部。

- queue([queueName,]naeQueue):用newQueue动画函数队列代替当前jQuery对象里所的DOM元素的动画函数队列。

- dequeue():执行动画函数队列头的第一个动画函数，并将该动画函数移出队列。

- clearQueue([queueName]):清空动画函数队列中的所有动画函数。可选的 callback 参数是动画完成后所执行的函数名称。

例子：
```JS
<style>  
    div {  
        width: 60px;   
        height: 60px;  
        position:absolute;  
        top:60px;   
        background: #f0f;  
        display:none;  
    }  
    </style>  
</head>  
<body>  
    <script type="text/javascript" src="../jquery-1.8.0.js">  
    </script>  
    <p>动画队列的长度是：<span></span></p>  
    <div></div>  
    <script type="text/javascript">  
    var div = $("div");  
    function runIt()  
    {  
        // 第1个动画：显示出来  
        div.show("slow");  
        // 第2个动画：自动动画，水平左移300px  
        div.animate({left:'+=300'},2000);  
        // 第3个动画：卷起来  
        div.slideToggle(1000);  
        // 第4个动画：放下来  
        div.slideToggle("fast");  
        // 第5个动画：自动动画，水平右移300px  
        div.animate({left:'-=300'},1500);  
        // 第6个动画：隐藏出来  
        div.hide("slow");  
        // 第7个动画：显示出来  
        div.show(1200);  
        // 第8个动画：卷起来，动画完成后回调runIt  
        div.slideUp("normal", runIt);  
    }  
    // 控制每0.1秒调用一次该方法，该方法用于显示动画队列的长度  
    function showIt()  
    {  
        var n = div.queue();  
        $("span").text(n.length);  
        setTimeout(showIt, 100);  
    }  
    runIt();  
    showIt();  
    </script>  
```
