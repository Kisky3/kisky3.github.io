---
title: Vue+Vuetify Confirm and Alert Component
date: 2020-07-14 23:50:46
tags:
 - Vuex
clearReading: true
autoThumbnailImage: yes
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
Vue+Vuetify打造独自的消息确认组件
<!--more-->
 vue框架自己好像没有弹出框, 而vuetify有弹出框v-dialog, 没有确认框confirm, 虽然确认框本身就是弹出框, 但是弹出框的功能有个特点, 就是确定做一件事情, 而取消了就是做另一件事情, 类似一个Promise要做的事情。

今天主要就是要自定义一个确认框, 我们利用组件的思维, 先定义组件结构, 然后定义组件如何配合文档document工作, 最后声明和调用。按照这个思路, 我们分如下几步：

1.写一个confirm的自定义组件
 src/components/atoms/Confirm.vue
```js
<template>
  <div class="dialogwrapper" v-if="show">
    <div class="overlay"></div>
    <v-card class="dialog">
      <v-card-title>{{ title }}</v-card-title>
      <v-card-text> {{ message }} </v-card-text>
      <v-card-actions>
        <v-btn @click="ok">確認</v-btn>
        <v-btn v-if="option === 'confirm'" @click="cancel">キャンセル</v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>
<script>
export default {
  data() {
    return {
      promiseStatus: null,
      show: false,
    };
  },
  methods: {
    confirm() {
      this.show = true;
      return new Promise((resolve, reject) => {
        this.promiseStatus = { resolve, reject };
      });
    },
    ok() {
      this.show = false;
      this.promiseStatus && this.promiseStatus.resolve();
    },
    cancel() {
      this.show = false;
      this.promiseStatus && this.promiseStatus.reject();
    },
  },
};
</script>
<style>
.dialogwrapper {
  align-items: center;
  display: flex;
  height: 100%;
  justify-content: center;
  left: 0px;
  pointer-events: none;
  position: fixed;
  top: 0px;
  width: 100%;
  z-index: 6;
  transition: all 0.2s cubic-bezier(0.25, 0.8, 0.25, 1) 0s, z-index 1ms ease 0s;
  outline: none;
}
.dialog {
  overflow-y: auto;
  pointer-events: auto;
  width: 100%;
  z-index: inherit;
  box-shadow: rgba(0, 0, 0, 0.2) 0px 11px 15px -7px,
    rgba(0, 0, 0, 0.14) 0px 24px 38px 3px, rgba(0, 0, 0, 0.12) 0px 9px 46px 8px;
  border-radius: 4px;
  margin: 24px;
  transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1) 0s;
  max-width: 290px;
}
.overlay {
  align-items: center;
  border-radius: inherit;
  display: flex;
  justify-content: center;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  pointer-events: auto;
  background: #000;
  opacity: 0.46;
}
.v-card__actions {
  justify-content: space-evenly;
}
</style>
```

2、定义插件src/plugins/confirm.ts
src/plugins/confirm.js

```js
import Vue from "vue";
import Confirm from "../../src/components/atoms/Confirm.vue";
const VueComponent = Vue.extend(Confirm);
const vm = new VueComponent().$mount();

let init = false;

const confirm = function (options) {
  Object.assign(vm, options, {
    type: "confirm",
  });
  if (!init) {
    document.body.appendChild(vm.$el);
    init = true;
  }

  return (vm as any).confirm();
};

export default confirm;
```

3、全局声明组件src/main.js
```js
import Vue from 'vue'
import App from './App'
import router from './router'
import vuetify from '@/plugins/vuetify'
import confirm from '@/plugins/confirm'
Vue.config.productionTip = false
Vue.prototype.$confirm = confirm
/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  vuetify,
  components: { App },
  template: '<App/>'
})
```

4.调用src/components/TestPage.vue
```js
<template>
	<v-card>
		<v-btn @click="showConfirm">open</v-btn>
	</v-card>
</template>
<script>
	methods: {
		showConfirm(){
		  this.$confirm({
          title: "このサイトを離れますか？",
          message: "編集中のものは保存されませんが、よろしいですか",
          option: "confirm",
          //没有取消按钮的时候用option: "alert"
        })
        .then(() => {
          next();
        })
        .catch(() => {
          next(false);
        });
		}
	}
</script>
```

5.在src/shims-tsx.d.ts里定义类型
```js
type ConfirmOptions = {
  title: string;
  message: string;
  option: string;
};
declare module "vue/types/vue" {
  interface Vue {
    $confirm: (options: ConfirmOptions) => Promise;
  }
}
```

最终呈现的效果：
<img src="./1.png" style="width:500px">

以上是一个简单的示例, 我们在需要使用的地方直接通过this.$confirm().then().catch()就可以把逻辑全部设置好, 可以看出this.$confirm是一个Promise, 而这个Promise是通过定义组件结构的时候confirm方法返回的。如下所示：

```js
confirm() {
	let _this = this;
	this.show = true;
	return new Promise(function (resolve,reject){
		_this.promiseStatus = {resolve,reject};
	});
}
```

我们在src/plugins/confirm.js中通过了一些方法找到了组件挂载点, 组件元素, 组件中的方法。

这个组件不比vuetify自带的v-dialog一开始就是长在文档中的, 而是需要我们手动appendChild的方式将元素插入文档中。