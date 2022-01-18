---
title: Basic Features Of Vue.js
date: 2021-05-11 19:26:19
tags:
- Vue
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
关于Vue的基础概念总结
<!--more-->
# Vue的生命周期
## 什么是Vue的生命周期
在Vue中实例从创建到销毁的过程就是生命周期，即指从创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、卸载等一系列过程。

Vue一共有8个生命周期：
|  生命周期名  |  描述  |
| ---- | ---- |
|  beforeCreate  |  组件实例被创建之初  |
|  created  |  组件实例已经完全创建  |
|  beforeMount  |  组件挂载之前  |
|  mounted  |  组件挂载到实例上去之后  |
|  beforeUpdate  |  组件数据发生变化，更新之前  |
|  updated  |  数据数据更新之后  |
|  beforeDestroy  |  组件实例销毁之前  |
|  destroyed  |  组件实例销毁之后  |

## Vue页面组件加载会执行哪些生命周期
Vue提供了一些生命周期的钩子，平时最主要用的是下面几个,这几个是页面进行加载的时候会被执行的生命周期。
- beforecreate
- created
- beforemounte
- mounted

beforecreate是属于第一个被加载的部分,没有节点也没有数据。

created在beforecreate之后执行,这个时候已经取得了数据但是还没有生成节点。

beforemounte的时候也是取得了数据但是还没有生成节点。

mounted的时候是既取得了数据,这个时候页面上也已经生成了节点。

## 生命周期的使用方法
created => 发送请求取得数据
mounted => 使用插件(dom相关的逻辑处理)

一般来说会在create的里面进行发送请求的处理。
因为此时已经生成了数据($data),但是还没生成节点($el),所以在dom节点生成之前从发送的请求里获取数据是比较合理的顺序。

而如果我们在mounted里进行发送请求的处理的话,会导致一个加载顺序的混乱。(虽然它也是可以跑的)。这就变成了先生成数据和空节点,再返回去取得数据再反应到节点的顺序。

有可能会造成一个页面卡顿的状态。

那我们一般在什么情况下使用mounted呢。比如说我们需要使用插件,并且要对dom节点进行一些操作的时候。

比如我们要使用swiper插件来生成一个轮播图,这个时候就应该在mounted里进行new swiper()的处理。

# Vue路由
## 隐形路由和显性路由
```js
const router = new VueRouter({
  routes // `routes: routes` 缩略
})

// 4. 生成root的对象并且为了让vue全体能够使用路由你需要在根目录进行router的注入
const app = new Vue({
  router
}).$mount('#app')
```

显示路由： query
隐形路由： params(需要注明路由的Name)


显示路由：
```js
this.$router.push({
    path: '/about',
    query: {
        key: "test-ket"
    }
})
```

隐形路由：
```js
this.$router.push({
    path: '/about',
    // 路由的名字
    name: 'About',
    params: {
        key: "test-ket"
    }
})
```

## 路由模式
vue中的router有两种模式：hash模式（默认）、history模式（需配置mode: 'history'）

hash(默认):    http://localhost:8080/#/about
history: http://localhost:8080/about

hash模式是默认的,使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

它的特点在于：hash 虽然出现在 URL 中，但不会被包括在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。

history模式：利用了 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法。（需要特定浏览器支持）
这两个方法应用于浏览器的历史记录栈，在当前已有的 back()、forward()、go() 方法的基础之上，这两个方法提供了对历史记录进行修改的功能。当这两个方法执行修改时，只能改变当前地址栏的 URL，但浏览器不会向后端发送请求，也不会触发popstate事件的执行。

传统的路由指的是：当用户访问一个url时，对应的服务器会接收这个请求，然后解析url中的路径，从而执行对应的处理逻辑。这样就完成了一次路由分发

而前端路由是不涉及服务器的，是前端利用hash或者HTML5的history API来实现的，一般用于不同内容的展示和切换。

## 路由拦截
比如如下的路由拦截,可以在路由跳转过程中进行控制。最后不要忘记写下保安执行代码next()。

参数from,to,
next()是必须的,也可以在next里写入路由。


```js
全局:
router.beforeEach((to, from, next)=>{})
router.afterEach((to, from, next)=>{})

组件内守卫:
beforeRouterEnter((to, from, next)=>{})
beforeRouterUpdate((to, from, next)=>{})
beforeRouterLeave((to, from, next)=>{})

路由独享:
beforeEnter((to, from, next) => {})
```

## 动态路由和子路由
动态路由一般用于id之类的,并可以使用从路由获取到的id来向后台发送数据。

## Keep-alive
Keep-alive是在优化的时候使用的。可以进行组件的缓存。使用keep-alive之后会在组件执行的时候触发两个额外的生命周期：
- actived
- deactived

在路由里设置keep-alive的话可以对是否之前访问过该组件进行判断,如果存在缓存则直接显示缓存,不存在的时候就重新向服务器发送请求。

也可以在需要缓存的组件外面使用<keep-alive>来进行包裹,达到组件数据的缓存效果。

※ 详细的例子可以参照这篇文章：
https://qiita.com/Y-Kanoh/items/a720c5c45929d232b634

https://jp.vuejs.org/v2/api/#keep-alive

