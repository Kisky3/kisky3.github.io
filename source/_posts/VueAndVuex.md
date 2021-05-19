---
title: Managing State in Vue with Vuex
date: 2021-01-05 21:24:50
tags:
 - Vue
 - Vuex
clearReading: true
autoThumbnailImage: yes
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
如何使用Vuex管理Vue的状态
<!--more-->
Vue和Vuex的连接方法想要总结一下:
一般来说Vue和Vuex都存在一下基础文件,

首先是store

stores/index.js
```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const state = {
  familyName: '',
  givenName: '',
}

const mutations = {
  setFamilyName (state, val) {
    state.familyName = val
  },
  setGivenName (state, val) {
    state.givenName = val
  },
}

const getters = {
  fullName: (state) => {
    return `${state.familyName}${state.givenName}`
  }
}

const actions = {}

export default new Vuex.Store({
  state,
  mutations,
  getters,
  actions,
})
```

***
## 如何从Vue向Vuex传值
#### 方法1.使用Vue.js的监视属性(watched propery)
如果使用watch的话，想通过别的值来改变目标值的时候，就不太灵活了。所以其实不是很推荐。

template
```js
<input type="text" name="familyName" v-model="familyName">
```

app.js
```js
import Vue from 'vue'
import Vuex from 'vuex'

import store from './stores'

new Vue({
  el: "#app",
  state: {
    familyName: '',
    givenName: '',
  },
  watch: {
    familyName (val) {
     // 直接通过这里向store里保存获取的新值
      store.commit('setFamilyName', val)
    },
  },
})
```

### 方法2: 通过事件传递值，并在Vue的方法里commit
首先还是一样的template
```js
<input type="text" name="familyName" @input="setFamilyName">
```

app.js
```js
import Vue from 'vue'
import Vuex from 'vuex'

import store from './stores'

new Vue({
  el: "#app",
  state: {},
  watch: {},
  methods: {
    setFamilyName (e) {
      store.commit('setFamilyName', e.target.value)
    },
  },
})
```

***
#### 方法4: 通过事件传递，通过store的action进行commit
首先是Vue的template, 这里给方法指定参数的话有多种方法。
```js
<input type="text" name="familyName" @input="updateFamilyName">
<!-- or -->
<input type="text" name="familyName" @input="updateFamilyName($event.target.value)">
```

app.js
```js
import Vue from 'vue'
import Vuex from 'vuex'
import { mapActions } from 'vuex'

import store from './stores'

new Vue({
  el: "#app",
  store, // mapActionsを使う場合は、Vueにstoreを追加する必要がある。
  state: {},
  watch: {},
  methods: {
    ...mapActions([
      'updateFamilyName',
    ]),
    // 不使用mapActions的情况下
    // Vue里不指定参数的情况下
    // setFamilyName (event) {
    //   store.commit('setFamilyName', event.target.value)
    // },
    // Vue里指定参数为$event.target.value的情况下
    // setFamilyName (value) {
    //   store.commit('setFamilyName', value)
    // }
    // 不使用mapActions,通过actions途径进行保存的情况下
    // updateFamilyName (e) {
    //   store.dispatch('updateFamilyName', e)
    // }
  },
})
```

