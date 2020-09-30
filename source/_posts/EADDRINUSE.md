---
title: node.js Error:Listen EADDRINUSE
date: 2019-12-21 23:07:45
tags:
- node.js
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
node.js启动错误: Error: listen EADDRINUSE
<!--more-->

在启动node app.js的时候出现了下面的错误
```
events.js:72
        throw er; // Unhandled 'error' event
              ^
Error: listen EADDRINUSE
    at errnoException (net.js:904:11)
    at Server._listen2 (net.js:1042:14)
    at listen (net.js:1064:10)
    at Server.listen (net.js:1138:5)
    at Function.app.listen (/vagrant/girly/girly-batch/node_modules/express/lib/application.js:532:24)
    at Object.<anonymous> (/vagrant/girly/girly-batch/bin/www:7:18)
    at Module._compile (module.js:456:26)
    at Object.Module._extensions..js (module.js:474:10)
    at Module.load (module.js:356:32)
    at Function.Module._load (module.js:312:12)
```

原因是因为端口号被占用了。关闭占用该端口号的进程然后再次启动就好。
```
$ ps aux | grep node
vagrant  14761  0.0  2.0 704016 38520 pts/0    Sl+  01:22   0:01 node app.js
vagrant  15069  0.0  0.0 107456   908 pts/2    S+   01:59   0:00 grep --color=auto node

$ sudo kill -9 14761
$ node app.js
```
