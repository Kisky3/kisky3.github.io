---
title: JS Developing History And New Characteristics Of ES6
date: 2019-02-10 17:59:27
tags:
- JS
clearReading: true
thumbnailImage: 20190210.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

JS发展简史及ES6新特性
<!--more-->

### JS标准制定简史
1995年，只花了10天时间 Brendan 就开发完成了当时被称作Mocha的初版 JavaScript 原型。

那时候的JS比较简陋，没有数组和字面量的对象的概念，所有的报错都只能通过丑陋的alert展示，缺乏异常处理机制，出错时很多运算的结果会是NaN或undefined。不过 Brendan 对 DOM 0 的描述及初版的JavaScript还是成为了最初的标准。

1995年9月，JavaScript也被包装为LiveScript一同面世。1995年12月，Netscape Navigator 2.0 beta3发布，LiveScript在这时被改名为JavaScript(当时这个商标为Sun公司所有，现在属甲骨文公司)。之后不久,网景推出了LiveWire，一种在其服务器（Netscape Enterprise Server）上的JavaScript实现1。

1996年，微软推出了JScript，同ie3捆绑发行,JScript在微软的IIS服务器上同样可用。

1996年开始,JS语言开始走上规范之路，由于当时Sun公司不愿意转让JavaScript这一商标，虽然微软愿意转让JScript这一商标,但却遭到其它公司成员的反对，因此这一语言的名字就成了我们熟悉的ECMAScript。

1997年6月ECMA-262的第一版发布，之后一年中，规范依据ISO / IEC 16262国际标准进行了改进，并由ISO认证机构大量审查，1997年6月ECMAScript规范正式发布第二版。

1999年12月，ES3也发布了，这一版的规范带来了正则表达式，switch，do/while,try/catch，Object#hasOwnProperty以及其它的一些改变，同时新增的大部分规范在网景的新版浏览器SpiderMonkey中也得以实现。
ES4标准的草案在之后不久就被TC39提出，这一草案直接影响了2000年中期的JScript,.NET等的开发，2006年Flash推出了ActionScript 3也深受其影响。

但是关于JavaScript语言该如何发展，当时的意见非常矛盾，这使规范制定的工作停滞不前。这在Web标准指定史上是一个非常尴尬且奇妙的时刻，当时微软掌握着主动权，但是它对规范的改进却没太大的兴趣。

两年后，随着火狐浏览器市场占有率不断增高，就职于 Mozilla 的 Brendan 迫使微软回到标准指定的议程中。2005年中期开始，TC39委员会又开始了例会。重新开始讨论起ES4，他们计划在ES4中引入模块系统，类，迭代器，生成器，解构，类型注释，适当的尾调用，新的数据类型和各种其他功能,由于这个工程的野心太大，ES4的制定因而被一而再的延期。

2007年，TC39委员会被迫分为两部分，一部分负责ES3的渐进加强版ES3.1标准的制定，另一部分则负责重新设计改动巨大的ES4标准。
2008年8月，ES3.1被认为是正确的选择，随后其更名为ES5， ES4也随之被废弃，不过值得庆幸的是 ES4 提出的很多新功能被融入到了 ES6 ，也有一些功能仍然在考虑之中，另一些则已被放弃，拒绝或撤回。兼容ES3.1 成为 ES4 标准提出的功能可能被采纳的前提。 2009年12月，ES3发布10周年后，第五版ECMAScript才得以发布。这个版本把十年来各浏览器中已有的普遍实践标准化了，新增了get`set`，改进了数组原型的函数式特征，原生支持了JSON的解析，提出了严格模式。 2011年6月，ES5标准再次修改并改进为 ISO/IEC 16262:2011标准 的第三版，并以ES5.1的名义正式发布。 2015年6月，也就是ES5.1发布的四年后，TC39公布了JS语言有史以来最大的更新 ES6， 其中包含了很多ES4中提出草案。

ES6的发布是JS标准制定历史上的一个重要里程碑。除了数十种引入注目的新功能，ES6 的发布也标志着 ECMAScript 标准将持续更新。

***

### ES6中的新特性

ES6改动非常大，这从规范的页数就可以看出，ES5.1 258页，ES6 566页。总的来说新增的规范可以划分为以下不同类别：

- 语法糖

- 新机制

- 更好的语义化

- 更多内置的方法

现存局限方法的非破坏性解决方案

语法糖是ES6所有改变中最重要的一块，class语法可简洁的构建对象实例，支持使用箭头函数，简写的对象属性方法，解构，剩余值，和拓展，等提供语义良好的编写程序的方法。

ES6为我们提供了几种控制异步代码流的机制：包括可靠的promises，表征一系列值的iterators，特殊的iterators–> generators。基于这些新概念,ES2017还有了了async/await语法，让我们以同步的方式来书写异步代码。

ES6提供了一些新的内置类型来管理set和map,这些新类型不具有仅使用字符串键的限制

Proxy对象重新定义了我们通过JavaScript reflection可以做什么，Proxy对象类似于其它上下文中的代理。可以用以修改与JavaScript对象的任何交互，如定义、删除或访问属性。考虑到代理对象的工作机制，他们不能彻底通过ployfill实现，事实上相关的ployfill也是存在的，但是由于存在局限性使得他们在某些方面与规范有所不一致。

除此之外，ES6还在语言层面上为我们提供了模块系统,ES6对Number,Math,Array和string等都进行了更新，提供了新的api。