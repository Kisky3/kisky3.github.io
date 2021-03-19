---
title: Vue Eslint Setting
date: 2020-07-06 20:16:14
tags:
  - eslintsrc
  - autosave
clearReading: true
coverCaption: 'Hello World, Hello Programming'
coverSize: partial
comments: false
categories: System Setting
---

Vue 的 Eslint 设定(自动修正)

<!--more-->

因为经常要设置所以做个 memo：

| 自动格式的对象文件 | npm 库(package.json) | .eslintrc.js 的 extends |
| ------------------ | -------------------- | ----------------------- |
| vue(template)      | eslint-plugin-vue    | plugin:vue/recommended  |
| vue(script)        | eslint               | eslint:recommended      |
| js                 | eslint               | eslint:recommended      |

### 安装

package.json 更新：

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

更新后把 package-lock.json 和 nodemodules 删除后，重新安装

```
npm install
```

安装好后修改以下文件：

.eslintrc.js

```js
module.exports = {
  plugins: ['vue'],
  env: {
    es6: true
  },
  extends: ['eslint:recommended', 'plugin:vue/essential'],
  rules: {
    'arrow-body-style': 'error',
    'arrow-parens': 'error',
    'arrow-spacing': 'error',
    'generator-star-spacing': 'error',
    'no-duplicate-imports': 'error',
    'no-useless-computed-key': 'error',
    'no-useless-constructor': 'error',
    'no-useless-rename': 'error',
    'no-var': 'error',
    'object-shorthand': 'error',
    'prefer-const': 'error',
    'prefer-rest-params': 'error',
    'prefer-spread': 'error',
    'prefer-template': 'error',
    'rest-spread-spacing': 'error',
    'template-curly-spacing': 'error',
    'yield-star-spacing': 'error'
  }
}
```

./vscode/settings.json

```js
{
  // vscode默认启用了根据文件类型自动设置tabsize的选项
  "editor.detectIndentation": false,
  // 重新设定tabsize
  "editor.tabSize": 2,
  // #每次保存的时候自动格式化
  "editor.formatOnSave": true,
  // #每次保存的时候将代码按eslint格式进行修复
  "eslint.autoFixOnSave": true,
  // 添加 vue 支持
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "vue",
      "autoFix": true
    }
  ],
  // #让prettier使用eslint的代码格式进行校验
  "prettier.eslintIntegration": true,
  // #去掉代码结尾的分号
  "prettier.semi": false,
  // #使用带引号替代双引号
  "prettier.singleQuote": true,
  // #让函数(名)和后面的括号之间加个空格
  "javascript.format.insertSpaceBeforeFunctionParenthesis": true,
  // #这个按用户自身习惯选择
  "vetur.format.defaultFormatter.html": "js-beautify-html",
  // #让vue中的js按编辑器自带的ts格式进行格式化
  "vetur.format.defaultFormatter.js": "vscode-typescript",
  "vetur.format.defaultFormatterOptions": {
    "js-beautify-html": {
      "wrap_attributes": "force-aligned"
      // #vue组件中html代码格式化样式
    }
  },
  // 格式化stylus, 需安装Manta's Stylus Supremacy插件
  "stylusSupremacy.insertColons": false, // 是否插入冒号
  "stylusSupremacy.insertSemicolons": false, // 是否插入分好
  "stylusSupremacy.insertBraces": false, // 是否插入大括号
  "stylusSupremacy.insertNewLineAroundImports": false, // import之后是否换行
  "stylusSupremacy.insertNewLineAroundBlocks": false // 两个选择器中是否换行
}
```
