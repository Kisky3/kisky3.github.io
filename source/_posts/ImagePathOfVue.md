---
title: Setting Image Psth by :src="require(Var)"
date: 2020-04-12 18:42:16
tags:
 - Vue
 - :src
 - image path
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
使用:src="require(Var)"指定图像路径
<!--more-->
最近在写vue时被卡了一下。
# 遇到的问题
Vue的component里想引用图像文件。
首先把图像文件放在了assets里，并如下引用文件
```js
src="require('@/assets/hoge.jpg')"
```
但是奇怪的是并取不到文件，并且显示`Cannot find module '@/assets/hoge.jpg`的错误。

- hoge.vue

```js
<template lang="pug">
  v-container(grid-list-lg fluid pa-0)
    v-layout(row wrap)
      template(v-for="(pr, index) in prContents")
        v-flex(xs12 md4 :key="index")
          v-card
            v-img(:src="require(pr.image)" height="300")
            v-container(fill-height fluid pa-2)
              v-layout(fill-height)
                v-flex(xs12 align-end flexbox)
                  span(class="headline white--text" v-text="pr.title")
</template>

<script>
export default {
  name: 'HomeConcept',
  data () {
    return {
      prContent: [
        {
          title: 'タイトル1',
          contents: 'コンテンツ',
          image: '@/assets/hoge1.jpg'
        },
       {
          title: 'タイトル2',
          contents: 'コンテンツ',
          image: '@/assets/hoge2.jpg'
        },
       {
          title: 'タイトル3',
          contents: 'コンテンツ',
          image: '@/assets/hoge3.jpg'
        },
      ]
    }
  }
}
</script>

<style scoped  lang="stylus">

</style>
```
***
# 解决方法
require内，如果单纯使用文字列的话，@是无法识别的，所以require()要在data对象里实行。
```js
data() {
  return {
    prContents: [
      {
        title: "タイトル1",
        contents: "コンテンツ",
        image: require("@/images/hoge.jpg")
      }
    ]
  }
}
```

修改后的hoge.vue
```js
<template lang="pug">
  v-container(grid-list-lg fluid pa-0)
    v-layout(row wrap)
      template(v-for="(pr, index) in prContents")
        v-flex(xs12 md4 :key="index")
          v-card
            v-img(:src="pr.image" height="300")
            v-container(fill-height fluid pa-2)
              v-layout(fill-height)
                v-flex(xs12 align-end flexbox)
                  span(class="headline white--text" v-text="pr.title")
</template>

<script>
export default {
  name: 'HomeConcept',
  data () {
    return {
      prContents: [
        {
          title: 'タイトル1',
          contents: 'コンテンツ',
          image: require('@/assets/hoge1.jpg')
        },
        {
          title: 'タイトル2',
          contents: 'コンテンツ',
          image: require('@/assets/images/3pr-medical.jpg')
        },
        {
          title: 'タイトル3',
          contents: 'コンテンツ',
          image: require('@/assets/images/3pr-compassionate.jpg')
        }
      ]
    }
  }
}
</script>

<style scoped  lang="stylus">

</style>

```

如果只是一般的修改参数来引用图像的情况下，写成下面这样就可以了。
```js
<img
  :src="require('@/assets/img/' + domainName + '.png')"
  class="c-logo-takaku"
  alt=""
/>
...
data(): Data {
    return {
      domainName: 'xxxx'
    }
  },
```