stores/index.js
```js
const actions = {
  updateFamilyName({ commit, state }, e) {
    // vue里不指定参数的情况下，将event作为第二参数引入
    commit('setFamilyName', e.target.value)
  },
  // 或者
  // setFamilyName({ commit, state }, value) {
  //   commit('setFamilyName', value)
  // },
}
```
***
#### 方法4：使用get和set进行双向绑定
[参考Vue公式](https://vuex.vuejs.org/ja/guide/forms.html#%E5%8F%8C%E6%96%B9%E5%90%91%E7%AE%97%E5%87%BA%E3%83%97%E3%83%AD%E3%83%91%E3%83%86%E3%82%A3)

vue里使用v-model并在Vue.js的computed属性里定义set。
最好顺便定义好get,如果没有定义好get的情况下,mapState有可能会无法正常工作。
不想使用watched property进行监视并对store里的值进行更新的情况下v-model就显得很方便了。

首先还是template
```js
<input type="text" name="familyName" v-model="familyName">
```

app.js
```js
import Vue from 'vue'
import Vuex from 'vuex'

import store from './stores'

new Vue({
  el: "#app",
  state: {},
  computed: {
    familyName: {
      get () { return store.state.familyName },
      set (val) { store.commit('setFamilyName', val) },
    },
  },
})
```
***
### 究竟那种方法更好？
在vue的官方公式网站上说state应该是由action通过mutaion来进行改变。
而不是在Vue的methods里进行store.commit。
但是在Vue的初级入门的例子里其实是存在通过methods来commit的。
所以按情况来说,直接通过action来改变state的状态也不是不可以/

在非同期的处理的时候, 可以直接通过action进行commit,
但是只是对store的情况下，可以考虑直接从Vue里commit。

***
## 如何从Vuex向Vue传值
### 方法1: 活用computed
app.js
```js
import Vue from 'vue'
import Vuex from 'vuex'

import store from './stores'

new Vue({
  el: "#app",
  ・・・（略）・・・
  computed: {
    familyName() {
      return store.state.familyName
    },
    givenName() {
      return store.state.givenName
    },
    // 想引用不同的方法名的时候
    firstName () {
      return store.state.givenName
    },
    fullName () {
      return store.getters.fullName
    },
  }
  ・・・（略）・・・
}
```
***
### 方法2: 使用mapState和mapGetters
通过数组的形式定义好store的key
app.js
```js
import { mapState, mapGetters } from 'vuex'
import store from './stores'

new Vue({
  el: "#app",
  store, // 使用mapState和mapGetters的时候、要记得向Vue里追加store的声明。
  ・・・（略）・・・
  computed: {
    ...mapState([
      'familyName',
      'givenName',
    ]),
    // 想引用不同的方法名的时候
    ...mapState({
      firstName: 'givenName'
    }),
    ...mapGetters([
      'fullName'
    ]),
  }
  ・・・（略）・・・
}
```
***
### 方法3: 在store里使用其他的getters
比如`familyName`和`givenName`里不存在半角空格和不正确的字符的情况下显示kana,
首先在getters里定义好判断`familyName`和`givenName`是否为valid的方法，
两个条件同时满足的时候显示kana。

getters方法里如果第二参数是getters的话可以直接调用其他的getters里的方法。
stores/index.js
```js
const getters = {
  ・・・（略）・・・
  isValidFamilyName: (state) => {
    return String(state.familyName).trim() !== ''
  },
  isValidGivenName: (state) => {
    return String(state.givenName).trim() !== ''
  },
  isVisibleKana: (state, getters) => {
    return getters.isValidFamilyName && getters.isValidGivenName
  },
  ・・・（略）・・・
}
```
***
### 最终效果
<img src="./1.gif" style="width=60px" />

familyName、givenName用的是方法4追加进store
familyNameKana、givenNameKana是用方法3。

index.html
```js
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>vuejs-vuex-simple-sample</title>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```

main.js
```js
import Vue from 'vue'
import App from './App'

Vue.config.productionTip = false

new Vue({
  el: '#app',
  components: {
    App,
  },
  template: '<App/>',
})
```

App.vue
```html
<template>
  <div id="app">
    <div>
      名前：<input type="text" name="familyName" v-model="familyName">
      <input type="text" name="givenName" v-model="givenName">
    </div>
    <div>フルネーム：{{ fullName }}</div>
    <div>firstName：{{ firstName }}</div>

    <div v-show="isVisibleKana">
      ナマエ：<input type="text" name="familyNameKana" @input="updateFamilyNameKana">
      <input type="text" name="givenNameKana" @input="updateGivenNameKana">
    </div>
    <div v-show="isVisibleKana">フルネーム（カナ）：{{ fullNameKana }}</div>
  </div>
</template>

<script>
import Vue from 'vue'
import Vuex from 'vuex'
import { mapState, mapGetters, mapActions } from 'vuex'

import store from './stores'

Vue.use(Vuex)

export default {
  name: 'App',
  store,
  computed: {
    familyName: {
      get () { return store.state.familyName },
      set (val) { store.commit('setFamilyName', val) },
    },
    givenName: {
      get () { return store.state.givenName },
      set (val) { store.commit('setGivenName', val) },
    },
    ...mapState({
      firstName: 'givenName',
    }),
    ...mapGetters([
      'fullName',
      'isVisibleKana',
      'fullNameKana',
    ]),
  },
  methods: {
    ...mapActions([
      'updateFamilyNameKana',
      'updateGivenNameKana',
    ]),
  },
}
</script>
```

stores/index.js
```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const state = {
  familyName: '',
  givenName: '',
  familyNameKana: '',
  givenNameKana: '',
}

const mutations = {
  setFamilyName (state, val) {
    state.familyName = val
  },
  setGivenName (state, val) {
    state.givenName = val
  },
  setFamilyNameKana (state, val) {
    state.familyNameKana = val
  },
  setGivenNameKana (state, val) {
    state.givenNameKana = val
  },
}

const getters = {
  fullName: (state) => {
    return `${state.familyName}${state.givenName}`
  },
  isValidFamilyName: (state) => {
    return String(state.familyName).trim() !== ''
  },
  isValidGivenName: (state) => {
    return String(state.givenName).trim() !== ''
  },
  isVisibleKana: (state, getters) => {
    return getters.isValidFamilyName && getters.isValidGivenName
  },
  fullNameKana: (state) => {
    return `${state.familyNameKana}${state.givenNameKana}`
  },
}

const actions = {
  updateFamilyNameKana ({ commit, state }, e) {
    commit('setFamilyNameKana', e.target.value)
  },
  updateGivenNameKana ({ commit, state }, e) {
    commit('setGivenNameKana', e.target.value)
  },
}

export default new Vuex.Store({
  state,
  mutations,
  getters,
  actions,
})
```
