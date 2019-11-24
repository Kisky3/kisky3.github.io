---
title: CSRF Attack And Prevention
date: 2018-10-14 13:58:15
clearReading: true
thumbnailImage: 20181014.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
tags: 
- CSRF
categories: Back-end Knowledge
---
CSRF攻击及防范措施
<!--more-->
### CSRF是什么
CSRF全称为跨站请求伪造（Cross-site request forgery），

是一种网络攻击方式，也被称为 one-click attack 或者 session riding。
简单说来就是已经利用登陆成功的User强制实行某些操作的恶意攻击行为。

***

### CSRF攻击原理
其原理是攻击者构造网站后台某个功能接口的请求地址，诱导用户去点击或者用特殊方法让该请求地址自动加载。
用户在登录状态下这个请求被服务端接收后会被误以为是用户合法的操作。对于 GET 形式的接口地址可轻易被攻击，
对于 POST 形式的接口地址也不是百分百安全，攻击者可诱导用户进入带 Form 表单可用POST方式提交参数的页面。
<img src="./1.png" style="width:500px">

角色：
- 正常浏览网页的用户： User
- 正规的但是具有漏洞的网站： WebA
- 利用CSRF进行攻击百度网站： WebB

例子：
比如有shop.example.com这样一个购物网站，用户通过用户名和密码可以登录。其中有点击按钮重设密码的功能。
当用户点击按钮更改密码时，下图的送信请求将会被提交至WebA的服务器

URL：
 1. http://shop.example.com/password/change
 2. Parmeter:
 3. new_pass:XXXXX
 4. new_pass_conf:XXXXX

说明：
1.User正常登陆网页WebA，WebA通过用户的认证并在User的浏览器中产生Cookie(证明是User本人登陆)

2.攻击者伪造能发送同样请求的网站WebB。利用简单的Javascript便可达到目的。

3.攻击者把该伪造的网站的URL放到img的src里上传，当User登陆后，打开网页时便会自动加载图片，WebB会利用用户的浏览器访问WebA。
由于User是在登录状态下，所以User的浏览器根据WebB的要求，带着1中生成的Cookie访问WebA。

4.WebA接收到User浏览器的请求，并带着用户的Cookie(如例子中的请求)，要求修改密码。

5.WebA误以为是用户的操作，响应修改密码的请求。User密码被盗。

以上Web便达到了在用户不知情的情况下，利用用户登陆后的Cookie进行用户的模拟操作过程。

***
### CSRF防范措施
1.服务端在收到路由请求时，生成一个随机数，在渲染请求页面时把随机数埋入页面
（一般埋入 form 表单内，）

2.服务端设置setCookie，把该随机数作为session种入用户浏览器。
(加入保存在Cookie中，旧Token消耗后，新的Token会被生成，造成用户混乱。而Session能避免此问题。)

3.当用户发送 GET 或者 POST 请求时带上_csrf_token参数
（对于 Form 表单直接提交即可，因为会自动把当前表单内所有的 input 提交给后台，包括_csrf_token）

4.后台在接受到请求后解析请求的cookie获取_csrf_token的值，然后和用户请求提交的_csrf_token做个比较，如果相等表示请求是合法的。

（下图是某电商网站的真实设置，这里页面上设置的 token和session里设置的token 虽然不直接相等，但 md5(‘1474357164624’) === ‘4bd4e512b0fbd9357150649adadedd4e’，后台还是很好计算的）

<img src="./2.png" style="width:500px">

<img src="./3.png" style="width:500px">

注意：尽量避免使用Get。因为能在发送请求时能在URL处暴露token信息。

***
### 参考
- [「每日一题」CSRF 是什么？](https://zhuanlan.zhihu.com/p/22521378);
- [CSRF攻击原理及防护](https://www.jianshu.com/p/00fa457f6d3e);



