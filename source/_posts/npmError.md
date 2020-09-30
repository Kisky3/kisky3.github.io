---
title: npm Error
date: 2020-02-08 16:14:59
tags:
- npm
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
 常见npm错误

<!--more-->
### npm install error

在npm install时候发生error的话可以检查以下：

**1.是否存在package.json,因为npm安装的时候是根据package.json获取相应包的。没有源文件的话就无法获取资源。**

***


**2.在有package.json的情况下有可能是重复多次安装npm，导致冲突。**
可以尝试以下步骤后再次安装。
```
// 删除原先安装好的node_modules
rm -rf node_modules

// 清楚缓存
npm cache clean --force

// 更新npm
npm update -g npm

// 卸载npm
npm uninstall npm -g

```

***


**3.当出现以下error的时候说明端口被占用,可以修改端口或者kill其他占用此端口的程序。**
```
> frontend@1.0.0 dev /Users/apple/Desktop/vue-s3-dropzone/frontend
> node build/dev-server.js

events.js:174
      throw er; // Unhandled 'error' event
      ^

Error: listen EADDRINUSE: address already in use :::8080
    at Server.setupListenHandle [as _listen2] (net.js:1279:14)
    at listenInCluster (net.js:1327:12)
    at Server.listen (net.js:1414:7)
    at Function.listen (/Users/apple/Desktop/vue-s3-dropzone/frontend/node_modules/express/lib/application.js:617:24)
    at Object.<anonymous> (/Users/apple/Desktop/vue-s3-dropzone/frontend/build/dev-server.js:60:22)
    at Module._compile (internal/modules/cjs/loader.js:776:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:787:10)
    at Module.load (internal/modules/cjs/loader.js:653:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:593:12)
    at Function.Module._load (internal/modules/cjs/loader.js:585:3)
    at Function.Module.runMain (internal/modules/cjs/loader.js:829:12)
    at startup (internal/bootstrap/node.js:283:19)
    at bootstrapNodeJSCore (internal/bootstrap/node.js:622:3)
Emitted 'error' event at:
    at emitErrorNT (net.js:1306:8)
    at process._tickCallback (internal/process/next_tick.js:63:19)
    at Function.Module.runMain (internal/modules/cjs/loader.js:832:11)
    at startup (internal/bootstrap/node.js:283:19)
    at bootstrapNodeJSCore (internal/bootstrap/node.js:622:3)
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! frontend@1.0.0 dev: `node build/dev-server.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the frontend@1.0.0 dev script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/apple/.npm/_logs/2020-02-08T07_13_59_039Z-debug.log
```

***

**4.有可能在安装时候包丢失，可以修复一下**
Error:
```
return (new fsevents(path)).on('fsevent', callback).start(); ^

TypeError: fsevents is not a constructor at createFSEventsInstance (/Users/nannam/test-app-2/node_modules/chokidar/lib/fsevents-handler.js:28:11) at setFSEventsListener (/Users/nannam/test-app-2/node_modules/chokidar/lib/fsevents-handler.js:82:16) at FSWatcher.FsEventsHandler._watchWithFsEvents (/Users/nannam/test-app-2/node_modules/chokidar/lib/fsevents-handler.js:252:16) at FSWatcher. (/Users/nannam/test-app-2/node_modules/chokidar/lib/fsevents-handler.js:386:25) at LOOP (fs.js:1565:14) at process._tickCallback (internal/process/next_tick.js:61:11) error Command failed with exit code 1. info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```

解决方法：
```
npm audit fix --force
```
