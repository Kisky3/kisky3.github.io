---
title: Vue VeeValidation Sample
date: 2020-01-30 17:57:50
tags:
 - Vue
 - Validation
 - VeeValidation
 - ValidationObserver
 - ValidationProvider
clearReading: true
thumbnailImage: 20200130.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
Vue的表单验证组件 VeeValidation
<!--more-->

### 1. 安装VeeValidation
从npm安装VeeValidation
官网： [VeeValidation](https://logaretm.github.io/vee-validate/guide/basics.html)

```
$ npm install vee-validate --save
```

***
### 2. VeeValidation进行验证
```js
import { extend, ValidationProvider, ValidationObserver } from "vee-validate";
import { required } from "vee-validate/dist/rules";

<template>
  <section class="section">
    <div class="columns is-mobile">
      <div class="field">
        <label class="label">テキストフォーム</label>
          <div class="control">
            <validation-provider v-slot="{ errors }" rules="required" name="テキスト項目">
             <input v-model="textValue" class="input" type="text" placeholder="Text input" />
             <p v-show="errors.length" class="help is-danger">
               {{ errors[0] }}
             </p>
           </validation-provider>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Card from '~/components/Card'

export default {
  name: 'HomePage',
  data () {
    return {
      textValue: ''
    }
  }
}
</script>
```

写出来是这样子:
<img src="./1.png" style="width:300px" />

***

### 3. 所有的验证通过之前限制送信
实际的表单里有很多输入项目，对各个输入项进行验证的话，要使用**ValidationObserver**,还可以利用它使所有的验证通过之前，设定送信按钮无效。
pages/form.vue
```js
<template>
  <section class="section">
    <div class="columns is-mobile">
      <validation-observer v-slot="{ invalid }">
      <div class="field">
        <label class="label">お名前</label>
          <div class="control">
            <text-field-with-validation v-model="textValue" rules="required" fieldname="お名前" />
        </div>
      </div>
      <div class="field">
        <label class="label">メールアドレス</label>
          <div class="control">
            <text-field-with-validation v-model="emailValue" rules="required|email" fieldname="メールアドレス" />
        </div>
      </div>
      <div class="field">
          <div class="control">
            <button class="button is-link" :disabled="invalid">送信する</button>
            <p v-show="invalid" class="help is-danger">
             全ての必須項目を入力すると「新規追加」ボタンが有効化されます。
            </p>
        </div>
      </div>
    </validation-observer>
    </div>
  </section>
</template>

<script>
import TextFieldWithValidation from '@/components/TextFieldWithValidation'

export default {
  name: 'HomePage',
  components: {
    TextFieldWithValidation
  },
  data () {
    return {
      textValue: '',
      emailValue: ''
    }
  }
}
</script>
```
重要的point是：
在**<validation-observer v-slot="{ invalid }">**和**</validation-observer>**之间的表单项目都在VeeValidation的监视下，
如果有没有通过验证的项目的话**invalid**为true,给提交按钮加上**:disabled="invalid"**限制的话就可设置成无效状态了。

另外像邮件地址的表单的话，**rules="required|email"**在这里就是加了两个规则**required（必填验证）**和**email（邮件格式验证）**，
像这样用**|**隔开的话就可以设定复数的规则。

***

### 送信时首先进行表单全体验证
在验证没有全通过的时候，想自行定义操作处理的话，使用**this.$refs.observer.validate()**来进行表单的再次验证。
```js
<template>
  <validation-observer ref="observer" v-slot="{ invalid }" tag="form" @submit.prevent="submit()">
      <text-field-with-validation rules="required" v-model="first" />

      <text-field-with-validation rules="required" v-model="second" />

    <button :disabled="invalid">Submit</button>
  </validation-observer>
</template>

<script>
export default {
  methods: {
    async submit () {
      const isValid = await this.$refs.observer.validate();
      if (!isValid) {
        // バリデーションが通る前に送信ボタンがクリックされた場合の処理
      }
       // バリデーションが通っている状態で送信ボタンがクリックされた場合の処理
    }
  }
};
</script>
```

