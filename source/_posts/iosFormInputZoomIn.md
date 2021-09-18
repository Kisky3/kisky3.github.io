---
title: iOS Input Zoom Issue
date: 2021-06-14 22:50:53
tags:
- Input
- iOS
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
iOS下的输入框点击放大问题
<!--more-->
在iOS的表单输入的时候，经常会出现页面被强制扩大的问题。
例子：
```html
<form>
  <div class="form-value">
    <input type="text" name="text">
  </div>
  <div class="form-value">
    <select name="select">
      <option value="">吉田優子</option>
      <option value="">千代田桃</option>
      <option value="">リリス</option>
      <option value="">陽夏木ミカン</option>
    </select>
  </div>
</form>
```

```css
input[type="text"] {
  box-sizing: border-box;
  width: 100%;
  font-size: 12px;
}
select {
  box-sizing: border-box;
  width: 100%;
  font-size: 12px;
}
```

如果在表单里输入的话，就像下面这样，页面被扩大。
<img src="./1.png" style="width:500px">

页面被扩大的原因是因为文字size没有达到16px的时候就会自动放大。

### 解决方法1
将所有未满16px的文字size设置成16px;虽然在页面上看起来会大一点但是这个是最简单的解决方法了。
```css
input[type="text"] {
  box-sizing: border-box;
  width: 100%;
  font-size: 16px;
}
select {
  box-sizing: border-box;
  width: 100%;
  font-size: 16px;
}
```
***
### 方法2:
在将文字size设置成16px的同时，使用`transform的scale()`来使其看起来相对小。sccale()的值可以利用calc()来计算得到。
这个方法不仅要调整字体的大小，还要相应的调整位置和相对大小，有一点点麻烦。
```css
input[type="text"] {
  box-sizing: border-box;
  width: 100%;
  font-size: 16px;
  transform: scale(calc(12 / 16));
}
select {
  box-sizing: border-box;
  width: 100%;
  font-size: 16px;
  transform: scale(calc(12 / 16));
}
```
***
### 方法3:
viewport里设置`user-scalable=no`,user-scalable=no本来是使得用户无法自己扩大页面，以此来控制iOS的自动扩大问题。

本来是不推荐限制用户的操作的，但是在iOS10以后，`user-scalable=no`将不再生效啦。

也就是说可以满足用户在输入表单的时候不自动扩大，但是用户手动扩大是可行的。

```
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no" />
```

Android的话只写`user-scalable=no`是不起效的。要加入下面的js.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<script>
var ua = navigator.userAgent.toLowerCase();
var isiOS = (ua.indexOf('iphone') > -1) || (ua.indexOf('ipad') > -1);
if(isiOS) {
  var viewport = document.querySelector('meta[name="viewport"]');
  if(viewport) {
    var viewportContent = viewport.getAttribute('content');
    viewport.setAttribute('content', viewportContent + ', user-scalable=no');
  }
}
</script>
```

### 参考
[iOSでinputのフォーカス時に画面がズームするのを防ぐ – Qiita](https://qiita.com/skwbr/items/b285cc312587c73a4812)

[iOS10のSafariでuser-scalable=no が効かなくズームがされる問題への対策 – Qiita](https://qiita.com/GreenDolphin/items/d74e5758a36478fbc039)
