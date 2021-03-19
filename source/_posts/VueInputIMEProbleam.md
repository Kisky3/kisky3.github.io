---
title: Vue iOS IME Input Probleam
date: 2020-08-14 23:16:05
tags:
- Vue
- Input
- iOS
- IME
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
Vue Input 在iOS下的IME日语输入问题
<!--more-->
## 1.重复值的输入
Vue的Input在网页输入时没啥问题 但是在iPhone的Sarari输入时会有输入值的重复输入
##### 理由
重复输入的情况是因为使用了@input,导致数字还在输入过程中事件被触发导致多重重复输入。
##### 解决方法
这个用@change,等到数字输入完毕之后再执行一次处理就可以了。

##2.日语输入时第一次输入时无法触发@change事件(input的type为number)

第一次输入日语的时候,default为null, 无论@change还是@input都无法将日语存储到state里。
如果需要监视输入值进行相关处理(Validation)之类的话,会出现bug。
但是奇怪的是一旦输入过一次数字之后,就不存在这个问题了。

##### 理由
如果你用日语输入法IME输入日文的时候,vue的data是当作null处理的,而如果default是null的话,
则相当于没变化,@change事件是不被触发的。
但是数字的话是填入数字,相当于data被激活了,@change之后就是能够运行的。


##### 解决方法
1.使用Element: compositionend Event
>compositionend イベントは、 IME などのテキスト編集システムが現在の編集セッションを完了またはキャンセルした時に発生します。

也就是当你使用IME输入法的时候,输入时或者输入完成的时候这个事件将被触发。
所以可以考虑和@change一起配套使用。

<img src="./1.gif" style="width: 300px"/>

```html
<input
  type="number"
  :value="item"
  @change="createItem"
  @compositionend="createItem"
  name="item"
/>
```

2.不要使用input[type="number"]

如果使用`number`的话键盘的上下键可以触发数字的大小变动,而且不能自动识别iPhone和Android的数字键盘。
可以考虑使用下面的方法,type依然为text,但是`inputmode="numeric"`.
```
<input type="text" inputmode="numeric" pattern="\d*">
```

这样不仅保证日语输入时候触发@change事件,并且能在使用iPhone和Android的时候自动打开数字键盘。
<img src="./2.jpg" style="width: 500px"/>
而且不需要同时使用@change和@compositionend两个事件,用户体验也是比较好的。
