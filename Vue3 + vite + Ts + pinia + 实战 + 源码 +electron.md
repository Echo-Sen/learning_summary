# Vue3 + vite + Ts + pinia + 实战 + 源码 +electron

## vue3

### bem布局

### 代码分包

用于包裹异步的组件

```html
<Suspense></Suspense>
```



### jsx tsx



### 自动引入插件

`unplugin-auto-import`是一个按需自动导入Vue/Vue Router等官方Api的插件；作者是Vite生态圈大名鼎鼎的[Anthony Fu](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fantfu)

使用此插件后，不需要手动编写`import {xxx} from vue`这样的代码了，提升开发效率。

```js
  // vite.config.js
import AutoImport from 'unplugin-auto-import/vite'

export default defineConfig({
  plugins: [
    AutoImport({
      // targets to transform
      include: [
        /\.[tj]sx?$/, 
        /\.vue$/, 
        /\.vue\?vue/, 
        /\.md$/,
      ],
  // global imports to register
  imports: [
    // 插件预设支持导入的api
    'vue',
    'vue-router',
    'pinia'
    // 自定义导入的api
  ],

  // Generate corresponding .eslintrc-auto-import.json file.
  // eslint globals Docs - https://eslint.org/docs/user-guide/configuring/language-options#specifying-globals
  eslintrc: {
    enabled: false, // Default `false`
    filepath: './.eslintrc-auto-import.json', // Default `./.eslintrc-auto-import.json`
    globalsPropValue: true, // Default `true`, (true | false | 'readonly' | 'readable' | 'writable' | 'writeable')
  },

  // Filepath to generate corresponding .d.ts file.
  // Defaults to './auto-imports.d.ts' when `typescript` is installed locally.
  // Set `false` to disable.
  dts: './auto-imports.d.ts',
})
  ],
})
```


proxy跨域

![image-20230704193636999](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230704193636999.png)



## pinia

与vuex相比 去掉了mutation属性，转而将所有同步异步都放在actives属性下执行，再无需使用dispatch等函数调用，转而直接引入对象，以调用对象中的属性、函数的形式来操作state里面的数据

### $reset

用于恢复state 的初始值



### $subScribe类似于watch

### 柯里化

将函数的多个参数转化为一个参数

![image-20230705095135411](https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230705095135411.png)