---
title: About jQuery ajax & jsonp
date: 2019-08-30 20:22:41
tags:
- jQuery
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

关于jQuery ajax & jsonp
<!--more-->

### jQuery.ajax([settings])

我们可以用ajax发送了请求(get/post)

ajax方法提供了几个常用的setting

- async：默认设置下，所有请求均为异步请求（也就是说这是默认设置为 true ）。如果需要发送同步请求，请将此选项设置为 false.

- beforeSend：请求发送前的回调函数，用来修改请求发送前jqXHR对象，此功能用来设置自定义 HTTP 头信息，等等。该jqXHR和设置对象作为参数传递

- cache：如果设置为 false ，浏览器将不缓存此页面。注意: 设置cache为 false将在 HEAD和GET请求中正常工作。它的工作原理是在GET请求参数中附加"timestamp"

- context：这个对象用于设置Ajax相关回调函数的上下文。 默认情况下，这个上下文是一个ajax请求使用的参数设置对象.

- data：发送到服务器的数据。将自动转换为请求字符串格式。GET 请求中将附加在 URL 后面，POST请求作为表单数据.

- headers：一个额外的{键:值}对映射到请求一起发送。此设置会在beforeSend 函数调用之前被设置 ;因此，请求头中的设置值，会被beforeSend 函数内的设置覆盖

- method：HTTP 请求方法 (比如："POST", "GET ", "PUT"，1.9之前使用“type”)。    

了解了这些参数，使用jQuery处理ajax请求就简单了

例子:

```JS
$.ajax({
  method: "POST",
  url: "some.php",
  data: { name: "John", location: "Boston" }
}).done(function( msg ) {
  alert( "Data Saved: " + msg );
});
```
***

### jQuery.get( [settings] ) / jQuery.post( [settings ] )

这两个方法专门用来处理get和post请求,
dataType：从服务器返回的预期的数据类型。默认：智能猜测（xml, json, script, 或 html）

```JS
$.ajax({
  url: url,
  data: data,
  success: success,
  dataType: dataType
});

$.ajax({
  type: "POST",
  url: url,
  data: data,
  success: success,
  dataType: dataType
});
```

***

### jQuery.getJSON( url [,data] [success(data, textStatus, jqXHR)])

使用一个HTTP GET请求从服务器加载JSON编码的数据，这是一个Ajax函数的缩写，这相当于:
```JS
$.ajax({
  dataType: "json",
  url: url,
  data: data,
  success: success
});
```


