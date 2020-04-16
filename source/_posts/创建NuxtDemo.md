---
title: Create Vue.js + SSR demo with Nuxt 
date: 2019-12-03 22:36:57
tags:
- Nuxt.js
- Vue
- SSR
- SPA
clearReading: true
thumbnailImage: 20191203.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

利用Nuxt创建一个Vue.js + SSR的Demo
<!--more-->
### 什么是Nuxt
Nuxt就是一个通用的Vue.js的框架。再直白点说，就是Vue.js原来是开发SPA（单页应用）的，但是随着技术的普及，很多人想用Vue开发多页应用，并在服务端完成渲染。这时候就出现了Nuxt.js这个框架，她简化了SSR的开发难度。还可以直接用命令把我们制作的vue项目生成为静态html。

***
### 什么是SSR和SPA
Nuxt.js最常用于的是SSR -- Server Side Rendering(服务端渲染)，也就是在服务器端上把我们的Vue文件渲染成html，然后再返回给浏览器。

Vue写出来的大都是SPA -- Single Page Application(单页应用)。
把所有东西加载完之后再在浏览器打开。但是对SEO搜索引擎不友好。

SSR优点：
- 更好的 SEO，由于搜索引擎爬虫抓取工具可以直接查看完全渲染的页面。
如果你的应用程序初始展示Loading菊花图，然后通过Ajax获取内容，抓取工具并不会等待异步完成后再行抓取页面内容。也就是说，如果SEO对你的站点至关重要，而你的页面又是异步获取内容，则你可能需要服务器端渲染(SSR)解决此问题。


- 更快的内容到达时间（首屏加载更快），因为服务端只需要返回渲染好的HTML，这部分代码量很小的，所以用户体验更好。

***
### 安装Nuxt并创建Hello World
1.安装vue-cli
```
install vue-cli -g
```

2.创建文件夹NuxtDemo

3.在文件夹下生成项目，并输入项目名称（尽量用小写），描述，作者。
```
vue init nuxt/starter
```

4.使用npm/yarn安装package.json里的dependencies里的包。
安装失败对时候可以删除node_modules之后再进行安装。
```
npm install / yarn install
```

5.启动Nuxt，修改Nuxt里的h1为Hello World
```
npm run dev / yarn run dev
```
<br>
<img src="./2.png" style="width:500px">
<br>
<img src="./1.png" style="width:500px">

***
接下来我们要了解一下目录结构及设定，并生成自己的页面。

### 生成的目录结构
<img src="./3.png" style="width:300px">

##### assets:
资源文件，放置不需要webpack打包处理的资源文件，比如scss,图片，字体文件等等。

##### components:
组件。可以复用的组件。存放项目中的各种组件。
※只有在这个目录下的文件才能被称为组件。

##### layouts：
创建自定义的页面布局。页面都需要有一个布局，默认为 default.vue。它规定了一个页面如何布局页面。 可以在这个目录下创建全局页面的统一布局，或是错误页布局。如果需要在布局中渲染 pages 目录中的路由页面，需要在布局文件中加上 <nuxt /> 标签。如果需要在普通页面中使用下级路由，则需要在页面中添加 <nuxt-child />。 该目录名为Nuxt.js保留的，不可更改。

##### middleware：
中间件。放置自定义的中间件，会在加载组件之前调用。可以在页面中调用： middleware: 'middlewareName'。

