---
title: React Memo 
date: 2022-01-18 23:35:11
tags:
- React
clearReading: true
coverCaption: 'Hello World, Hello Programming'
coverSize: partial
comments: false
categories: Front-end Knowledge
---
React 的小总结
<!--more-->
# 一、React简介
## 1、React的特点

- 声明式编程
- 组件化编程
- React Native编写原生应用
- React Native (简称RN)是Facebook于2015年4月开源的跨平台移动应用开发框架，是Facebook早先开源的JS框架 React 在原生移动应用平台的衍生产物
- 高效 (优秀的Diffing算法)

## 2、React高效的原因
- 使用虚拟(virtual)DOM,不总是直接操作页面真实DON
- DOM Diffing算法,最小化页面重绘
注意：React并不会提高渲染速度,反而可能会增加渲染时间,真正高效的原因是它能有效减少渲染次数
## 3、创建虚拟DOM的两种方式
Ⅰ- js创建虚拟DOM(不推荐)

```
//1.创建虚拟DOM,创建嵌套格式的doms
const VDOM=React.createElement('h1',{id:'title'},React.createElement('span',{},'hello,React'))
//2.渲染虚拟DOM到页面
ReactDOM.render(VDOM,docoment.getElementById('test'))
```

Ⅱ -jsx创建虚拟DOM
```
//1.创建虚拟DOM
	const VDOM = (  /* 此处一定不要写引号，因为不是字符串 */
    	<h1 id="title">
			<span>Hello,React</span>
		</h1>
	)
//2.渲染虚拟DOM到页面
	ReactDOM.render(VDOM,document.getElementById('test'))

//3.打印真实DOM与虚拟DOM,这一步不是jsx创建虚拟dom必须，我只是为了方便查阅
	const TDOM = document.getElementById('demo')
    console.log('虚拟DOM',VDOM);
	console.log('真实DOM',TDOM);
```
可以看到，上下两种方式，明显jsx的写法更符合我们的习惯,当出现多重嵌套时,js创建方法会使我们编程出现很大麻烦。但是jsx其实也只是帮我们做了一层编译,当我们写完jsx代码后,最终我们的代码也会被编译成js的书写方式

## 4、关于虚拟DOM
- 本质是Object类型的对象(一般对象)
- 虚拟DOM比较'轻',真实DOM比较'重',因为虚拟DOM是React内部在用,无需真实DOM上那么多的属性(只有React需要的属性)
- 虚拟DOM最终会被React转化为真实DOM,呈现在页面上。

***
# 二、jsx语法规则
JSX是一种JavaScript的语法扩展、是一种嵌入式的类似XML的语法,常应用于React架构中,但也不仅限于此.应该说JSX因React框架而流行,但也存在其他的实现.只要你够厉害,甚至能在单片机上实现(当然你要自己写出它的实现方式)。

```
1、规则
- 定义虚拟DOM时,不要写引号
- 标签中混入JS表达式时要用{}
- 样式的类名指定不要用class,要用className
- 内联样式,要用style={{key:value}}的形式(双{}代表对象,单{}代表表达式)去写
- 只有一个跟标签(整个虚拟DOM在外层有且仅有一个容器包裹)
- 标签必须闭合
- 标签首字母
- 若小写字母开头,则将该标签转为html中同名元素,若html中无该标签对应的同名元素,则报错
- 若大写字母开头,ract就去渲染对应组件,若组件没有定义,则报错
```

***
# 三、两种组件定义区别、组件与模块理解
## Ⅰ-react中定义组件
### ① 函数式声明组件 (function)
执行了ReactDOM.render(.......之后，发生了什么？

1.React解析组件标签，找到了MyComponent组件。
2.发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中。


### ②类式组件(下面的实例都是指类组件 class)
执行了ReactDOM.render(.......之后，发生了什么？

​- 1.React解析组件标签，找到了MyComponent组件。

​- 2.发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法。

​- 3.将render返回的虚拟DOM转为真实DOM，随后呈现在页面中。

### 组件中的render是放在哪里的？
MyComponent的原型对象上，供实例使用。
### 组件中的render中的this是谁？
MyComponent的实例对象 <=> MyComponent组件实例对象。

## Ⅱ-模块与模块化
### ① 模块
理解:向外提供特定功能的js程序,一般就是一个js文件
为什么要拆成模块:随着业务逻辑增加,代码越来越多且复杂
作用:复用js,简化js的编写,提高js运行效率
### ② 模块化
当应用的js都以模块来编写,这个应用就是一个模块化的应用
