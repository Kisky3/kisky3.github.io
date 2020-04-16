---
title: About Gulp
date: 2019-12-08 22:17:02
tags:
- Gulp
clearReading: true
thumbnailImage: 20191209.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
关于Gulp
<!--more-->
### gulp是什么？
gulp是一个基于流的构建工具，可以自动执行指定的任务，简洁且高效。

***
### gulp能做什么
1.开发环境下，想要能够按模块组织代码，监听实时变化
2.css/js预编译，postcss等方案，浏览器前缀自动补全等
3.条件输出不同的网页，比如app页面和mobile页面
4.线上环境下，我想要合并、压缩 html/css/javascritp/图片，减少网络请求，同时降低网络负担

***
### 安装gulp
```
npm install -g gulp     //全局安装
npm install --save-dev gulp //安装到当前项目并在package.json中添加依赖
```
***
### 核心API
##### ・gulp.task task(name[, deps], fn)
task()方法用于定义任务，传入名字、依赖任务数组、函数即可，gulp会先执行任务数组，结束后调用定义的函数，可以通过此手段控制任务的执行顺利。

例子：要定义一个任务build来执行css、js、imgs这三个任务，我们可以通过指定一个任务数组而不是函数来完成。

```
gulp.task('build', ['css', 'js', 'imgs']);
```
***
##### ・gulp.src src(globs[, options])
src()方法输入一个glob或者glob数组，然后返回一个可以传递给插件的数据流

Gulp使用node-glob来从你指定的glob里面获取文件：

- app.js 精确匹配
- *.js 能匹配js后缀的文件
- **/*.js 能匹配多级目录下的js文件（也包含当前目录下）
- !js/app.js 精确排除

例子：js目录下包含了压缩和未压缩的js文件，我们想要压缩还没有被压缩的文件
```
gulp.src(['js/**/*.js', '!js/**/*.min.js'])
```
***
##### ・gulp.watch watch(globs[, opts], cb) or watch(globs[, opts], tasks)
watch()方法可以监听文件，它接受一个glob或者glob数组以及一个任务数组来执行回调
// 当templates目录下的模板文件发生变化，自动执行编译任务
```
gulp.task('watch', function (event) {
  gulp.watch('templates/*.tmpl.html', ['artTemplate']);
  console.log('Event type: ' + event.type); // added, changed, or deleted   
  console.log('Event path: ' + event.path); // The path of the modified file
});
```
***
### 配置文件 
1.package.json文件。它就是记录了我们使用了什么插件，以及版本号的记录的一个json文件而已。
```js
"author": "",
  "license": "ISC",
  "devDependencies": {
    "gulp": "^3.9.1",
    "gulp-autoprefixer": "^3.1.0",
    "gulp-concat": "^2.6.0",
    "gulp-imagemin": "^3.0.1",
    "gulp-jshint": "^2.0.1",
    "gulp-less": "^3.1.0", //例如这是处理LESS文件的，将LESS编译成.css文件
    "gulp-minify-css": "^1.2.4",
    "gulp-notify": "^2.2.0",
    "gulp-rename": "^1.2.2",
    "gulp-uglify": "^1.5.4",
    "gulp-watch": "^4.3.8"
    ...
```

2.gulpfile.js文件
它就是告诉了gulp我们要将什么文件编译到什么文件下的XXX目录里面。例如在我的src目录里面存在一个css文件夹，里面装了很多css或者LESS等样式文件，我现在想通过gulp将它编译到dist目录下面的css文件夹里面并且这个css文件夹里的样式文件还是压缩过了。那么gulpfile.js就是起到了这样的一个作用。
```js
//下面我将自己的gulpfile.js文件的内容给你大家做做案例
var gulp=require('gulp'); //基础库
var less=require('gulp-less'); //编译less工具
var concat=require('gulp-concat'); //文件合并
var uglify=require('gulp-uglify');//压缩JS文件
//首先require的意思就是从你的电脑里面加载某一个gulp的插件，例如require('gulp-less')
//请注意：加载这些插件需要你的电脑已经安装了这些插件，详情请百度，很简单也就是CMD两行命令的事情而已。

//配置文件路径
var paths={
    src_html:"./src/**/*.html",  //src_变量开头的是源文件的文件目录
    src_less:"./src/css/**/*.less", //less文件
    src_css:"./src/css/**/*.css",  //css文件
    src_json:"./src/**/*.json",   //json格式的文件
    src_images:"src/images/**/*", 
    src_pic:"src/pic/**/*", //它的意思是src文件下pic文件里面的所有格式的文件
    src_js:"./src/js/**/*.js",
    src_text:"./src/**/*.text",

    dist:"./dist",    //dist_变量开头的都是编译过后的文件目录
    dist_css:"./dist/css",   //dist文件夹下的css文件
    dist_mincss:"./dist/mincss", //dist文件夹下的mincss文件
    dist_images:"dist/images",
    dist_html:"./dist/**/*.html",
    dist_pic:"dist/pic",
    dist_js:"./dist/js",
    dist_minjs:"./dist/minjs"
}
//style任务
gulp.task('styles',function(){ //gulp.task就是告诉gulp要编译什么东西，并且使用什么样的处理方式，比如我这边就是编译less文件
    gulp.src([paths.src_less])
    .pipe(less())
    
    //然后输出编译文件到指定文件夹
    .pipe(gulp.dest(paths.dist_css))
});


// HTML任务
gulp.task('html', function() {  //编译html文件
    return gulp.src([paths.src_html,paths.src_json,paths.src_text])
    .pipe(gulp.dest(paths.dist))//输出到指定文件夹
    //提醒任务完成
    .pipe(notify({ message: 'html,json,text is OK' }))
});
gulp.task('scripts',function(){  //编译js文件内容，诸如此类，你也可以创建其他的任务，比如images
    return gulp.src([paths.src_js])
    .pipe(gulp.dest(paths.dist_js)) //输出到指定文件夹
    .pipe(notify({ message: 'Scripts is OK' })) //提醒任务完成
});
```

监听文件变化
```js
//下面的这段代码是gulpfile.js最后的一点的代码，他的作用就是去监听哪一个文件发生了变化，然后好把它编译到dist的文件夹内
//监听文档
gulp.task('watch',function(){
    //监听less,sass,css
    gulp.watch([paths.src_less,paths.src_css],['styles', 'html'])
    //监听js
    gulp.watch([paths.src_js],['scripts'])
    //监听图片
    gulp.watch([paths.src_images],['images'])
    //监听hhtml,json,text
    gulp.watch([paths.src_html,paths.src_json,paths.src_text], function(event) {
      gulp.run('html')
  });
})
```
然后cmd中输入gulp watch就是监听的意思，然后就OK了。
现在你在src文件目录中修改了什么文件，就会编译到dist文件中，gulp有很多的好处，比如处理CSS预编译语言，压缩，或者将EC6编译成EC5都是很好的！
***
### 参考
[絶対つまずかないGulp 4入門（2019年版）
インストールとSassを使うまでの手順](https://ics.media/entry/3290/)
