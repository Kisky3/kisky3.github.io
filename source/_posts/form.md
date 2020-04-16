---
title: Vue Form Component
date: 2020-01-14 22:51:40
tags:
  - vue
  - form
clearReading: true
thumbnailImage: 20200114.png
thumbnailImagePosition: left
coverImage: cover.jpg
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---

Vue.js 的表单要素组件化

<!--more-->

### Input

首先是 input 的组件化。`field`的值是通过`@input`来监听变化，并使用`$emit`的`'input'`来给父组件的 v-model 传值。`updateValue`通过参数 e，取得`event`，并进一步取得`target.value`,通过`emit`传值给父组件。

子组件 MyInput.vue
```html
<template>
  <input
    :type="type"
    :name="name"
    :value="value"
    :placeholder="placeholder"
    @input="updateValue"
  />
</template>

<script>
export default {
  name: "MyInput",
  props: {
    value: { type: String, required: true },
    type: { type: String, required: true },
    name: { type: String, required: true },
    placeholder: { type: String, required: false }
  },
  methods: {
    updateValue: function(e) {
      this.$emit("input", e.target.value);
    }
  }
};
</script>
```
父组件 App.vue
```html
<MyInput
  v-model="sampleForm.text"
  placeholder="サンプル"
  name="sample-input"
  type="text"
></MyInput>
```
***

### Textarea
子组件 MyTextarea.vue
```html
<template>
  <textarea
    :name="name"
    :value="value"
    :placeholder="placeholder"
    :rows="rows"
    :cols="cols"
    @input="updateValue"
  ></textarea>
</template>

<script>
export default {
  name: "MyTextarea",
  props: {
    value: { type: String, required: true },
    name: { type: String, required: true },
    placeholder: { type: String, required: false },
    rows: { type: Number, required: false },
    cols: { type: Number, required: false }
  },
  methods: {
    updateValue: function(e) {
      this.$emit("input", e.target.value);
    }
  }
};
</script>
```

父组件 App.vue
```html
<MyTextarea
  v-model="sampleForm.textarea"
  placeholder="サンプル"
  name="sample-textarea"
  :rows="10"
  :cols="50"
></MyTextarea>
```
***

### Radio Button
子组件 MyRadio
```html
<template>
  <fieldset>
    <template v-for="(option, index) in options">
      <label :key="index">
        <input
          type="radio"
          :name="name"
          :value="option.value"
          @change="updateValue"
        />{{ option.label }}
      </label>
    </template>
  </fieldset>
</template>

<script>
export default {
  name: "MyRadio",
  props: {
    value: { type: String, required: true },
    options: { type: Array, required: true },
    name: { type: String, required: true },
  },
  methods: {
    updateValue: function(e) {
      this.$emit("input", e.target.value);
    }
  }
};
</script>
```

父组件 App.vue
```html
// 変数optionsには[{label: "hoge", value: "1"}, {label: "fuga", value: "2"}]のようなオブジェクトの配列を設定
<MyRadio
  v-model="sampleForm.radio"
  name="sample-radio"
  :options="options"
></MyRadio>
```
***
### Select
子组件 select.vue
```html
<template>
  <fieldset>
    <select :name="name" @change="updateValue">
      <template v-for="(option, index) in options">
        <option :value="option.value" :key="index">
          {{ option.label }}
        </option>
      </template>
    </select>
  </fieldset>
</template>

<script>
export default {
  name: "MySelect",
  props: {
    value: { type: String, required: true },
    options: { type: Array, required: true },
    name: { type: String, required: true },
  },
  methods: {
    updateValue: function(e) {
      this.$emit("input", e.target.value);
    }
  },
  mounted() {
    this.$emit("input", this.options[0].value);
  }
};
</script>
```

父组件 App.vue
```html
```App.vue
// 変数optionsには[{label: "hoge", value: "1"}, {label: "fuga", value: "2"}]のようなオブジェクトの配列を設定
<MySelect
  v-model="sampleForm.select"
  name="sample-select"
  :options="options"
></MySelect>
```
***
### Checkbox
子组件 MyCheckbox.vue
```html
<template>
  <fieldset>
    <template v-for="(option, index) in options">
      <label :key="index">
        <input
          type="checkbox"
          :name="name"
          :value="option.value"
          @change="updateValue"
        />{{ option.label }}
      </label>
    </template>
  </fieldset>
</template>

<script>
export default {
  name: "MyCheckbox",
  props: {
    options: { type: Array, required: true },
    name: { type: String, required: true },
  },
  data() {
    return {
      values: []
    };
  },
  methods: {
    updateValue: function(e) {
      if (e.target.checked) {
        this.values.push(e.target.value);
      } else {
        this.values = this.values.filter(v => v !== e.target.value);
      }
      this.$emit("input", this.values);
    }
  }
};
</script>
```
父组件 App.vue
```html
// 変数optionsには[{label: "hoge", value: "1"}, {label: "fuga", value: "2"}]のようなオブジェクトの配列を設定
<MyCheckbox
  v-model="sampleForm.checkbox"
  name="sample-checkbox"
  :options="options"
></MyCheckbox>
```
***
### Form全体
```html
<template>
  <div id="app">
    <form action="">
      <MyLabel>InputText</MyLabel>
      <MyInput
        v-model="sampleForm.text"
        placeholder="サンプル"
        name="sample-input"
        type="text"
      ></MyInput>
      <MyLabel>TextArea</MyLabel>
      <MyTextarea
        v-model="sampleForm.textarea"
        placeholder="サンプル"
        name="sample-textarea"
        :rows="10"
        :cols="50"
      ></MyTextarea>
      <MyLabel>RadioButton</MyLabel>
      <MyRadio
        v-model="sampleForm.radio"
        name="sample-radio"
        :options="options"
      ></MyRadio>
      <MyLabel>Checkbox</MyLabel>
      <MyCheckbox
        v-model="sampleForm.checkbox"
        name="sample-checkbox"
        :options="options"
      ></MyCheckbox>
      <MyLabel>Select</MyLabel>
      <MySelect
        v-model="sampleForm.select"
        name="sample-select"
        :options="options"
      ></MySelect>
      <MyBtn @click="sendForm">send</MyBtn>
    </form>
  </div>
</template>

<script>
import MyInput from "./components/MyInput";
import MyTextarea from "./components/MyTextarea";
import MyRadio from "./components/MyRadio";
import MyCheckbox from "./components/MyCheckbox";
import MySelect from "./components/MySelect";
import MyBtn from "./components/MyBtn";
import MyLabel from "./components/MyLabel";

export default {
  name: "app",
  components: {
    MyTextarea,
    MyInput,
    MyRadio,
    MyCheckbox,
    MySelect,
    MyBtn,
    MyLabel
  },
  data() {
    return {
      sampleForm: {
        text: "",
        radio: "",
        select: "",
        textarea: "",
        checkbox: []
      },
      options: [
        {
          label: "fizz",
          value: "3"
        },
        {
          label: "buzz",
          value: "5"
        },
        {
          label: "fizzBuzz",
          value: "15"
        }
      ]
    };
  },
  methods: {
    sendForm() {
      // formデータの送信処理
    }
  }
};
</script>
```
