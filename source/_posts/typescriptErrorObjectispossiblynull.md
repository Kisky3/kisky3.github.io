---
title: Typescript Error「Object is possibly null」
date: 2021-01-04 00:03:57
tags:
 - Vue
 - Typescript
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
typescript 提示 「Object is possibly null」的N种解决方法
<!--more-->
---
```js
document.querySelector('.main-table').setAttribute('height', '300px');
```

如上，我要设置某元素的高度，但typescript提示 Object is possibly ‘null’，是因为可能不存在选择元素的情况。

### 解决方案一
最正确的解决方案，就是加null的判断。
```js
const table = document.querySelector('.main-table');
if (table) {
  table.setAttribute('height', '300px');
}
```

***
### 解决方案二
使用断言方式，当然这是你能保证元素必定存在的情况。
```js
(document.querySelector('.main-table') as Element).setAttribute('height', '300px');
```
***
### 解决方案三
这和解决方案原理一样，要判断null情况，但写法简单点，当然这是关闭eslint的情况下，否则eslint会提示错误。
```js
document.querySelector('.main-table')?.setAttribute('height', '300px');
```
这里使用了 ?. 符号，相当于&&，意思是先判断?前面的对象是否存在，存在情况下再执行后面的方法；
使用下面代码也是可以的：

```js
const table = document.querySelector('.main-table';
table && table.setAttribute('height', '300px');
```

