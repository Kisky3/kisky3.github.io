---
title: SSR VS CSR
date: 2019-12-09 10:59:42
tags:
- SSR
- CSR
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
SSR和CSR
<!--more-->
本文分别从两者的概念，主要的不同，优劣势列举等等去分析CSR和SSR。

### SSR和CSR的概念
- SSR(Server Side Rendering) ：
传统的渲染方式，由服务端把渲染的完整的页面吐给客户端。这样减少了一次客户端到服务端的一次http请求，加快相应速度，一般用于首屏的性能优化。
<img src="./1.png" style="width: 500px">
<br>
- CSR(Client Side Rendering)：
是一种目前流行的渲染方式，它依赖的是运行在客户端的JS，用户首次发送请求只能得到小部分的指引性HTML代码。第二次请求将会请求更多包含HTML字符串的JS文件。
<img src="./2.png" style="width: 500px">
***
### 两者有何不同
服务器端渲染的优势在于首屏渲染速度块，简单来讲它不需要来回多次往返于客户端和服务端。但是其性能等众多因素会影响用户体验，比如说：网速，在线活跃人数，服务器的物理位置等等。而客户端渲染则和服务端渲染相反，因为多次和服务器的交互导致首屏加载速度慢。但一旦这些请求完成之后，用户和页面之间的交互时用户体验就会好很多。

用一个现实生活的例子来看：假如从超市买东西吃，以SSR的角度来看，你每次在超市买完随即吃完再走，每次饿了都需要出发去超市。而从CSR的角度来看，就是你从超市购买许多原材料再拿回家去自己煮，多了能放冰箱，这样每次肚子饿了就不需要每次都往超市跑，唯一麻烦一点在于你得花时间挑选食材。

<p style="font-weight:bold;background:rgb(255, 0, 0,0.1);padding:5px">简而言之，SSR强在首屏渲染。而CSR强在用户和页面多交互的场景。</p>

***
### 服务端渲染如何工作
服务器渲染简单说来就是将一个完整的HTML发送给客户端，客户端只负责HTML的解析。
不过由于网速等影响有可能造成用户体验不佳的情况。
如果面临客户端和服务器多次交互的情况就更为明显了。因为即使是在页面只有稍加改动的地方都需要重新请求服务器发送一个完整页面给浏览器，然后浏览器再次进行重新渲染。
这对服务器的消耗是非常大的。

实例：
假如你需要访问的域名叫： example.testsite.com
 ```html
 <html>
  <head>
    <meta charset="utf-8">
    <title>Example Website</title>
  </head>
  <body>
    <h1>My Website</h1>
    <p>This is an example of my new website</p>
    <a href="http://example.testsite.com/other.html.">Link</a>
  </body>
</html>
 ```

 当我们点击Link的时候，就会跳转到下面这个页面:
 ```html
 <!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Example Website</title>
  </head>
  <body>
    <h1>My Website</h1>
    <p>This is an example of my new website</p>
    <p>This is some more content from the other.html</p>
  </body>
</html>
 ```
 效果是当你点击Link的时候，
 可以看出来这两个页面的差异非常小，但是渲染的时候要将整个页面进行重新渲染，而不是只渲染不同的那一行。
 如果复杂的页面代码量大的情况下，这是不合适的。

 <p style="font-weight:bold;background:rgb(255, 0, 0,0.1);padding:5px">从上面的页面特征来看，使用服务器端渲染的返回的页面是完整的HTML页面。</p>

 ***
 ### 客户端是如何工作的
 客户端代表渲染内容部分转嫁到JS身上。客户端只是从服务器得到相对简单的HTML文档，然后使用JS文件对页面的显示内容进行控制。就像Vue.js 用的就是这种方式，还是用刚才那个例子，看看用客户端渲染是怎么做的。

 假设你想要访问的域名还是http://example.testsite.com
 ```html
 <!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Example Website</title>
</head>
<body>
  <div id="root">
    <app></app>
  </div>
    <!--下面两个js文件控制HTML的显示内容 -->
  <script src="https://vuejs.org"type="text/javascript"></script>
  <script src="location/of/app.js"type="text/javascript"></script>
</body>
</html>
 ```
 可以通过上面的html页面看到，你只能识别到自定义组件名id为root的div的标签。
 而所有的逻辑处理你看不到，全部被写在了app.js里面。
如果需要看到完整的页面的模样，就必须要下载app.js到本地才能运作，否则直接访问你是什么都看不到的。

OK，假如现在你把app.js下载到本地了，那我们来看看app.js的内容: 
```html
// this is app.js
var data = {
        title:"My Website",
        message:"This is an example of my new website"
      }
  Vue.component('app', {
    template:
    `
    <div>
    <h1>{{title}}</h1>
    <p id="moreContent">{{message}}</p>
    <a v-on:click='newContent'>Link</a>
    </div>
    `,
    data: function() {
      return data;
    },
    methods:{
      newContent: function(){
        var node = document.createElement('p');
        var textNode = document.createTextNode('This is some more content from the other.html');
        node.appendChild(textNode);
        document.getElementById('moreContent').appendChild(node);
      }
    }
  })
  new Vue({
    el: '#root',
  });

```
在这里，当你点击了Link的时候，Vue的js起了效果，通过操作DOM给p添加了需要展示的那一行内容。
这里不需要页面和服务器的交互，只是对所需要添加的内容进行了修改，而不是像SSR一样做了整个页面的重新渲染。

当然这里也暴露了一个问题:
就是控制页面的所有js文件如果没有完全加载的话，真个页面是渲染不出来的。
这是导致了客户端渲染弱于服务端渲染的原因。

<p style="font-weight:bold;background:rgb(255, 0, 0,0.1);padding:5px">从这个例子来看，可以看出客户端渲染的页面特征是包含有js链接的script标签。</p>

### 总结SSR和CSR的优劣势
|      |  优势  |  劣势  |
| ---- |  ---- | ---- |
|  SSR  |  1.搜索引擎可以抓取网站以获得更好的SEO <br> 2.初始页面加载速度更快 <br> 3.非常适合静态网站 |  1.频繁的服务器请求 <br> 2.真题缓慢的页面渲染 <br> 3.整个页面重新加载 <br> 4.不擅长于网站交互  |
|  CSR  |  1.擅于网站交互 <br> 2.初始加载后网站的渲染速度很快 <br> 3.非常适合Web应用程序 <br>4.可以运用强大的JavaScript处理和选择 |  1.不利于SEO <br> 2.初始加载可能需要花费更多时间  |

***
### 参考
- [Client-side vs. server-side rendering: why it’s not all black and white](https://www.freecodecamp.org/news/what-exactly-is-client-side-rendering-and-hows-it-different-from-server-side-rendering-bd5c786b340d/)

- [JavaScript SEO: Server Side Rendering vs. Client Side Rendering](https://link.zhihu.com/?target=https%3A//medium.com/%40benjburkholder/javascript-seo-server-side-rendering-vs-client-side-rendering-bc06b8ca2383)