## ref
在Vue2.0的时候ref可以用于连接子组件的状态。
例子：在母组件点击按钮的时候取得子组件input输入的值。

子组件：
```vue
<template>
    <div>
        <input type="test" v-model="text" />
        <textarea v-model="textArea"></textarea>
    </div>
</template>
<script>
export default({
    data : function (){
        return {
            text: '',
            textArea: '',
        }
    }
})
</script>
```

母组件：
```vue
<template>
  <div id="app">
    <text-and-text-area ref="texts" />
    <button @click="testAction">test</button>
  </div>
</template>

<script>
import TextAndTextArea from './components/TextAndTextArea.vue'

export default {
  name: 'App',
  components: {
    TextAndTextArea
  },
  mounted: function (){
    console.log(this.$refs.texts);
  },
  methods: {
    testAction: function () {
      console.log(this.$refs.texts.text); // 子组件input输入值的参照
      console.log(this.$refs.texts.textArea); // 母组件input输入值的参照
    }
  }
}
</script>
```
在这里母组件在引用子组件的时候要给子组件加上ref`ref="texts"`,然后可以在母组件mounted的时候呼出`this.$refs.texts`,就能得到子组件里data定义好的数据对象。`this.$refs.texts.text`,`this.$refs.texts.textArea`。


vue3的时候写法会有一点不一样：
和上面同样的例子，

先来看看子组件:
```vue
<template>
    <div>
        <input type="test" v-model="text" />
        <textarea v-model="textArea"></textarea>
    </div>
</template>
<script>
import { ref } from 'vue'
export default({
  setup () {
    const text = ref('')
    const textArea = ref('')

    return {
      text,
      textArea
    }
  }
```

母组件：
```vue
<template>
  <div id="app">
    <text-and-text-area ref="texts" />
    <button @click="testAction">test</button>
  </div>
</template>

<script>
import TextAndTextArea from './TextAndTextArea.vue'
import { onMounted, ref } from 'vue'

export default {
  components: {
    TextAndTextArea
  },
  setup () {
    const texts = ref(null)
    const testAction = () => {
      console.log(texts.value.text);
      console.log(texts.value.textArea);
    }

    onMounted(() => {
      console.log(texts.value);
    })

    return {
      texts,
      testAction
    }
  }
}
</script>
```

Vue3的话写法发生了一些变化,首先就是不再使用Vue2里的`this.$refs`了。
取而代之要使用`setup`这个新属性。

分析一下就是首先要给子组件加上ref定义。`ref="texts"`,然后在母组件的setup里定义`const texts = ref(null)`。
这里的变量名称要和给子组件定义的名称相同,要不会报错。然后将结果return出来。
```js
return {
  texts, // 設定したrefにアクセスできる変数
  testAction
}
```
然后将方法method写在setup里面,就可以像这样`texts.value.textArea`使用子组件的值了。

## nextTick
我们知道Vue的DOM更新是不同步的,所以有时候会取得DOM更新前的数据。而nextTick就是能够保证当DOM加载完毕才执行内部代码。

例：
```js
Vue.createApp({
  data: function() {
    return {
      message: 'Welcome to Vue.js!'
    }
  },
  mounted: function() {
    this.message = 'Hello Vue.js!';
    // コンソールにログを出力します。
    console.log(this.$el.textContent);
    this.$nextTick(function() {
      // nextTickを使用してコンソールにログを出力します。
      console.log(this.$el.textContent);
    });
  }
}).mount('#app')
```
在这里我们可以看到console的输出结果如下：
```
Welcome to Vue.js!
Hello Vue.js!
```

所以可以看到在mounted里如果直接输出`this.$el.textContent`是得到DOM改变前的数据,而nextTick保证了取得DOM改变之后的数据。

使用场景比如我们要使用better-scroll插件的话就需要在DOM更新后取得数据来完成。

## axios二次封装
axios二次封装是为了方便简洁和后续使用。
要不每次请求都要写重复的代码来保证数据取得成功。

使用场景比如说数据加载的时候显示加载中,数据加载完毕则取消加载中的显示。
比如验证token存在与否,如果不存在的情况下要强制返回login页面进行登陆。

## Vuex
Vuex是一个vue的状态管理器,属性有：`state,getters,mutation,action,modules`.

- Vuex要在什么情况下进行使用呢？
比如说管理数据方便： 比如说同一个数据要在不同的组件中使用的时候就适合使用Vuex。
- 为什么要用modules呢?
就是当state数据比较多比较庞大不好管理的时候,也不知道哪个属性在哪个地方使用的时候就用modules进行管理。这样比较方便后期维护。

- mutaions和action有什么异同?
mutaions是同步的,actions是异步的。
action是提交mutation的,不会直接修改状态。
action可以包含异步操作。

- Vuex的state能否直接修改？
不能,因为state是实时更新的,而mutation又无法进行异步操作,所以如果你直接修改state进行异步操作的时候,还没有执行完,而别的地方如果同时对state进行了修改就会导致冲突。
所以state要同步操作,并且mutation被限制了不允许异步操作。

## computed,methods,watch的区别
computed: 有缓存,当值发生变化的时候才会执行。
methods: 没有缓存,template改变的时候methods都会执行。
watch: 是监听数据的,当值发生变化才会执行。
