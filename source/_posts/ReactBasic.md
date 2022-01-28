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

## Ⅲ-组件与组件化
### ① 组件
理解:用来实现局部功能效果的代码和资源的集合(html/css/js/img等等)
为什么要用组件:一个界面的功能复杂
作用:复用编码,简化项目编码,提高运行效率

### ② 组件化
当应用是以多组件的方式实现,这个应用就是组件化的应用

# 四、React面向组件编程
1.使用React开发者工具调试
React Developer Tools

2.注意
a) 组件名必须是首字母大写

b) 虚拟DOM元素只能有一个根元素

c) 虚拟DOM元素必须有结束标签 < />

3.渲染类组件标签的基本流程
a) React内部会创建组件实例对象

b) 调用render()得到虚拟DOM,并解析为真实DOM

c) 插入到指定的页面元素内部

## 1、组件三大属性1:state
### ① 理解
state是组件对象最重要的属性,值是对象(可以包含多个key:value的组合)
组件被称为状态机,通过更新组件的state来更新对应的页面显示(重新渲染组件)

### ② 强烈注意
组件中的render方法中的this为组件实例对象

组件自定义方法中this为undefined,如何解决?

a) 强制绑定this:通过函数对象的bind()

b) 箭头函数推荐

状态数据,不能直接修改或者更新

### ③代码示例
#### Ⅰ-正常的用函数对象的bind()
```js
    //1.创建组件
		class Weather extends React.Component{
			//构造器调用几次？ ———— 1次
			constructor(props){
				console.log('constructor');
				super(props)
				//初始化状态
				this.state = {isHot:false,wind:'微风'}
				//解决changeWeather中this指向问题,也可以在调用出直接使用
				this.changeWeather = this.changeWeather.bind(this)
			}
			//render调用几次？ ———— 1+n次 1是初始化的那次 n是状态更新的次数
			render(){
				console.log('render');
				//读取状态
				const {isHot,wind} = this.state
				return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}，{wind}</h1>
			}
			//changeWeather调用几次？ ———— 点几次调几次
			changeWeather(){
				//changeWeather放在哪里？ ———— Weather的原型对象上，供实例使用
				//由于changeWeather是作为onClick的回调，所以不是通过实例调用的，是直接调用
				//类中的方法默认开启了局部的严格模式，所以changeWeather中的this为undefined
				console.log('changeWeather');
				//获取原来的isHot值
				const isHot = this.state.isHot
				//严重注意：状态必须通过setState进行更新,且更新是一种合并，不是替换。
				this.setState({isHot:!isHot})
				console.log(this);

				//严重注意：状态(state)不可直接更改，下面这行就是直接更改！！！
				//this.state.isHot = !isHot //这是错误的写法
			}
		}
		//2.渲染组件到页面
		ReactDOM.render(<Weather/>,document.getElementById('test'))

```

#### Ⅱ-简写方式:赋值语句的形式+箭头函数
```js
	    //1.创建组件
		class Weather extends React.Component{
			//初始化状态
			state = {isHot:false,wind:'微风'}
			render(){
				const {isHot,wind} = this.state
				return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}，{wind}</h1>
			}
			//自定义方法————要用赋值语句的形式+箭头函数
			changeWeather = ()=>{
				const isHot = this.state.isHot
				this.setState({isHot:!isHot})
			}
		}
		//2.渲染组件到页面
		ReactDOM.render(<Weather/>,document.getElementById('test'))
```
## 2、组件三大属性2:props
### ①理解
每个组件对象都会有props(properties的简写)属性
组件标签的所有属性都保存在props中

### ② 作用
通过标签属性从组件外向组件内传递变化的数据
注意:组件内部不要修改props数据

### ③代码示例:
#### Ⅰ-类组件使用props
```js
	//创建组件
		class Person extends React.Component{
			render(){
				// console.log(this);
				const {name,age,sex} = this.props
				return (
					<ul>
						<li>姓名：{name}</li>
						<li>性别：{sex}</li>
						<li>年龄：{age+1}</li>
					</ul>
				)
			}
		}
		//渲染组件到页面
		ReactDOM.render(<Person name="jerry" age={19}  sex="男"/>,document.getElementById('test1'))
		ReactDOM.render(<Person name="tom" age={18} sex="女"/>,document.getElementById('test2'))
		const p = {name:'老刘',age:18,sex:'女'}
		// console.log('@',...p);
		// ReactDOM.render(<Person name={p.name} age={p.age} sex={p.sex}/>,document.getElementById('test3'))
        //此处使用赋值解构方式,使得代码更简洁
		ReactDOM.render(<Person {...p}/>,document.getElementById('test3'))

```
#### Ⅱ-函数组件使用props
```js
//创建组件
		function Person (props){
			const {name,age,sex} = props
			return (
					<ul>
						<li>姓名：{name}</li>
						<li>性别：{sex}</li>
						<li>年龄：{age}</li>
					</ul>
				)
		}
//此处限制可以换成typrScript
		Person.propTypes = {
			name:PropTypes.string.isRequired, //限制name必传，且为字符串
			sex:PropTypes.string,//限制sex为字符串
			age:PropTypes.number,//限制age为数值
		}

		//指定默认标签属性值
		Person.defaultProps = {
			sex:'男',//sex默认值为男
			age:18 //age默认值为18
		}
		//渲染组件到页面
		ReactDOM.render(<Person name="jerry"/>,document.getElementById('test1'))
```

