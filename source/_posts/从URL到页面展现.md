---
title: What Happens When You Type in a URL
date: 2018-11-10 16:54:13
tags:
- URL
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

从URL到页面展现
<!--more-->
专有名词解释：URL：统一资源定位符 http : 网络协议 用于找到互联网上的资源
### 1.在浏览器输入URL

通过输入「http://www.baidu.com」 的URL来最终定位找到百度首页。
***

### 2.域名解析
对于　「http://baidu.com」　的URL来说，浏览器实际上不知道baidu.com到底是什么，需要对应查找到该域名对应的服务器IP地址才能找到目标。
域名解析的流程

1.浏览器缓存
如果你之前打开过百度首页，那么百度的ip地址会被缓存到浏览器里，当你打开时百度首页时就能从浏览器缓存里获取之前缓存的百度ip地址并访问它。

2.系统缓存
如果你是第一次打开百度，那么无法从浏览器获取缓存，便会从你电脑的Hosts文件查找是否有该域名和其对应ip。
下图为Mac电脑的hosts文件内容。开发时可以修改hosts文件内的ip，达到打开本地文件的效果。

<img src="./1.png" style="width:500px">

3.路由器缓存
如果浏览器缓存和系统缓存都没有，就会看你的路由器缓存。路由器曾经登陆过也会缓存域名信息，如果你或别人在该路由器上登陆过网站，则可以获取到baidu的ip地址。

4.ISPDNS缓存
如果路由器也没有缓存就会找你的服务商，比如到电信的DNS上查找。

5.如果都没有找到就会到你的根域名服务器查找域名对应ip，根域名服务器把请求转发到下一级，直到找到ip。(找不到就返回404 找不到服务器)

***
### 3.服务器处理
服务器是一台安装电脑的机器，常见的系统如Linux，Windows Server 系统里安装的处理请求的应用叫做Web Server。
常见的Web服务器有Apache，Nginx，IIS，Lighttpd等。Web服务器接收用户的Request交给网站代码，或者接受请求反向代理到其他Web服务器。也就是一个管理者的作用。
下图的白色区域为Web服务器。

<img src="./2.png" style="width:500px">

***
### 4.网站处理流程
经服务器处理后，网站接受请求后进行处理，最后将页面呈现给用户。
比如下图的MVC模型

<img src="./3.png" style="width:500px">

***
### 5.浏览器读取并再次请求
HTML字符串被浏览器接受后被一句句读取解析，解析到link标签后重新发送请求获取CSS。解析到script标签后发送请求获取js，并执行代码。解析到img标签后发送请求获取图片。

***
### 6.浏览器渲染
浏览器根据获取到的HTML和CSS计算并渲染，绘制到屏幕上的js会被执行。
到此你就能看到你所打开的网页了。

