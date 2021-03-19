---
title: Vue Route Redirect on Refresh
date: 2020-08-03 23:50:46
tags:
 - Vue
 - Router
clearReading: true
autoThumbnailImage: yes
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
Vue Router的页面刷新处理
<!--more-->
上一篇文章里我们写了如何利用Vue+Vuetify打造自定义的消息确认组件
[Vue+Vuetify打造独自的消息确认组件]('https://kisky3.github.io/2020/07/14/VueAlert/')

今天我们来利用打造好的组件来写一个常见的需求。
比如用户在输入数据时,刷新页面或者进行了页面跳转,为了防止数据丢失应该跳出弹窗提示问用户是否保存数据。

需求如下:(自定义消息组件已经可以使用)

1.从页面A跳转到页面B的时候进行页面B的自动刷新,以便获取最新的数据。
(包括浏览器上的返回和代码里a标签的返回)

2.从页面A跳转到页面B的时候,跳出自定义的弹窗,提示是否跳转页面。

3.在特定的编辑页面进行手动刷新(在浏览器上按刷新按钮)的时候,跳出系统自带弹窗提示是否刷新。

***
### 需求1
> 1.从页面A跳转到页面B的时候进行页面B的自动刷新,以便获取最新的数据。
(包括浏览器上的返回和代码里a标签的返回)

这个可以通过App.vue里对router的watch和页面跳转时动态路由匹配的判断来进行刷新。
刷新可以用`this.$router.go(0);` (当然也有别的写法)

※ 提醒一下,当使用路由参数时,例如从`foo`导航到`bar`,原来的组件实例会被复用。因为两个路由都渲染同个组件,比起销毁再创建,复用则显得更加高效。不过,这也意味着组件的生命周期钩子不会再被调用。

App.vue
```js
 watch: {
    $route(to, from) {
      if (from.name === "foo" && to.name === "bar") {
        // 在这里刷新
        this.$router.go(0);
      }
    },
  },
```
***
### 需求2
> 2.从页面A跳转到页面B的时候,跳出自定义的弹窗,提示是否跳转页面。
在A页面的component里定义`beforeRouteLeave`事件。

你可以在路由组件内直接定义以下路由导航守卫,比如`beforeRouteEnter`,`beforeRouteUpdate (2.2 新增)`,`beforeRouteLeave`,比如具体的说明如下：
```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

为了满足这个需求我们使用`beforeRouteLeave`来通过追加自己的条件和动态路由匹配来调出自定义弹窗。

实例:
```js
beforeRouteLeave(to, from, next) {
    const goback =
      to.name === "foo" &&
      from.name === "bar" &&
      !this.isConfirm;
    if (goback) {
      // 调用弹窗
        this.$confirm({
          title: "このサイトを離れますか？",
          message: "編集中のものは保存されませんが、よろしいですか",
          option: "confirm",
        })
        .then(() => {
          // 取消浏览器自定义弹窗
          this.destoryBeforeUnload();
          // 点击确认的时候进行页面跳转
          next();
        })
        .catch(() => {
          // 点击确认的时候不进行页面跳转
          next(false);
        });
    } else {
      next();
    }
  },
```
***
### 需求3
> 3.在特定的编辑页面进行手动刷新(在浏览器上按刷新按钮)的时候,跳出系统自带弹窗提示是否刷新。

我们可以考虑使用`beforeunload事件`:

> `Window: beforeunload event`:当浏览器窗口关闭或者刷新时, 会触发beforeunload事件。当前页面不会直接关闭, 可以点击确定按钮关闭或刷新, 也可以取消关闭或刷新。但是请注意, 并非所有浏览器都支持此方法, 而有些浏览器需要事件处理程序实现两个遗留方法中的一个作为代替：

> 将字符串分配给事件的returnValue属性
从事件处理程序返回一个字符串。

> 某些浏览器过去在确认对话框中显示返回的字符串, 从而使事件处理程序能够向用户显示自定义消息。但是, 此方法已被弃用, 并且在大多数浏览器中不再支持。


所以我们可以像下面这样给window添加或者移除弹窗:
在其他使用自定义弹出框的时候要记得将这个事件destory了(`destoryBeforeUnload`)
```js
// 在按浏览器上的刷新按钮自动监视并弹出弹框
window.addEventListener("beforeunload", this.handleBeforeUnload);

destoryBeforeUnload() {
  // 移除弹窗
  window.removeEventListener("beforeunload", this.handleBeforeUnload);
},

handleBeforeUnload(e) {
  e.preventDefault();
  const message =
  "このサイトを離れますか？\n行った変更が保存されない可能性があります。";
  e.returnValue = message;
  return message;
},
```

以上！
