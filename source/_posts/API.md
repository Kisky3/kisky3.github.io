---
title: jQuery Common API
date: 2019-07-16 18:57:55
tags:
- jQuery
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---


jQuery常用API
<!--more-->

## jQuery常用DOM操作

### .append()
添加元素，通过$符号，生成一个dom元素并将它添加到页面，也可以添加jQuery对象，字符串等
```HTML
<body>
<p>你好！</p>
 
<script>
    $('p').append('小明和小红。');
</script>
</body>

/*执行结果*/
/*<p>你好！小明和小红。</p>*/
```
***
### .prepend()
在元素的前头添加字符串或元素。
```HTML
<ul>
    <li>太郎</li>
    <li>花子</li>
    <li>三郎</li>
</ul>

$('li').prepend('<strong>ユーザー名：</strong>');
/* 执行结果*/
/* <ul>
    <li><strong>ユーザー名：</strong>太郎</li>
    <li><strong>ユーザー名：</strong>花子</li>
    <li><strong>ユーザー名：</strong>三郎</li>
</ul>*/
```
***
### .before()
在对象前面（不是内部而是外面，和对象同级）插入内容，参数和append相似。
```HTML
<p> is what I said...</p>
<p> is what you said too...</p>

$("p").before("<b>Hello</b>");
/* 执行结果 */
/*
<b>Hello</b>
<p> is what I said...</p>
<b>Hello</b>
<p> is what you said too...</p>
*/
```
***
### .after()
在对象后面（不是内部而是外面，和对象同级）插入内容，参数和append相似。
```HTML
<p> is what I said...</p>
<p> is what you said too...</p>

$("p").before("<b>Hello</b>");
/* 执行结果 */
/*
<p> is what I said...</p>
<b>Hello</b>
<p> is what you said too...</p>
<b>Hello</b>
*/
```
***
### .remove()
删除所选对象的元素或者子元素。
```HTML
<body>
<div>
    <p>sample1</p>
    <p class="two">sample2</p>
</div>
 
<script>
    $('.two').remove();
</script>
</body>

/* 执行结果*/
/*
<div>
    <p>sample1</p>
</div>
*/
```
***
### .empty()
删除指定元素内的所有子元素。该元素保留
```HTML
<div id="parent">
  <p class="child">子元素</p>
</div>
<button id="button">删除子元素</button>

$("#button").on("click", function(){
  $("#parent").empty();
});

/* 执行结果*/
/*
<div id="parent">
</div>
<button id="button">删除子元素</button>
*/
```
***
### .html()
实用html()能够任意的获取HTML元素，并进行添加或替换处理。
获取：
```HTML
<body>
<p>你好</p>
<a href="#">sample</a>
 
<script>
    const result1 = $('p').html();
    const result2 = $('a').html();
 
    console.log( result1 );
    console.log( result2 );
</script>
</body>
/* 执行结果*/
/*
你好
sample
*/
```
替换：
```HTML
<body>
<div>
    <h1>title</h1>
    <p>sample text</p>
    <a href="#">link</a>
</div>
 
<script>
    $('div').html('<p>Hello</p>');
</script>
</body>

/* 执行结果*/
/*
<div>
    <p>こんにちは</p>
</div>
*/
```
添加：
```HTML
<body>
<p>here is the link</p>
 
<script>
    $('p').html('<p>link is<a href="#">this one</a>!!</p>');
</script>
</body>
/* 执行结果*/
/*
<p>link is<a href="#">this one</a>!!</p>
*/
```
***
### .text() 
text()和html()十分相似，$node.text()和$node.html()的区别是text取得所有符合条件的元素进行处理，添加时只能添加字符串。
text()和html()的获取元素对比：
```HTML
<body>
<p>Good Morning</p>
<p>Hello</p>
 
<script>
    const result1 = $('p').html();
    const result2 = $('p').text();
 
    console.log( result1 );
    console.log( result2 );
</script>
</body>

/* 执行结果*/
/*
Good Morning
Good MorningHello
*/
```