## 3、组件三大属性3:refs
### ① 理解
组件内的标签可以定义ref来标识自己

### ② 代码示例:
#### 1、字符串形式的ref(不推荐,将被淘汰)
```js
//展示左侧输入框的数据
	showData = ()=>{
		const {input1} = this.refs
		alert(input1.value)
	}
	//展示右侧输入框的数据
	showData2 = ()=>{
		const {input2} = this.refs
		alert(input2.value)
	}
	render(){
		return(
			<div>
				<input ref="input1" type="text" placeholder="点击按钮提示数据"/>&nbsp;
				<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
				<input ref="input2" onBlur={this.showData2} type="text" placeholder="失去焦点提示数据"/>
			</div>
		)
	}
}
```

#### 2、回调形式的ref
```js
/**下面的this指的是组件实例,我直接this.input1 = c 意思是给实例上的input1赋值,之后直接通过调用打印得到*/
//展示左侧输入框的数据
	showData = ()=>{
		const {input1} = this
		alert(input1.value)
	}
	//展示右侧输入框的数据
	showData2 = ()=>{
		const {input2} = this
		alert(input2.value)
	}
	render(){
		return(
			<div>
				<input ref={c => this.input1 = c } type="text" placeholder="点击按钮提示数据"/>&nbsp;
				<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
				<input onBlur={this.showData2} ref={c => this.input2 = c } type="text" placeholder="失去焦点提示数据"/>&nbsp;
			</div>
		)
	}
}
```

#### 3、createRef创建ref容器最推荐的
```js
/*React.createRef调用后可以返回一个容器，该容器可以存储被ref所标识的节点,该容器是“专人专用”的*/
myRef = React.createRef()
myRef2 = React.createRef()
//展示左侧输入框的数据
showData = ()=>{
	alert(this.myRef.current.value);
}
//展示右侧输入框的数据
showData2 = ()=>{
	alert(this.myRef2.current.value);
}
render(){
	return(
		<div>
			<input ref={this.myRef} type="text" placeholder="点击按钮提示数据"/>&nbsp;
			<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
			<input onBlur={this.showData2} ref={this.myRef2} type="text" placeholder="失去焦点提示数据"/>&nbsp;
		</div>
	)
}
}	
```

## 4、事件处理与收集表单数据
### Ⅰ-事件处理
1. 通过onXxx属性指定事件处理函数(注意大小写)
a)React使用的是自定义(合成事件,而不是使用的原生DOM事件) ----为了更好的兼容性

b)React中的事件是通过事件委托的方式处理的(委托给组件最外层的元素)----为了更高效

2.通过event.target得到发生事件的DOM元素对象 -----不要过度使用ref

### Ⅱ-表单组件的分类
就形式上来说，受控组件就是为某个form表单组件添加value属性；非受控组件就是没有添加value属性的组件。

#### 1、受控组件
```js
state = {//初始化状态
	username:'', //用户名
	password:'' //密码
}

//保存用户名到状态中
saveUsername = (event)=>{
	this.setState({username:event.target.value})
}

//保存密码到状态中
savePassword = (event)=>{
	this.setState({password:event.target.value})
}

//表单提交的回调
handleSubmit = (event)=>{
	event.preventDefault() //阻止表单提交
	const {username,password} = this.state
	alert(`你输入的用户名是：${username},你输入的密码是：${password}`)
}

render(){
	return(
		<form onSubmit={this.handleSubmit}>
			用户名：<input onChange={this.saveUsername} type="text" name="username"/>
			密码：<input onChange={this.savePassword} type="password" name="password"/>
			<button>登录</button>
		</form>
	)
}
}
```

#### 2、非受控组件
```js
handleSubmit = (event)=>{
		event.preventDefault() //阻止表单提交
		const {username,password} = this
		alert(`你输入的用户名是：${username.value},你输入的密码是：${password.value}`)
	}
	render(){
		return(
			<form onSubmit={this.handleSubmit}>
				用户名：<input ref={c => this.username = c} type="text" name="username"/>
				密码：<input ref={c => this.password = c} type="password" name="password"/>
				<button>登录</button>
			</form>
		)
	}
}

```

## 5、高阶函数与函数柯里化
### ① 高阶函数:
如果一个函数符合下面两个规范中的任何一个,那该函数就是高阶函数

- 若A函数,接受的参数是一个函数,那么A就可以称之为高阶函数
- 若A函数,调用的返回值依然是一个函数,那么A就可以称之为高阶函数

常见的高阶函数有:Promise、setTimeout、arr.map()等等

### ② 函数的柯里化
通过函数调用继续返回函数的方式,实现对此接受参数最后统一处理的函数编码形式
```js
function sum(a){ return (b)=>{return c=>{ return a+b+c} }}
```

### ③ 不用函数柯里化实现事件的绑定
直接使用回调函数,因为他本身就是以一个函数为返回值
```js
<input onChange={event => this.saveFormData('username',event) } type="text" name="username"/>
```

## 五、生命周期