##### pages：
页面。一个 vue 文件即为一个页面。index.vue 为根页面。用于nuxt自动组织应用的路由及视图。Nuxt.js 框架读取该目录下所有的 .vue 文件并{% hl_text red %}自动{% endhl_text %}生成对应的路由配置。
  1. 若需要二级页面，则添加文件夹即可。
  2. 如果页面的名称类似于 _id.vue （以 _ 开头），则为动态路由页面，_ 后为匹配的变量（params）。
  3. 若变量是必须的，则在文件夹下建立空文件 index.vue。更多的配置请移步至 [官网](https://zh.nuxtjs.org/guide/routing/)。

##### plugins:
插件。用于组织那些需要在 根vue.js应用 实例化之前需要运行的 Javascript 插件。需要注意的是，在任何 Vue 组件的生命周期内， 只有 beforeCreate 和 created 这两个钩子方法会在 客户端和服务端均被调用。其他钩子方法仅在客户端被调用。 可以在这个目录中放置自定义插件，在根 Vue 对象实例化之前运行。例如，可以将项目中的埋点逻辑封装成一个插件，放置在这个目录中，并在 nuxt.config.js 中加载。

##### static:
静态文件。静态文件目录 static 用于存放应用的静态文件，此类文件不会被 Nuxt.js 调用 Webpack 进行构建编译处理。 放置不需要经过 webpack 打包的静态资源。如一些 js, css 库,图片,ico地址导航图标。服务器启动的时候，该目录下的文件会映射至应用的根路径 / 下。

##### store:
用于组织vuex状态管理。

##### nuxt.config.js:
nuxt.config.js 文件用于组织Nuxt.js 应用的个性化配置，以便覆盖默认配置。

***
### Nuxt.js 的渲染流程
Nuxt.js 通过一系列构建于 Vue.js 之上的方法进行服务端渲染，具体流程如下:

<img src="./4.png" style="width:400px">

<h6>1.调用 nuxtServerInit 方法</h6>

当请求打入时，最先调用的即是 nuxtServerInit 方法，可以通过这个方法预先将服务器的数据保存，如已登录的用户信息等。另外，这个方法中也可以执行异步操作，并等待数据解析后返回。

<h6>2.Middleware 层</h6>

经过第一步后，请求会进入 Middleware 层，在该层中有三步操作：

读取 nuxt.config.js 中全局 middleware 字段的配置，并调用相应的中间件方法 匹配并加载与请求相对应的 layout 调用 layout 和 page 的中间件方法

<h6>3.调用 validate 方法</h6>

在这一步可以对请求参数进行校验，或是对第一步中服务器下发的数据进行校验，如果校验失败，将抛出 404 页面。

<h6>4.调用 fetch 及 asyncData 方法</h6>

这两个方法都会在组件加载之前被调用，它们的职责各有不同， asyncData 用来异步的进行组件数据的初始化工作，而 fetch 方法偏重于异步获取数据后修改 Vuex 中的状态。

***
### 添加Page
Nuxt.js是使用了Vue-Router进行routing管理。当Vue.js制作SPA的时候也是使用了Vue-Router的。但是需要先写好component，然后在Vue-Router将URL和componeng进行配对设置。

Nuxt.js的话pages目录下已经放置了页面用的component。
page目录下如果放置了类似*.vue的文件的话，它就会自动进行routing的定义设定。

比如:
在page下新建一个index.vue文件和sample.vue文件。

/pages/index.vue
```html
<template>
    <div class="index_container">
       <h3>This is a test homepage</h3>
       <p>test home page content</p>
    </div>
</template>

<script>

</script>

<style scoped>
.index_container {
      padding: 45px;
      position: absolute;
      left: 5%;
      right: 5%;
      width: 90%;
      height: 100%;
      background: #f9f7e8;
  }
</style>>


```
/pages/sample.vue
```html
<template>
    <div class="sample_container">
       <h1>This is sample page</h1>
       <p>sample sample sample sample sample sample</p>
    </div>
</template>

<script>
</script>

<style>
  .sample_container {
      padding: 45px;
      position: absolute;
      left: 5%;
      right: 5%;
      width: 90%;
      height: 100%;
      background: #f9f7e8;
  }
</style>
```
打开 http://localhost:3000 和 http://localhost:3000/sample 就可以看到以下画面了。

<img src="./5.png" style="width:400px">
<br>
<img src="./6.png" style="width:400px">

***
### 添加Layout
在component下添加layout组件
.Footer.vue
```html
<template>
   <footer>footer</footer>
</template>

<style>
  footer {
      position:absolute;
      display: flex;
      justify-content: center;
      align-items: center;
      width:100%;
      height: 100px;
      background: lightblue;
      bottom:0;
  }
</style>
```

.Header.vue
```html
<template>
    <header>header</header>
</template>

<style>
  header {
    width: 100%;
    height: 100px;
    background: #ff8b8b;
    display:flex;
    justify-content: center;
    align-items: center;
    top: 0;
  }
</style>
```

.Sidebar.vue
```html
<template>
    <aside class="c-widget">
        <h1 class="c-widget__title">Sidebar Title</h1>
        <div class="c-widget__text">
            <p>This is a sidebar content</p>
            <p>This is a sidebar content</p>
            <p>This is a sidebar content</p>
            <p>This is a sidebar content</p>
            <p>This is a sidebar content</p>
            <p>This is a sidebar content</p>
            <p>This is a sidebar content</p>
            <p>This is a sidebar content</p>
            <p>This is a sidebar content</p>
            <p>This is a sidebar content</p>
        </div>
    </aside>
</template>

<style>
  aside {
      background: #61bfad;
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px;
  }
</style>
```

在layout下创建twocolumns.vue作为自定义layout,并import Footer 和 Sidebar两个组件。最后export name为twocolumns
```html
<template>
  <div>
      <div class="l-container">
        <div class="l-main">
            <nuxt/>
        </div>
        <div class="l-sidebar">
            <my-sidebar />
        </div>
    </div>
    <my-footer/>
  </div>
</template>

<script>
import MyFooter from "../components/Footer.vue"
import MySidebar from "../components/Sidebar.vue"
export default {
    name: "twocolumns",
    components: {
        MyFooter,
        MySidebar
    }
}
</script>

<style>
.l-container {
    overflow: hidden;
    max-width: 1080px;
    margin-left: auto;
    margin-right: auto;
}
.l-main {
    width: calc(100% - 340px);
    float: left;
}
.l-sidebar {
    width: 320px;
    float: right;
}
</style>
```
自定义的default.vue（所有页面默认引用的layout）里添加Header 和 Footer
```html
<template>
  <div>
    <my-header />
    <nuxt/>
    <my-footer />
  </div>
</template>

<style>
html {
  font-family: "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}
</style>
<script>
  import MyHeader from '../components//Header.vue';
  import MyFooter from '../components/Footer';

  export default {
    components: {
      MyHeader,
      MyFooter
    }
  }
</script>
```
在希望使用自定义的sample.vue里添加下面的代码来指定layout
```html
<script>
export default {
  layout: 'twocolumns'
}
</script>
```
可以看到在homepage是使用默认样式带着Header和Footer，但是sample页面使用来自定义layout，没有Header但是有sidebar。

top:
<img src="./7.png" style="width:400px">

<br>
sample:
<img src="./8.png" style="width:400px">

完成！