text()插入字符串
```HTML
<body>
<div>
    <p>Good Morning</p>
</div>
 
<script>
    $('div').text('<h1>Title</h1>');
</script>
</body>

/* 执行结果*/
/* 「<h1>Title</h1>」被当作字符串插入*/
```
***
## jQuery属性&CSS操作
### 属性相关
### .val()
val()用于取得HTML元素的value，并可以对其进行修改和设定
```HTML
<button id="btn-a" value="a">Button A</button>

	
$('#btn-a').val('value-a');


/* 执行结果*/
/*
<button id="btn-a" value="value-a">ボタンA</button>
*/
```

### .attr()
attr()用于获取HTML元素的属性，并对其进行修改和设定
获取元素的属性并修改：
```HTML
<body>
<p id="sample">Hello</p>
 
<script>
const result = $('p').attr('id', 'text');
 
console.log( result[0] );
</script>
</body>

/* 执行结果*/
/*
<p id="text">Hello</p>
*/
```
添加元素的属性：
```HTML
<body>
<input>
 
<script>
const result = $('input').attr({
    id: 'text',
    class: 'form',
    type: 'checkbox',
    value: 'one',
    checked: true
});
 
console.log( result[0] );
</script>
</body>

/* 执行结果*/
/*
<input id="text" class="form" type="checkbox" value="one" checked="checked">
*/
```
***
### .removeAttr()
.removeAttr()用于删除对象元素的属性
```HTML
<body>
<p class="text">Hello</p>
 
<script>
const result = $('p').removeAttr('class');
 
console.log( result[0] );
</script>
</body>


/* 执行结果*/
/*
<p>Hello</p>
*/

```

### .prop()
prop()和removeAttr()十分相似，不同在于prop能够确认属性是否存在的状态。
当某个属性比如checked / disabled不存在时，和removeAttr()返回undefined,而prop返回false.
```HTML
<body>
<button class="btn1">Button1</button>
<button class="btn2" disabled>Button1</button>
 
<script>
const result1 = $('.btn1').attr('disabled');
const result2 = $('.btn2').attr('disabled');
 
const result3 = $('.btn1').prop('disabled');
const result4 = $('.btn2').prop('disabled');
 
 
console.log( result1 );
console.log( result2 );
console.log( result3 );
console.log( result4 );
</script>
</body>

/* 执行结果*/
/*
undefined
disabled
 
false
true
*/

```
***
### .css()
.css()能够进行元素css的设定，添加，获取，修改等。
元素css的设定
```HTML
<body>
<p>sample text</p>
 
<script>
</script>
    $('p').css('color', '#f00');
</body>

/* 执行结果
   将p内的文字颜色变红（#f00）
*/

```

修改元素的css
```HTML
<body>
<p style="font-size:12px">sample1</p>
<p style="font-size:16px">sample2</p>
<p style="font-size:20px">sample3</p>
 
<script>
    $('p').css('font-size', function(index, value) {
        var newValue = parseInt(value) + 6;
    
        return newValue + 'px';
    })
</script>
</body>

/* 执行结果 */
/* 

<p style="font-size:18px">sample1</p>
<p style="font-size:22px">sample2</p>
<p style="font-size:26px">sample3</p>
*/
```

### .addClass()
用于给任何一个元素添加css

如果p元素没有任何的样式，则添加addRed样式。index为该对象HTML元素的下标，myclass为该元素最初自身拥有的class属性名
```HTML
<p class="addBlue">Good Morning</p>
<p class="addGreen">Hello</p>
<p>こんばんは</p>

$('p').addClass(function( index, myclass ) {
 
    if( !myclass ) {
        return 'addRed';
    }
    
})
```

### removeClass()
removeClass()用于给任何元素删除css，有重复的情况下，删除所有匹配元素的css。
复数指定时用空格隔开，不传参则对象元素全部删除css。

