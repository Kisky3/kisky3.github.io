---
title: Set localStorage Wih JS , Read With CakePHP
date: 2021-04-19 23:55:01
tags:
- CakePHP
- Cookie
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Back-end Knowledge
---
在js里设定localStorage,并在CakePHP里使用
<!--more-->
在最近要在js里设定localStorage,并在CakePHP里读取js的localStorage.而且使用js和CakePHP的地方分别处于两个repo.

为了解决这个问题,费了一点时间.
下面不知道是不是最好的方法,但是能够满足需求了.

1. 在Javascript里设定localStorage

<!-- javascript -->
```js
localStorage.setItem('mytext', 'Hello, World!');
```

如果对期限有要求的话可以考虑使用sessionStorage, 或者自己将时间的数据存到storage里, 并在读取localstorage时获取现在时刻进行对比并追加处理.

2. 在CakePHP里创建Element,并在template里通过js引用.
例子：

my-project/myname/Template/Element/helloworld.tpl
```js
<script type="text/javascript">
var test = localStorage.getItem("mytext");
console.log(test);
</script>
```


CakePHP template

test.tpl
```js
{$this->Element('helloworld')}
// other CakePHP coding...
```

这样在加载test.tpl的时候就能够通过element读取之前保存在本地的localstorage了.
