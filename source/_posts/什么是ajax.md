---
title: What is Ajax
date: 2019-06-05 17:04:23
tags:
- JS
- ajax
clearReading: true
thumbnailImage: 20190605.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

什么是Ajax
<!--more-->

### 什么是ajax
ajax是一种技术方案，但并不是一种新技术。

它依赖的是现有的CSS/HTML/Javascript，而其中最核心的依赖是浏览器提供的XMLHttpRequest对象，是这个对象使得浏览器可以发出HTTP请求与接收HTTP响应。 
实现在页面不刷新的情况下和服务端进行数据交互。

ajax可以理解就是，以前向服务器请求资源，必须对这个页面资源进行请求以获得这个信息资源（以这个页面资源为载体来携带信息资源），这必然会对页面进行刷新（因为是请求服务器后会同步返回一个页面进行刷新）。
现在页面可以通过浏览器脚本编程语言调用一个隐藏请求装置（也就是XMLHttpRequest），由这个请求向服务器请求资源，然后返回一个资源载体（可能是一个页面，也可能是一个xml或json文段），然后由编程语言去处理这个信息。与此同时，页面是不会发生刷新行为的（也就是没有向服务器请求这个页面资源）。这就是异步原理了。“AJA”就是异步JavaScript的缩写，其基础就是浏览器脚本编程语言JavaScript和XMLHttpRequest对象，X就是作为信息载体的XML，不过现在多数用JSON代替。

作用就是可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

### ajax范例

GET
```js
var xhr = new XMLHttpRequest()
xhr.open('GET', 'http://api.jirengu.com/weather.php', true)
xhr.onreadystatechange = function(){
    if(xhr.readyState === 4) {
        if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
            //成功了
            console.log(xhr.responseText)
        } else {
            console.log('服务器异常')
        }
    }
}
xhr.onerror = function(){
    console.log('服务器异常')
}
xhr.send()
```

POST
```JS
var xhr = new XMLHttpRequest()
  xhr.timeout = 3000        //可选，设置xhr请求的超时时间
  xhr.open('POST', '/register', true)

  xhr.onload = function(e) { 
    if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
      console.log(this.responseText)
    }
  }
    //可选
  xhr.ontimeout = function(e) { 
        console.log('请求超时')
  }

  //可选
  xhr.onerror = function(e) {
      console.log('连接失败')
  }
  //可选
  xhr.upload.onprogress = function(e) {
      //如果是上传文件，可以获取上传进度
  }

  xhr.send('username=jirengu&password=123456')
```

封装一个ajax
```JS
function ajax(opts){
    var url = opts.url
    var type = opts.type || 'GET'
    var dataType = opts.dataType || 'json'
    var onsuccess = opts.onsuccess || function(){}
    var onerror = opts.onerror || function(){}
    var data = opts.data || {}

    var dataStr = []
    for(var key in data){
        dataStr.push(key + '=' + data[key])
    }
    dataStr = dataStr.join('&')

    if(type === 'GET'){
        url += '?' + dataStr
    }

    var xhr = new XMLHttpRequest()
    xhr.open(type, url, true)
    xhr.onload = function(){
        if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
            //成功了
            if(dataType === 'json'){
                onsuccess( JSON.parse(xhr.responseText))
            }else{
                onsuccess( xhr.responseText)
            }
        } else {
            onerror()
        }
    }
    xhr.onerror = onerror
    if(type === 'POST'){
        xhr.send(dataStr)
    }else{
        xhr.send()
    }
}

ajax({
    url: 'http://api.jirengu.com/weather.php',
    data: {
        city: '北京'
    },
    onsuccess: function(ret){
        console.log(ret)
    },
    onerror: function(){
        console.log('服务器异常')
    }
})
```