```HTML
<ul>
    <li class="test">list1</li>
    <li class="sample">list2</li>
    <li class="text">list3</li>
</ul>

$('li').removeClass('test sample');

/* css为text和sample的list2，list3的css被删除 */
```

### .hasClass()
hasClass()用于查看对象元素是否存在某样式css。存在返回true，不存在返回false。
复数的情况下用空格隔开，并且要求搜索参数值与css值完全一致。
```HTML
<ul>
      <li class="red">リスト１</li>
      <li class="blue">リスト２</li>
      <li class="red green">リスト３</li>
</ul>


var li = $('li').hasClass('red green');
 
console.log(li);

/* 执行结果 */

/* true */
```

### .toggleClass()
toggleClass()可以操作对象的class属性，并进行添加，删除等循环操作。

利用toggleClass()进行mytoggle的显示/隐藏的切换：
```HTML
.mytoggle {
    display: none;
}

<body>
<h1>Title</h1>
<button>Button</button>
 
<script>
    $('button').click(function() {
        $('h1').toggleClass('mytoggle');
    })
</script>
</body>
```

### .each()
.each()用于循环历遍每个元素。相当于forEach。

对HTML元素的操作：
```HTML
<ul>
    <li>sample1</li>
    <li>sample2</li>
    <li>sample3</li>
</ul>

$('li').each(function(index, element) {
 
    console.log(index);
    console.log($(element).text());
 
})

/* 执行结果*/

sample1 
1 
sample2 
2 
sample3
```

对数列的操作：
```HTML
var array = [3,6,2,8,6];
 
$.each(array, function(index, value) {
 
    console.log(index + ': ' + value);
 
})

/* 执行结果*/
0: 3 
1: 6 
2: 2 
3: 8 
4: 6

```

### $.extend()
$.extend()用与连结两个或多个对象，将其整合为一个对象。
无指定则在第一个传递的对象上进行覆盖，如果想保留原对象，则第一参数传空{}

```HTML
var user1 = {
  name: '太郎',
  area: 'tokyo'
}
 
var user2 = {
  name: '花子',
  age: 28
}

var result = $.extend({}, user1, user2);
 
console.log(result);
console.log('------------');
console.log(user1);

/* 执行结果 */
Object { name: "花子", area: "tokyo", age: 28 }
------------
Object { name: "太郎", area: "tokyo" }
```

### .clone()
.clone()用于复制对象和元素。参数要指定true，false时或不传参数时不能复制。

按键时按钮被复制：
```HTML
<button name="clone">Clone!</button>

<script>
  $('button[name=clone]').on('click', function () {
    $(this).clone(true).insertAfter(this);
  });
</script>
```

### .index()
.index()用于取得元素的下标

点击按钮时获取下标：
```HTML
<ul>
  <li>北海道</li>
  <li>東北</li>
  <li>関東</li>
  <li>東海</li>
  <li>関西</li>
  <li>中国</li>
  <li>四国</li>
  <li>九州</li>
</ul>
 
<p><span id="num">－</span>几个</p>


$(function() {
 
  $('li').click(function() {
 
    var i = $('li').index(this);

    $('#num').text(i);
 
  });
});

```

### .ready()
.ready()可以忽略浏览器默认加载，在DOM准备好后就立即执行这个函数。

下面的函数执行时，由于对h1的操作在body的h1生成之前，所以相当于无效。
```HTML
<head>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
        $('h1').text('Hello World');
    </script>
</head>
 
<body>
    <h1>こんにちは</h1>
</body>
```

此时使用.ready()，则可以在h1加载完成时再立马执行处理。

```HTML
<head>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
 
            $('h1').text('Hello World');
 
        })
    </script>
</head>
 
<body>
    <h1>こんにちは</h1>
</body>
```

还可以简写成下面的形式:

```HTML
$(function() {
 
    //your coding
 
});

or

$(showLog);

function showLog() {
 
    console.log('Hey! Let's see log);
 
}
```