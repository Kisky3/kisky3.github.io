---
title: About React Virtual Dom And JSX
date: 2019-07-04 22:47:43
tags:
- React
- Virtual Dom
- JSX
clearReading: true
thumbnailImage: 20190704.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

关于React的虚拟Dom和JSX
<!--more-->
### React的实行理念 
首先JS的DOM操作一般经历了从页面获取已存在的元素，进行修改操作，再渲染回页面这三个过程。
比如页面里有一个span标签，JS想要编辑的时候需要先从页面获取标签(利用id等),进行操作(数字加1),再将编辑后的数据返回页面（innerText等）。
<img src="./1.png" style="width:600px;margin:40px 0">

而react有一种更先进的理念，也就相当于不从Dom获取数据，而是只是向页面更新数据。
比如一开始页面里什么都没有，react里有一个number变量，并且在JSX里生成一个span的对象（虚拟Dom),再将对象生同步成到页面中，JSX里进行span内容的修改操作，然后再一次自动更新到页面的span里。
<img src="./2.png" style="width:600px;margin:40px 0">

react在生成新的虚拟Dom之后,会与旧的虚拟Dom的内容进行比较,再将有变化的那一部分,同步到页面中。
而进行内部对象span的更新速度远比JS直接更新Dom的速度要快很多,并且少了从页面获取元素的这一过程,导致react的效率和性能都高于普通JS。

### 关于JSX
JSX不是html,而是相当于利用Html的形式来更简便地写JS。

JSX就是将下面的语法2翻译成上面的JS语法1，这相当于以简便的Html的形式来写JS，这里包含了几个虚拟Dom，也就是表示DOM节点的对象。
```JS
// JS语法1
function render(){
  let h = React.createElement;
  let div = 
  h('div',{className:'parent'},
    h('span',{className:'red'},number),
    h('button',{onClick:onClickButton},'+'),
    h('button',{onClickButton},'-')
  )
}

// 语法2
<div className="parent">
  <span className="red">{number}</span>
  <button onClick = {onClickButton}>+</button>
  <button onClick = {onClickButton2}>-</button>
</div>
```
不同的是将对象和函数用括号括起来{},然后JS就会从当前作用域往上找相应的变量。
- class要写作className（有时两种写法都可以）。
- 在onClick = {onClickButton}时要向react传递一个对象而不是返回值，相当于React.createElement('button',{onClick:onClickButton}),所以不加括号。

完整代码例子：

```JS
let number = 0;

let onClickButton = ()=> {
  number +=1
  render();
}

let onClickButton2 = ()=> {
  number -= 1
  render();
}

// JS语法1
function render(){
  let h = React.createElement;
  let div = 
  h('div',{className:'parent'},
    h('span',{className:'red'},number),
    h('button',{onClick:onClickButton},'+'),
    h('button',{onClickButton},'-')
  )
}

// 语法2
<div className="parent">
  <span className="red">{number}</span>
  <button onClick = {onClickButton}>+</button>
  <button onClick = {onClickButton2}>-</button>
</div>

ReactDOM.render(div,document.querySelector('#root'));
```

JSX的翻译机制
下面的左右代码是等价的。
<img src="./3.png" style="width:800px;margin:40px 0">
