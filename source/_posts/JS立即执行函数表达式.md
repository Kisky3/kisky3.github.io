---
title: JS Immediately Invoked Function Expression
date: 2019-03-02 19:13:39
tags:
- JS
clearReading: true
thumbnailImage: 20190302.jpg  
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

JS立即执行函数表达式
<!--more-->

### 什么是JS作用域
JS作用域表示变量或函数起作用的区域，指代了它们在什么样的上下文中执行，亦即上下文执行环境。

Javascript 的作用域只有两种：全局作用域和本地作用域，本地作用域是按照函数来区分的。       

#### 全局变量：

声明在函数外部的变量，在代码中任何地方都能访问到的对象拥有全局作用域（所有没有var直接赋值的变量都属于全局变量）


1.最外层函数和在最外层函数外面定义的变量拥有全局作用域         
  
```JS
var authorName="Dream";
function doSomething(){
    var blogName="blogName";
    function innerSay(){
        alert(blogName);
    }
    innerSay();
}
alert(authorName); //Dream
alert(blogName); //脚本错误
doSomething(); //blogName
innerSay() //脚本错误
```

2.所有末定义直接赋值的变量自动声明为拥有全局作用域
例：变量blogName拥有全局作用域，而authorName在函数外部无法访问到。

```JS
function doSomething(){
    var authorName="Dream";
    blogName="blogName";
    alert(authorName);
}
doSomething(); //Dream
alert(blogName); //blogName
alert(authorName); //脚本错误
```

#### 局部变量（函数作用域）：

声明在函数内部的变量，和全局作用域相反，局部作用域一般只在固定的代码片段内可访问到。
例：代码中的blogName和函数innerSay都只拥有局部作用域。

```JS
function doSomething(){
    var blogName="Dream";
    function innerSay(){
        alert(blogName);
    }
    innerSay();
}
alert(blogName); //脚本错误
innerSay(); //脚本错误
```

***

### 什么是JS作用域链

每当执行一个函数就会进入一个新的作用域下，当使用一个变量时首先从自己的作用域找，如果找到就输出，如果没有再从自己的上层作用域找，也就是该函数声明的作用域。
这就是js的作用域链。

例：执行下面fn（）时输出结果为 2

- 因为return fn3，所以执行fn3内的fn2，
- 首先从fn2的作用域内找变量a，
- 而fn2内没有声明变量a，所以从fn2的上层作用域，也就是声明fn2的fn1下找变量a，
- 在fn1下找到 var a = 2

```JS
var a = 1
function fn1() {
  function fn2(){
    console.log(a);
  }
  function fn3() {
    var a = 4
    fn2()
  }
  var a = 2
  return fn3
}
var fn = fn1()
fn() // 2
```
***

### 什么是JS立即执行函数

首先声明一个匿名函数 function(){alert(‘我是匿名函数’)}。然后在匿名函数后面接一对括号 ()，调用这个匿名函数。

```JS
(function(){alert('我是匿名函数')})()
```

用括号把函数包起来其实是为了兼容 JS 的语法。如果我们不加另一对括号，直接写成下面这样浏览器会报语法错误。

```JS
function(){alert('我是匿名函数')}()
```

立即执行函数只有一个作用：创建一个独立的作用域。
这个作用域里面的变量，外面访问不到（即避免「变量污染」）。