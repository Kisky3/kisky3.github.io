---
title: jQuery event handlers
date: 2019-08-27 22:39:01
tags:
- jQuery
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
关于jQuery事件绑定
<!--more-->

***

### 事件处理

事件处理中最头疼的就是浏览器兼容问题，jQuery封装了很好的API，可以方便的进行事件处理

在1.7之前的版本中jQuery处理事件有多个方法，后来统一的使用on/off方法.

***

#### on( events [,selector ] [,data ], handler(eventObject) )
1. events：一个或多个空格分隔的事件类型和可选的命名空间，或仅仅是命名空间，比如"click", "keydown.myPlugin", 或者 ".myPlugin"

2. selector：一个选择器字符串，用于过滤出被选中的元素中能触发事件的后代元素。如果选择器是 null 或者忽略了该选择器，那么被选中的元素总是能触发事件

3. data：当一个事件被触发时，要传递给事件处理函数的event.data

4. handler(eventObject)：事件被触发时，执行的函数。若该函数只是要执行return false的话，那么该参数位置可以直接简写成 false

例子：
```JS
<div class="box">
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
  </ul>
</div>
<input id="ipt" type="text"> <button id="btn">添加</button>

<div id="wrap"></div>

<script>
$('.box li').on('click', function(){
    console.log(1)
  var str = $(this).text()
  $('#wrap').text(str)
})

/* 等同于 */
$('.box>ul>li').click(function(){
    console.log(2)
  var str = $(this).text()
  $('#wrap').text(str)
})

/* 也可以这样写 */
$('.box li').on('click.hello', function(){
    console.log(3)
  var str = $(this).text()
  $('#wrap').text(str)
})

/* 命名空间没什么特别的作用，只不过在解绑事件时便于区分绑定的事件 */
$('.box li').off('click.hello')

/* 可是用如下方法新增的元素是没绑定事件的 */
$('#btn').on('click', function(){
  var value = $('#ipt').val()
  $('.box>ul').append('<li>'+value+'</li>')
})

/* 我们可以用事件代理 */
$('.box ul').on('click', 'li', function(){
  var str = $(this).text()
  $('#wrap').text(str)
})

/* 上面代码相当于原生 js 的 */
document.querySelector('.box ul').addEventListener('click', function(e){
    if(e.target.tagName.toLowerCase() === 'li'){
        //do something
    }
})

/* 绑定事件的时候我们也可以给事件附带些数据，只不过这种用法很少见 */
$('.box').on('click', {name: 'hunger'}, function(e){
    console.log(e.data)
})

```

***

#### .one( events [, selector ] [, data ], handler(eventObject) )
同 on，绑定事件，但只执行一次.

***

#### .off( events [, selector ] [, handler ] )
移除一个事件处理函数

```JS
$('.box li').off('click')
```

***

#### .trigger( eventType [, extraParameters ] )
根据绑定到匹配元素的给定的事件类型执行所有的处理程序和行为.

```JS
$('#foo').on('click', function() {
  console.log($(this).text())
});
$('#foo').trigger('click')
```