---
title: CSS TIPS
date: 2019-01-11 23:24:59
tags:
- CSS
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverImage: cover.jpg
coverSize: partial
comments: false
categories: Front-end Knowledge
top: true
---
About common css tips. Continuously update here.
<!--more-->
{% alert info no-icon %}
#### 1. CSS关闭按钮
{% endalert %}

```CSS
.c-modal-close_button::before {
   content: "\00D7";
}

.c-modal-close_button {
   position: absolute;
   top: 8px;
   right: 15px;
   font-size: 25px;
   color: var(--color-navy-e9);
}
```

***
{% alert info no-icon %}
#### 2. [Input]伪类实现输入框active时改变背景颜色
{% endalert %}
```CSS
input[name="input"]:focus{
    background:var(--color-gray-f8);
}
```
***
{% alert info no-icon %}
#### 3. [Input]去掉Input自带淡蓝色边框
{% endalert %}
```CSS
input[type="text"],
input[type="password"],
textarea,
select {
    outline: none;
}
```
***
{% alert info no-icon %}
#### 4. CSS伪元素实现三角形
{% endalert %}
```CSS
.triangle{
    width: 200px;
    height: 100px;
    border-radius: 5px;
    border: 2px solid #000;
    position: relative;
 }
 .triangle:after{
    content: "";
    position: absolute;
    left: 200px;
    top:12px;
    width: 0;
    height: 0;
    border-top: 10px solid transparent;
    border-left: 10px solid #fff;
    border-right: 10px solid transparent;
    border-bottom: 10px solid transparent;
 }
 .triangle:before{
    content: "";
    position: absolute;
    left: 200px;
    top:10px;
    width: 0;
    height: 0;
    border-top: 12px solid transparent;
    border-left: 12px solid #000;
    border-right: 12px solid transparent;
    border-bottom: 12px solid transparent;
  }
```
***
{% alert info no-icon %}
#### 5. CSS上下跳动的动画效果
{% endalert %}
```CSS
.c-style-item:hover {
  background:var(--color-green-00);
  color:red;
  animation: shake 2s infinite;
  opacity: 1;
}

@keyframes shake {
  0% {
    transform: translate(0px, 0px);
  }
  50% {
    transform: translate(0px, -10px);
  }
  100% {
    transform: translate(0px, 0px);
  }
}
```
***
{% alert info no-icon %}
#### 6.CSS不固定宽度模块屏幕居中
{% endalert %}
```CSS
.c-dialog {
     position: absolute;
     left: 50%;
     top: 50%;
     transform: translate(-50%,-50%);
     border: solid 1px #ccc;
     background: #ccc;
   }
```

***
{% alert info no-icon %}
#### 7. HTML使用pre的情况下，让长文字自动换行
{% endalert %}
```CSS
pre{
  white-space:pre-wrap;
  word-wrap:break-word;
}

/*Parent*/
.c-modal {
   ...
   word-wrap: break-word;
   white-space : normal
}
```
***
{% alert info no-icon %}
#### 8. 纯CSS3实现点击行展开
{% endalert %}
```HTML
  <label class="drop" for="_1">Collapse 1 </label>
  <input id="_1" type="checkbox">
  <div>This is content text 1</div>
  
  <label class="drop" for="_2">Collapse 1 </label>
  <input id="_2" type="checkbox">
  <div>This is content text 2</div>
  
  <label class="drop" for="_3">Collapse 1 </label>
  <input id="_3" type="checkbox">
  <div>This is content text 3</div>
```

```CSS
.drop {
  cursor: pointer;
  display: block;
  background: #090;
   }

.drop+input {
  display: none;
  /* hide the checkboxes */
}

.drop+input+div {
  display: none;
 }

.drop+input:checked+div {
  display: block;
 }
```
