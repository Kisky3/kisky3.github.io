---
title: AWS Amplify with Vue.js
date: 2020-04-21 23:43:14
tags:
- AWS
- Amplify
- Amplify Vue
clearReading: true
thumbnailImage: 20200421.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
AWS Amplify 框架和Vue.js
<!--more-->
### Amplify框架是什么
amplify 是aws几个月前开发的一款支持web和mobile 的无服务框架，具备身份验证，分析，API，GraphQL, 数据库，推送，机器学习等很多强大功能，可以理解为google firebase的加强版。

***
### Vue
```
$ npm install vue
$ vue create amplify-sample
```
Vue Project
```
Vue CLI v3.11.0
? Please pick a preset: 
  default (babel, eslint) 
❯ Manually select features 
```

```
? Please pick a preset: Manually select features
? Check the features needed for your project: 
 ◉ Babel
 ◯ TypeScript
 ◯ Progressive Web App (PWA) Support
 ◉ Router
❯◉ Vuex
 ◯ CSS Pre-processors
 ◉ Linter / Formatter
 ◯ Unit Testing
 ◯ E2E Testing
```

### Vue启动
```
$ cd amplify-sample/
$ npm run serve
```

可以在localhost:8080看到Vue的主页面了。
<img src="./1.png" style="width: 500px">

***
### 导入Amplify
使用npm导入Amplify
```
$ npm i aws-amplify
$ npm i aws-amplify-vue
```

设置amplify所使用的AWS
```
$ amplify configure
```
sign in你的AWS之后回到命令行界面，按下enter
```
Specify the AWS Region
? region:  us-east-1
```

选择你所在区域，并且设置好你的用户名
<img src="./2.png" style="width: 500px">

<img src="./3.png" style="width: 500px">

首先先将就着选择权限最强的AdministratorAccess吧，有需要的话之后再改也可以。
AccessKey和SecretKey要记好。
```
$ amplify init

Note: It is recommended to run this command from the root of your app directory
? Enter a name for the project amplify-sample
? Enter a name for the environment dev
? Choose your default editor: Visual Studio Code
? Choose the type of app that you're building javascript
Please tell us about your project
? What javascript framework are you using vue
? Source Directory Path:  src
? Distribution Directory Path: dist
? Build Command:  npm run-script build
? Start Command: npm run-script serve
```
中途可能会出现让你选择环境的选项`? Enter a name for the environment`
可以先输入dev
```
? Do you want to use an AWS profile? (Y/n) Y
```
***
### 追加认证
给Project追加认证
```
$ amplify add auth
```

```
Using service: Cognito, provided by: awscloudformation

 The current configured provider is Amazon Cognito. 

 Do you want to use the default authentication and security configuration? (Use arrow keys)
❯ Default configuration 
  Default configuration with Social Provider (Federation) 
  Manual configuration 
  I want to learn more. 
  ```
这次首先可以选择最基本的Default configuration，如果social provider的话，还可以使用Facebook, Google, Amazon等进行登陆。

```
 How do you want users to be able to sign in? 
  Username 
❯ Email 
  Phone Number 
  Email and Phone Number 
  I want to learn more. 
```
使用Email进行登陆。以后的设定都有默认配置就好。

如果只是这样的话实际的认证还是做不了的。
需要把本地的变更反映到服务器上。
```
$ amplify push
```

```
✔ Successfully pulled backend environment dev from the cloud.

Current Environment: dev

| Category | Resource name         | Operation | Provider plugin   |
| -------- | --------------------- | --------- | ----------------- |
| Auth     | amplifysample******** | Create    | awscloudformation |
? Are you sure you want to continue? (Y/n) 
```

看第一行的话，首先pull到远程环境，然后检测出差别。
就像git一样！利用这个功能，其他的末端也好，从AWS的命令行也能通过pull，
来获取和远程一摸一样的环境。
(比如说当本地环境坏掉之后，可以用这个挽回！)

这次追加了新的Auth，应该能反应到服务器上。

这个上传可能会花一点时间，趁反映到服务器的间隙，来编辑一下main.js
main.js
```
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

import Amplify, * as AmplifyModules from 'aws-amplify'
import { AmplifyPlugin } from 'aws-amplify-vue'
import awsconfig from './aws-exports'
Amplify.configure(awsconfig)

Vue.use(AmplifyPlugin, AmplifyModules)

Vue.config.productionTip = false

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```

然后接着编辑 router/index.js
router/index.js
```
import Vue from 'vue'
import Router from 'vue-router'
import Home from '../views/Home.vue'
import Login from '../views/Login.vue'

import store from '../store/index.js'

Vue.use(Router)


// Amplify読み込み
import { components, AmplifyEventBus } from 'aws-amplify-vue'
import Amplify, * as AmplifyModules from 'aws-amplify'
import { AmplifyPlugin } from 'aws-amplify-vue'

Vue.use(Router)
Vue.use(AmplifyPlugin, AmplifyModules)

let user;

// ユーザー管理
getUser().then((user) => {
    if (user) {
        router.push({path: '/'});
    }
});

function getUser() {
    return Vue.prototype.$Amplify.Auth.currentAuthenticatedUser().then((data) => {
        if (data && data.signInUserSession) {
            store.commit('setUser', data);
            return data;
        }
    }).catch(() => {
        store.commit('setUser', null);
        return null;
    });
}

// ログイン状態管理
AmplifyEventBus.$on('authState', async (state) => {
    if (state === 'signedOut'){
        user = null;
        store.commit('setUser', null);
        router.push({path: '/login'});
    } else if (state === 'signedIn') {
        user = await getUser();
        router.push({path: '/'});
    }
});

// ルーティング設定
const router = new Router({
    mode: 'history',
    routes: [
        {
            // ログインページ
            path: '/login',
            name: 'login',
            component: Login
        },
        {
            // トップページ
            path: '/',
            name: 'home',
            component: Home,
            meta: { requiresAuth: true}
        }
    ]
});

// リダイレクト設定
router.beforeResolve(async (to, from, next) => {
    if (to.matched.some(record => record.meta.requiresAuth)) {
        user = await getUser();
        if (!user) {
            return next({
                path: '/login'
            });
        }
        return next()
    }
    return next()
});

export default router
```
***