---
layout: post
title: Setting Vuex Namespace
date: 2020-01-30 16:23:41
tags:
 - Vue
 - form
clearReading: true
thumbnailImage: 20200130.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
Vuex命名空间的设定
<!--more-->
Vuex 2.1开始Store的module划分就变得简单了，追加**namespace:true**就可以自动划分命名空间了。

追加了这个选项之后，**state**，**getters**，**actions**，**mutations**就会自动地被划分到和自己所属的module名（下面的例子里module名就是helloModule）同名的命名空间里。

modules/hello/index.js
```js
export default helloModule = {
  namespaced: true, // 追加
  state: {
    hello: false,
  },
  getters: {
    // 方法名
    hello(state) {
      return state.hello
    }
  },
  actions: {
    // 方法名
    sayHello(context, payload) {
      api.sayHello()
        .then((data) => {})
        .catch((err) => {})
    }
  },
  mutations: {
    // 方法名
    hello(state, payload) {
      state.hello = true
    }
  }
}
```
呼出的时候「mapState/mapActions参数传递命名空间」的形式也可以。
还可以使用明确指明命名空间的形式。

### 引用方：使用参数传递命名空间
```js
computed: {
  ...mapState('helloModule', // ← 参数传递命名空间
    // 在这里state并不是根state、而是被自动识别为helloModule/state的命名空间
    {  
      hello: state => state.hello
    }
  }
},
methods: {
  ...mapActions('helloModule', //← 参数传递命名空间
    // this.sayHello() 是 this.$store.dispatch('helloModule/sayHello')的映射
    ['sayHello']
  )
}
```

***
### 明确指定命名空间
```js
computed: {
  ...mapState({
    hello: state => state.helloModule.hello // 这个state是全局store。
  }
},
methods: {
  ...mapActions([
    // this.sayHello() 是this.$store.dispatch('helloModule/sayHello')的映射
    'helloModule/sayHello'
  ])
}
```
