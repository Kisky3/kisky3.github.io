---
title: iOS Input Input Expansion Probleam
date: 2020-09-14 23:16:05
tags:
- iOS
- Input
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
iOS下输入时画面的放大问题
<!--more-->
## 解决方法1: 禁止页面缩放
```html
<meta name="viewport" content="width=device-width,
initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
   <!-- initial-scale     - 初始的缩放比例
   minimum-scale     - 允许用户缩放到的最小比例
   maximum-scale     - 允许用户缩放到的最大比例
   user-scalable     - 用户是否可以手动缩放 -->
```

但是这样会限制用户的手动放大。

***
## 解决方法2: input输入框中的字体不能小于16px
例子：
```css
input[type=text] {
  font-size: 16px;
  transform: scale(0.8);
}
```
***
## 解决方法3: 调整行高
调整input框的行高与字体font-size的行高，一般是*input框的行高>=字体font-size的行高*

***
## 参考
[ios中点击input输入框光标变大，页面放大问题](https://blog.csdn.net/weixin_40890907/article/details/82378873?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control)

[iOSでinputのフォーカス時に画面がズームするのを防ぐ](https://qiita.com/skwbr/items/b285cc312587c73a4812)

