---
title: HTTP Methods GET vs POST
date: 2018-10-25 16:18:03
tags:
- Get
- Post
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
Get和Post提交数据有什么区别
<!--more-->
{% hl_text #FFCCCC %}1.post更安全，安全要求高的用post 要求低的用get{% endhl_text %}
<br>

<br>
{% hl_text #FFCCCC %}2.post发送的数据更大（get有url长度限制）{% endhl_text %}
<br>
<br>
{% hl_text #FFCCCC %}3.post能发送更多的数据类型{% endhl_text %}
<br>
<br>
{% hl_text #FFCCCC %}4.post比get慢{% endhl_text %}
<br>
(原因:post在真正接收数据之前会先将请求头发送给服务器进行确认，服务器返回100 Continue响应之后才真正发送数据 )
<br>
<br>
{% hl_text #FFCCCC %}5.post用于向后台传数据，get一般用于向后台要数据。{% endhl_text %}