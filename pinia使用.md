# pinia使用

## 基本介绍

Pinia 是 Vue.js 的轻量级状态管理库



## 基本使用及state使用

- 安装

```
yarn add pinia
# or
npm i pinia
```

- 挂载

```js
import { createApp } from 'vue'
import App from './App.vue'

// 在main.ts挂载
import { createPinia } from 'pinia'
const pinia = createPinia()

createApp(App).use(pinia).mount('#app')
```

- 新建文件store/counter.js

```js
import { defineStore } from 'pinia'
// 创建store,命名规则： useXxxxStore
// 参数1：store的唯一表示,仓库名
// 参数2：对象，可以提供state actions getters
const useCounterStore = defineStore('counter', {
  state: () => {
    return {
      count: 0,
    }
  },
  getters: {
   
  },
  actions: {
    
  },
})

export default useCounterStore

```

- 在组件中使用

```js
<script setup>
import useCounterStore from './store/counter'

const counter = useCounterStore()
</script>

<template>
      // 使用静态state 
  <h1>根组件---{{ counter.count }}</h1>
</template>

<style></style>

```

## actions的使用

在pinia中没有mutations，只有actions，不管是同步还是异步的代码，都可以在actions中完成。

- 在actions中提供方法并且修改数据

```js
import { defineStore } from 'pinia'
// 1. 创建store
// 参数1：store的唯一表示
// 参数2：对象，可以提供state actions getters
const useCounterStore = defineStore('counter', {
  state: () => {
    return {
      count: 0,
    }
  },
  actions: {
    increment() {
      this.count++
    },
    incrementAsync() {
      setTimeout(() => {
        this.count++
      }, 1000)
    },
  },
})

export default useCounterStore
```

- 在组件中使用

```js
<script setup>
import useCounterStore from './store/counter'

const counter = useCounterStore()
</script>

<template>
  <h1>根组件---{{ counter.count }}</h1>
  <button @click="counter.increment">加1</button>
  <button @click="counter.incrementAsync">异步加1</button>
</template>

```



## getters的使用

pinia中的getters和vuex中的基本是一样的，也带有缓存的功能

- 在getters中提供计算属性

```js
import { defineStore } from 'pinia'
// 1. 创建store
// 参数1：store的唯一表示
// 参数2：对象，可以提供state actions getters
const useCounterStore = defineStore('counter', {
  state: () => {
    return {
      count: 0,
    }
  },
  getters: {
    double() {
      return this.count * 2
    },
  },
  actions: {
    increment() {
      this.count++
    },
    incrementAsync() {
      setTimeout(() => {
        this.count++
      }, 1000)
    },
  },
})

export default useCounterStore


```

- 在组件中使用

```js
  <h1>根组件---{{ counter.count }}</h1>
  <h3>{{ counter.double }}</h3>

```



## storeToRefs的使用

如果直接从pinia中解构数据，会丢失响应式， 使用storeToRefs可以保证解构出来的数据也是响应式的

```js
<script setup>
import { storeToRefs } from 'pinia'
import useCounterStore from './store/counter'

const counter = useCounterStore()
// 如果直接从pinia中解构数据，会丢失响应式
const { count, double } = counter

// 使用storeToRefs可以保证解构出来的数据也是响应式的
const { count, double } = storeToRefs(counter)
</script>

```



## pinia模块化

在复杂项目中，不可能把多个模块的数据都定义到一个store中，一般来说会一个模块对应一个store，最后通过一个根store进行整合

- 新建store/user.js文件

```js
import { defineStore } from 'pinia'

const useUserStore = defineStore('user', {
  state: () => {
    return {
      name: 'zs',
      age: 100,
    }
  },
})

export default useUserStore


```

- 新建store/index.js

```js
import useUserStore from './user'
import useCounterStore from './counter'

// 统一导出useStore方法
export default function useStore() {
  return {
    user: useUserStore(),
    counter: useCounterStore(),
  }
}
```

- 在组件中使用

```js
<script setup>
import { storeToRefs } from 'pinia'
import useStore from './store'
const { counter } = useStore()

// 使用storeToRefs可以保证解构出来的数据也是响应式的
const { count, double } = storeToRefs(counter)
</script>
```



## pinia数据持久化

通过 `Pinia` 插件快速实现持久化存储

### 基础使用

- 安装

```js
yarn add pinia-plugin-persistedstate
or
npm i  pinia-plugin-persistedstate
```

- **使用插件 **在main.ts中注册

```js
import { createApp } from "vue";
import App from "./App.vue";

import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'
const pinia = createPinia();
pinia.use(piniaPluginPersistedstate);

createApp(App).use(pinia);
```

- **模块开启持久化**

```js
const useHomeStore = defineStore("home",{
  // 开启数据持久化
  persist: true
  // ...省略
});
```

- 模块做了持久化后，以后数据会不会变，怎么办？
  - 先读取本地的数据，如果新的请求获取到新数据，会自动把新数据覆盖掉旧的数据。
  - 无需额外处理，插件会自己更新到最新数据。

### 进阶用法

需求：不想所有数据都持久化处理，能不能按需持久化所需数据

- 可以用配置式写法，按需缓存某些模块的数据。

```js
import { defineStore } from 'pinia'

export const useStore = defineStore('main', s{
  state: () => {
    return {
      someState: 'hello pinia',
      nested: {
        data: 'nested pinia',
      },
    }
  },
  // 所有数据持久化
  // persist: true,
  // 持久化存储插件其他配置
  persist: {
    // 修改存储中使用的键名称，默认为当前 Store的 id
    key: 'storekey',
    // 修改为 sessionStorage，默认为 localStorage
    storage: window.sessionStorage,
    // 部分持久化状态的点符号路径数组，[]意味着没有状态被持久化(默认为undefined，持久化整个状态)
    paths: ['nested.data'],
  },
})

```

