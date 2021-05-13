---
title: New Feature - Vue 3's Full TypeScript Support
date: 2021-04-09 23:58:23
tags:
 - Vue3
 - Typescript
clearReading: true
autoThumbnailImage: yes
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Front-end Knowledge
---
Vue3的新特性以及对TypeScript的全支持
<!--more-->
以前使用Vue2的时候相信很多朋友都没有特别好的体验。
但是尤大发布Vue3之后，据说能够友好地拥抱TypeScript了。
今天一边谈谈Vue3大新特性一边总结一下在Vue3里使用TypeScript的方法。

## 新特性
### Composition API
这是Vue3追加的最受注目的功能。Props和Content就可以生成全体Component都能使用和参照的参数。有点像React Hooks里记述的可以动态参照的参数。

原本的Vue2的Component里的话一般都像下面这样：
```html
export default {
  components: { RepositoriesFilters, RepositoriesSortBy, RepositoriesList },
  props: {
    user: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      repositories: [],
      filters: { ... },
      searchQuery: ''
    }
  },
  computed: {
    filteredRepositories () { ... },
    repositoriesMatchingSearchQuery () { ... },
  },
  watch: {
    user: 'getUserRepositories'
  },
  methods: {
    getUserRepositories () {
      // using `this.user` to fetch user repositories
    },
    updateFilters () { ... },
  },
  mounted () {
    this.getUserRepositories()
  }
}
```
这样会导致Component变得沉重,本来只是想进行一个处理, 但是却分散在不同的地方，影响可读性。
像下面这样同样的处理全部总结写在一处比较好。

例:
```html
import { fetchUserRepositories } from '@/api/repositories'
import { ref, onMounted, watch, toRefs, computed } from 'vue'

export default {
  props: {
    user: {
      type: String,
      required: true
    }
  },
  setup (props) {
    // using `toRefs` to create a Reactive Reference to the `user` property of props
    const { user } = toRefs(props)

    const repositories = ref([])
    const getUserRepositories = async () => {
      // update `props.user` to `user.value` to access the Reference value
      repositories.value = await fetchUserRepositories(user.value)
    }

    onMounted(getUserRepositories)

    // set a watcher on the Reactive Reference to user prop
    watch(user, getUserRepositories)

    const searchQuery = ref('')
    const repositoriesMatchingSearchQuery = computed(() => {
      return repositories.value.filter(
        repository => repository.name.includes(searchQuery.value)
      )
    })

    return {
      repositories,
      getUserRepositories,
      searchQuery,
      repositoriesMatchingSearchQuery
    }
  }
}
```
- `setup`返回的值可以供全体Component使用。
- `toRefs`和`ref`能够参照动态值,Component里使用`hoge.value`可以动态参照修改后的值。
- 支持生命周期,`onMounted`里可以指定在mount之后实行的参数。
- 使用`computed`后也可以生成computed value。

***
### Teleport
Vue Component里的dom的一部分, 在渲染的时候可以在指定的dom下进行描画。
在body下描画全画面的model的时候可以使用。

例子:
```html
app.component('modal-button', {
  template: `
    <button @click="modalOpen = true">
        Open full screen modal! (With teleport!)
    </button>

    <teleport to="body">
      <div v-if="modalOpen" class="modal">
        <div>
          I'm a teleported modal!
          (My parent is "body")
          <button @click="modalOpen = false">
            Close
          </button>
        </div>
      </div>
    </teleport>
  `,
  data() {
    return {
      modalOpen: false
    }
  }
})

```
***
### Fragments
在route里放置多个dom.

例: (v2)
```html
<template>
  <div>
    <header>...</header>
    <main>...</main>
    <footer>...</footer>
  </div>
</template>
```

例: (v3)
```html
<template>
  <header>...</header>
  <main v-bind="$attrs">...</main>
  <footer>...</footer>
</template>
```
***
### Emits Component Option
新追加的option.
为了记录Component是怎样运行的, 推荐写在所有emit了的自定义方法里面.

例:
```html
app.component('custom-form', {
  emits: {
    // No validation
    click: null,

    // Validate submit event
    submit: ({ email, password }) => {
      if (email && password) {
        return true
      } else {
        console.warn('Invalid submit event payload!')
        return false
      }
    }
  },
  methods: {
    submitForm() {
      this.$emit('submit', { email, password })
    }
  }
})
```
- 也可以使用`emit: ["click", "submit"]`这样的数列来进行定义.
- 如果使用object来指定参数的话, 将会被作为Validator来运行.

***
### Custom Render(createRender)
可以自定义你的渲染器。
可以使用option来在渲染的时候插入特定的处理.

例:
```html
import { createRenderer } from '@vue/runtime-core'
import { nodeOps } from '@vue/runtime-dom'

const { render, createApp } = createRenderer({
  ...nodeOps,
  insert: () => {},
})

// `render` is the low-level API
// `createApp` returns an app instance with configurable context shared
// by the entire app tree.
export { render, createApp }

export * from '@vue/runtime-core'
```
***
### TypeScript应对
下面是重头戏TS的应对.

定义Component的时候请注意
1.`script`标签里进行ts的声明
```html
<script lang="ts">
...
</script>
```

2.使用`defineComponent`参数
为了在Component里使用正确的类型定义,这里不使用常用的class,使用以下的参数就必须新生成一个Component.
```html
import { defineComponent } from 'vue'

const Component = defineComponent({
  // type inference enabled
})
```

改变的点：
Vue3开始对TS进行全力的支持这一点相信大家都有听过,Vue3里对TS部分进行了全部的重新定义,只是Composition API里对逻辑进行了总结, TS因此变得更好用了?



