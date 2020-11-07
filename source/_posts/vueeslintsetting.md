---
title: Vue Eslint Setting
date: 2020-07-06 20:16:14
tags:
- eslintsrc
- autosave
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: System Setting
---
Vue的Eslint设定(自动修正)
<!--more-->
因为经常要设置所以做个memo：

|自动格式的对象文件|npm库(package.json)	|.eslintrc.js的extends|
| ---- | ---- | ---- |
|vue(template) |eslint-plugin-vue|plugin:vue/recommended|
|vue(script)|eslint|eslint:recommended|
|js|eslint|	eslint:recommended|

### 安装
package.json更新：
```js
 "scripts": {
    "lint": "eslint --ext .js,.vue src",
    "lint:fix": "eslint --fix --ext .js,.vue src"
  },
  "devDependencies": {
    "babel-core": "^6.26.3",
    "babel-eslint": "^10.0.1",
    "babel-helper-vue-jsx-merge-props": "^2.0.3",
    "babel-plugin-istanbul": "^5.1.0",
    "babel-plugin-syntax-jsx": "^6.18.0",
    "babel-plugin-transform-runtime": "^6.23.0",
    "babel-plugin-transform-vue-jsx": "^3.7.0",
    "eslint": "^5.12.1",
    "eslint-config-standard": "^12.0.0",
    "eslint-plugin-import": "^2.14.0",
    "eslint-plugin-node": "^8.0.1",
    "eslint-plugin-promise": "^4.0.1",
    "eslint-plugin-standard": "^4.0.0",
    "eslint-plugin-vue": "^5.1.0"
  },
```
更新后把package-lock.json和nodemodules删除后，重新安装
```
npm install
```

安装好后修改以下文件：

.eslintrc.js
```js
module.exports = {
  plugins: [
    "vue"
  ],
  env: {
    es6: true,
  },
  extends: [
    'eslint:recommended',
    "plugin:vue/essential",
  ],
  rules: {
    "arrow-body-style": "error",
    "arrow-parens": "error",
    "arrow-spacing": "error",
    "generator-star-spacing": "error",
    "no-duplicate-imports": "error",
    "no-useless-computed-key": "error",
    "no-useless-constructor": "error",
    "no-useless-rename": "error",
    "no-var": "error",
    "object-shorthand": "error",
    "prefer-const": "error",
    "prefer-rest-params": "error",
    "prefer-spread": "error",
    "prefer-template": "error",
    "rest-spread-spacing": "error",
    "template-curly-spacing": "error",
    "yield-star-spacing": "error"
  }
}
```

./vscode/settings.json
```js
{
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        {
          "language": "vue",
          "autoFix": true,
        },
      ],
    "eslint.autoFixOnSave": true,
}
```
