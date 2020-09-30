---
title: Webpack Getting Started
date: 2019-09-14 18:28:10
tags:
- webpack
- getting started
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: System Setting
---

Webpack的环境设置
<!--more-->

2018年8月25日更新，目前 webpack 已经更新值 4.17.1！不用配置很复杂的config也能运行了!!
今天搭建webpack环境时顺便记录一下.

### 什么是webpack
>webpack is used to compile JavaScript modules. Once installed, you can interface with webpack either from its CLI or API. If you're still new to webpack, please read through the core concepts and this comparison to learn why you might use it over the other tools that are out in the community.

<br>
WebPack可以看做是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其转换和打包为合适的格式供浏览器使用。

***

### 为什么要使用webpack
很多人开发了各种优秀的 JavaScript 模块或组件，我们不想重复发明轮子，而是想直接利用别人的模块，就是类似 require 或 include 这样的机制，把别人的模块引入进来，这就是modules(模块化).
但是 JavaScript 又没有 类或包 这样的概念，那应该如何做呢？如何去引入别人的模块？引入之后保证各种依赖关系不出错？这就是 webpack 要解决的问题。
Webpack的处理速度更快更直接，能打包更多不同类型的文件。

[Webpack与其他打包工具的比较](https://webpack.js.org/comparison/)
[Webpack的核心原理](https://webpack.js.org/concepts/)

***

### Webpack的安装（版本4.40.1）
1.首先创建自己的文件夹，初始化npm，安装本地化webpack并安装webpack-cli(一个能在命令行运行webpack的工具)

```JS
mkdir webpack-demo
cd webpack-demo
npm init -y
npm install webpack --save-dev
npm install webpack-cli --save-dev
```

2.创建以下的文件结构以及内容

project:
```JS
  webpack-demo
  |- package.json
+ |- index.html
+ |- /src
+   |- index.js
```

src/index.js
```JS
function component() {
  const element = document.createElement('div');

  // Lodash, currently included via a script, is required for this line to work
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');

  return element;
}

document.body.appendChild(component());
```

imdex.html
```JS
<!doctype html>
<html>
  <head>
    <title>Getting Started</title>
    <script src='https://unpkg.com/lodash@4.16.6'></script>
  </head>
  <body>
    <script src='./src/index.js'></script>
  </body>
</html>
```

我们还想需要编辑package.json文件，使其我们的package私有化，并同时移除main入口，防止代码误公开。

package.json
```JS
{
    "name": "webpack-demo",
    "version": "1.0.0",
    "description": "",
+   "private": true,
-   "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "devDependencies": {
      "webpack": "^4.20.2",
      "webpack-cli": "^3.1.2"
    },
    "dependencies": {}
  }
```

这个例子与&lt;script&gt;标签有默认依存关系，我们的index.js文件是依存于lodash并且在运行前是包含在网页里的.
这是因为index.js并没有明确声明需要lodash，它只是假定默认了全局变量'_'的存在.
所以我们需要创建打包.

***

### 创建打包
在以上的步骤中我们微调整了文件树结构，将源代码从发布代码中分离开.
源代码就是我们可以直接编辑和修改的代码，发布代码就是通过压缩和最优化之后,在打包时最终输出的代码.它最终将会被加载到浏览器上.

project
```JS
  webpack-demo
  |- package.json
+ |- /dist
+   |- index.html
- |- index.html
  |- /src
    |- index.js
```

为了打包依存于index.js的lodash，我们需要安装本地包.
```JS
npm install --save lodash
```

然后引用lodash到我们的script里
src/index.js
```JS
+ import _ from 'lodash';
+
  function component() {
    const element = document.createElement('div');

-   // Lodash, currently included via a script, is required for this line to work
    element.innerHTML = _.join(['Hello', 'webpack'], ' ');

    return element;
  }

  document.body.appendChild(component());
```
现在，因为我们已经打包了script,我们需要更新index.html文件，删除原有的lodash&lt;script&gt;并import之后加入另一个&lt;script&gt;来加载打包.(替换/src文件)

***
dist/index.html
```JS
  <!doctype html>
  <html>
   <head>
     <title>Getting Started</title>
-    <script src="https://unpkg.com/lodash@4.16.6"></script>
   </head>
   <body>
-    <script src="./src/index.js"></script>
+    <script src="main.js"></script>
   </body>
  </html>
```
在这个设置中，index.js 显式要求引入的 lodash 必须存在，然后将它绑定为 _（没有全局作用域污染）。通过声明模块所需的依赖，webpack 能够利用这些信息去构建依赖图，然后使用图生成一个会以正确顺序执行的优化 bundle。

可以这样说，执行 npx webpack，会将我们的脚本 src/index.js 作为 入口起点，也会生成 dist/main.js 作为 输出。Node 8.2/npm 5.2.0 以上版本提供的 npx 命令，可以运行在开始安装的 webpack package 中的 webpack 二进制文件（即 ./node_modules/.bin/webpack）

***
```JS
npx webpack

...
Built at: 13/06/2018 11:52:07
  Asset      Size  Chunks             Chunk Names
main.js  70.4 KiB       0  [emitted]  main
...

WARNING in configuration(配置警告)
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/concepts/mode/('mode' 选项还未设置，webpack 会将其值回退至 'production'。将 'mode' 选项设置为 'development' 或 'production'，来启用对应环境的默认优化设置。)
```
在浏览器中打开 index.html，如果一切正常，你应该能看到以下文本：'Hello webpack'
