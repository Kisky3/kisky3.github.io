---
title: About Same Origin Policy And How To Deal With It
date: 2019-06-10 17:23:16
tags:
- JS
- CORS
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

关于跨域
<!--more-->

### 同源策略（Same origin Policy）和跨域
浏览器处于安全方面的考虑，只允许与本域下的接口进行交互。
不同源的客户端脚本在没有明确授权的情况下。不能读写对方的资源。

简而言之就是你当前网页的协议名，域名和端口名与你请求的接口地址的各项是否相同，如果不相同则浏览器将会对返回的数据进行拦截。
这是隔离潜在恶意文件的关键安全机制。

本域指的是：
- 同协议： 比如都是http或者https
- 同域名： 比如都是http://kisky3.com/a 和 http://kisky3.com/b
- 同端口： 比如都是80端口

注意：对于当前页面来说页面存放的JS文件的域不重要，而是指当前页面的URL，也就是加载JS页面所在的域。

而跨域就是绕过浏览器的同源策略，让前端能够获取到数据。

### 跨域的几种方法
#### JSONP（JSON with padding）
JSONP就是通过script标签加载数据的方式去获取数据当作JS代码来执行。
提前在页面上声明一个函数。函数名通过接口传参的方式传给候梯，后台解析道函数名后在原始数据上包裹这个函数名，发送给前端。
换句话说，JSONP需要对应接口的后端的配合才能实现。

例如

后端服务器server.js
```JS
app.get('/getNews', function(req, res){

	var news = [
		"this is a test message 1",
		"this is a test message 2",
		"this is a test message 3",
		"this is a test message 4",
		"this is a test message 5",
		"this is a test message 6"
	]

	var data = [];
	for(var i=0; i<3; i++){
		var index = parseInt(Math.random()*news.length);
		data.push(news[index]);
		news.splice(index, 1);
	}

    // This is the point!! Backend has to check whether front request ask for a callback date or not
    // If so retuen a callback object contains JSON data => cb + '('+ JSON.stringify(data) + ')'
	var cb = req.query.callback;
	if(cb){
		res.send(cb + '('+ JSON.stringify(data) + ')');
	}else{
		res.send(data);
	}
})
```

前端页面 index.html
```JS
// Front created a script with callback object and require date to backend server.js 
 $('.change').addEventListener('click', function(){
    var script = document.createElement('script');
    script.src = 'http://127.0.0.1/getNews?callback=appendHtml';
    document.head.appendChild(script);
    document.head.removeChild(script);
  })

```
JSONP的特性：
- 它不像XMLHttpRequest对象实现的Ajax请求那样受到同源策略的限制；它的兼容性更好，在更加古老的浏览器中都可以运行。
- 它只支持 GET 请求，而不支持 POST 请求等其他类型的 HTTP 请求。

#### CORS(Cross-Origin Resource Sharing)
CORS全称是跨域资源共享,是一种ajax跨域请求资源的方式，支持现代浏览器，IE支持10以上。
实现方法很简单，当你从XMLHttpRequest发送请求时，浏览器发现该请求不符合同源策略，会给改请求加一个请求头：Origin，
后台进行一系列处理，如果确定接受请求则在返回结果中加入一个响应头：Access-Control-Origin;
浏览器判断相应头中事都包含Origin的值，如果有则浏览器会处理响应，我们就可以拿到响应数据，如果不包含则浏览器直接驳回，这时我们无法拿到响应数据。所以CORS的表象是让你觉得它与同源的ajax请求没啥区别，代码完全一样。

前端和平常一样，利用ajax发送数据。
后端服务器 server.js
```JS
app.get('/getNews', function(req, res){

	var news = [
		"this is a test message 1",
		"this is a test message 2",
		"this is a test message 3",
		"this is a test message 4",
		"this is a test message 5",
		"this is a test message 6"
	]

	var data = [];
	for(var i=0; i<3; i++){
		var index = parseInt(Math.random()*news.length);
		data.push(news[index]);
		news.splice(index, 1);
	}

    // This is the point!! Backend has to add this to response header. So that front can get data
    res.setHeader('Access-Control-Allow-Origin','http://localhost:8080');

    // This is the point!! Allow anybody to get date
    res.setHeader('Access-Control-Allow-Origin','*');
})

```
CORS的特性：

- JSONP 只能实现 GET 请求，而 CORS 支持所有类型的 HTTP 请求
- 使用 CORS ，开发者可以是使用普通的 XMLHttpRequest 发起请求和获取数据，比起 JSONP 有更好的错误处理
- 虽然绝大多数现代的浏览器都已经支持 CORS，但是 CORS 的兼容性比不上 JSONP，IE10以下的浏览器不支持CORS

### 服务端中转跨域
JSONP、CORS 这两种跨域请求方式都需要对方服务器支持。假设对方服务器不提供支持怎么办？还有一个必杀技，自己搭建 server 中请求中转。

假设 我们的页面为 https://jirengu.github.io/weather/weather.html， 需要向 https://weather.com/now 这个接口发送请求获取数据，但此接口不支持JSONP 和 CORS跨域。

我们可以这样做

- 搭建服务器，创建接口，如 https://api.jirengu.com/weather
- 设置这个接口允许 CORS 跨域
- 我们的页面向自己的这个接口发请求
- 接口收到请求后，在服务器端向https://weather.com/now 这个接口要数据（在服务端不存在同源策略限制），拿到数据后，返回给前端页面。

