---
title: Nuxt Memo
date: 2021-07-25 00:33:55
tags:
  - Nuxt
  - Netlify
clearReading: true
coverCaption: 'Hello World, Hello Programming'
coverSize: partial
comments: false
categories: Front-end Knowledge
---

Nuxt 学习笔记

<!--more-->

在这里记录一些 Nuxt 踩的坑。

### Nuxt + Netlify Deploy 404 Problem

当我想在 Netlift 上发布 Nuxt 写的代码的时候发现变成了 404。
这个时候首先要考虑设定 404 的跳转。

方法 1:
`/pages`里新建一个的`*vue`文件,然后再里面写上跳转的设定。比如跳转到根页面。
pages/\*.vue

```js
<script>
export default {
 asyncData({ redirect }) {
  return redirect('/')
 }
}
</script>
```

在这个状态下如果输入任意的 URL (例：https://localhost:3000/hoge),就会自动跳转到根页面。

方法 2:
在 Netlify 里设定跳转

static 的下面创建一个为`_redirects`(没有后缀名)的文件, 然后在里面写入下面的一行就可以了。
static/\_redirects

```js
/*   /index.html   200
```

之后发布就可以解决 404 问题啦〜！

---

### Nuxt Sass TypeError: this.getOptions is not a function

Nuxt 里使用 Sass 的时候, install node-sass 和 sass-loader 之后启动服务器,sass-loader 发生以下的错误。

```
ERROR  Failed to compile with 1 errors

ERROR  in ./components/Header.vue?vue&type=style&index=0&id=1a9bb128&lang=sass&scoped=true&

Module build failed (from ./node_modules/sass-loader/dist/cjs.js):

TypeError: this.getOptions is not a function
    at Object.loader (/var/www/front/node_modules/sass-loader/dist/index.js:25:24)
```

原因有两个

##### 1.sass-loader

sass-loader 版本在 11 之后,需要 webpack 的版本在 5 以上。
但是装有 Nuxt 的 webpack 的最新 Nuxt 版本(2021/06/23 的时候为 2.15.7),webpack@4.46.0, 所以版本不符合就会造成错误。

sass-loader v11.0.0 的发布信息里有下面的消息：

> minimum supported webpack version is 5

所以我们必须将 Nuxt 指定为 11 之前的版本。

##### 2.node-sass

另外也不要忘记 node-sass 的版本检查。
看 node-sass 的仓库就可得知不同版本的 node 支持对应不同版本的 node-sass。

##### 解决方法：

将`sass-loader@10.2.0`版本固定,然后查找对应的 node 的版本,再查找相应版本的 node-sass。

1.首先删除已经安装的东西

```
yarn remove node-sass sass-loader
```

2.确认 Node 版本

```
node -v
```

我的版本是 v14.15.5

3.根据上面的 node-sass 的仓库来查找对应的 node-sass
我的情况是 Node 14.15.5、node-sass 4.14 是最新版本、所以指定 4.14.1。

```
# 我的情况
yarn add -D sass-loader@10.2.0 node-sass@4.14.1

# 复制粘贴用
yarn add -D sass-loader@10.2.0 node-sass@
```

这样的话就可以正常启动服务器了！使用 Sass 的时候要注意版本信息！
