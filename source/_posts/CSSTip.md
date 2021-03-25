---
title: CSS TIPS
date: 2019-01-11 23:24:59
tags:
- CSS
clearReading: true
coverCaption: "Hello World, Hello Programming"
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
***
{% alert info no-icon %}
#### 8. div内显示两行，超出部分用省略号显示
{% endalert %}
```CSS
.break-word {
word-wrap:break-word;
display: -webkit-box;
-webkit-line-clamp: 2;
-webkit-box-orient: vertical;
}
```
***
{% alert info no-icon %}
#### 9. 常用的box-shadow
{% endalert %}
```CSS
.box-shadow {
box-shadow: rgba(0,0,0,.2) 0 1px 5px 0px;
}
```
***
{% alert info no-icon %}
#### 10. Footer在页面最下固定
{% endalert %}
```html
<body>
  <div id="wrapper">
    <main>
    </main>

    <footer>
    </footer>
  </div>
</body>
```

```CSS
body,
#wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

footer {
  margin-top: auto;
}
```
***
{% alert info no-icon %}
#### 11. CSS渐变色
{% endalert %}
<img src="./1.png" style="width:400px "/>

图1:
```CSS
background: linear-gradient(#58a, #fb3);
```


图2:
```CSS
background: linear-gradient(45deg, #58a, #fb3);
```


图3:
```CSS
background: linear-gradient(to right, #58a 20%, #fb3 60%);
```
***
{% alert info no-icon %}
#### 12. CSS3设置Border内边框 / 外边框
{% endalert %}
内边框：

```css
box-sizing: border-box;

```

外边框：
```css
box-sizing: content-box;
```

***
{% alert info no-icon %}
#### 12. select box自定义
{% endalert %}
```html
<div class="select-wrap">
    <i class="iconfont icon-pulldown" />
    <select :value="value" v-on="listeners">
      <slot />
    </select>
  </div>
```

```css
.select-wrap {
  margin: 0;
  position: relative;
  &:hover {
    opacity: 0.8;
  }
  .iconfont {
    border-top: 4px solid #878787;
    border-right: 4px solid transparent;
    border-bottom: 4px solid transparent;
    border-left: 4px solid transparent;
  }
  .icon-pulldown {
    position: absolute;
    right: 11px;
    top: 57%;
    z-index: 2;
    color: #999;
    transform: translate(0, -50%);
  }
  select {
    width: 100%;
    border: solid 1px #d2d2d2;
    height: 28px;
    min-width: 208px;
    padding: 3px 5px;
    font-size: 13px;
    background: #fff;
    outline: none;
    cursor: pointer;
    &:disabled {
      background-color: #f5f5f5;
    }
    &:focus {
      border: 1px solid #606060;
    }
    &::-ms-expand {
      display: none;
    }
  }
}
@media only screen and (max-width: 480px) {
  .select-wrap {
    width: 100%;
    min-width: 170px;
    select {
      height: 40px;
      min-width: 170px;
      font-size: 16px;
    }
  }
}
```
***
{% alert info no-icon %}
#### 13. radio自定义
{% endalert %}
```html
<input
  type="radio"
  :checked="itemPrice.bring.flg === 'bring_priceless'"
  :id="'bring_free' + index"
  value="bring_priceless"
  :name="'bring_flg' + index"
  @change="createItemPrices"
  class="radio-input"
  :class="{
    'radio-error':
    bringBuyActive &&
    priceErrorObj.priceErrors[index].bring.free_error,
  }"
/>
 <label :for="'bring_free' + index">無料で買取</label>/>
```

```css
.radio-input {
    display: none;
  }
  .radio-input + label {
    padding-left: 20px;
    position: relative;
    margin-left: 20px;
    cursor: pointer;
    width: auto;
  }
  .radio-input + label::before {
    content: "";
    display: block;
    position: absolute;
    top: 2px;
    left: 0;
    width: 12px;
    height: 12px;
    border: 1px solid #999;
    transform: scale(1.5);
    border-radius: 50%;
  }

  .radio-input.radio-error + label::before {
    border: 1px solid red;
    transform: scale(1.5);
  }

  .radio-input:checked + label::before {
    border: 1px solid #0275ff;
    transform: scale(1.5);
  }

  .radio-input:checked + label::after {
    content: "";
    display: block;
    position: absolute;
    top: 4px;
    left: 2px;
    background: #0275ff;
    border-radius: 50%;
    width: 8px;
    height: 8px;
    transform: scale(1.5);
  }

  .radio-input.radio-error + label::before {
    border: 1px solid red;
    transform: scale(1.5);
  }

  .input_error {
    border: solid 2px red;
  }

  .input_text_error {
    margin: 3px 0;
    color: red;
    margin-left: 125px;
  }

  .disabled {
    background: #d3d3d3 !important;
  }
```

