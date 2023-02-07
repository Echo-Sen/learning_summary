# uni-app

## git 操作

### 创建仓库

```javascript
git init 
git add .
git status
git commit -m "项目名字"
git branch -M main
git remote add origin git@github.com:yltfy12/yltfy12.git
git push -u origin main
```

### 创建 tabBar 分支

```javascript
git checkout -b tabBar // 创建 tabBar 并切换到 tabBar分支
git branch // 查看当前全部分支
```

### tabBar 分支的提交与合并

```
git add .
git commit -m "完成了 tabBar 的开发"	
git push -u origin tabBar // 将本地的 tabBar 分支推送到远程仓库
git checkout master // 切换到主分支
git merge tabBar // 将 tabBar 合并到主分支
git branch -d tabBar // 删除本地分支
```

 ### home 分支的提交与合并

```
git add .
git commit -m "完成了 home 页面的开发"
git push -u origin home
git checkout master
git merge home 
git branch -d home
```

### cate 分支的提交与合并

```
git add .
git commit -m "完成了分类页面的开发"
git push -u origin cate
git checkout master
git merge cate 
git branch -d cate
```

### search 分支的提交与合并

问题：<img src="https://olrando.oss-cn-chengdu.aliyuncs.com/img/image-20230106163841641.png" alt="image-20230106163841641" style="zoom:67%;" />

解决：

```js
 git config core.autocrlf
```

core.autocrlf配置

假如你正在Windows上写程序，又或者你正在和其他人合作，他们在Windows上编程，而你却在其他系统上，在这些情况下，你可能会遇到行尾结束符问题。这是因为Windows使用回车和换行两个字符来结束一行，而Mac和Linux只使用换行一个字符。虽然这是小问题，但它会极大地扰乱跨平台协作。

- Git可以在你提交时自动地把行结束符CRLF转换成LF，而在签出代码时把LF转换成CRLF。用core.autocrlf来打开此项功能，如果是在Windows系统上，把它设置成true，这样当签出代码时，LF会被转换成CRLF

```js
git checkout -b search // 创建分支
git add .
git commit -m "完成了搜索页面的开发"
git push -u origin search
git checkout master
git merge search
git branch -d search
```

### goodsdetail 分支的提交与合并

```
git checkout -b goodsdetail // 创建分支
git add .
git commit -m "完成了商品详情页面的开发"
git push -u origin goodsdetail
git checkout master
git merge goodsdetail
git branch -d goodsdetail
```



git 取消与远程仓库的连接

`git remote remove origin`

## tabBar 

### 创建 tabBar 对应的页面

1. 在Hbuildx创建 ，在 微信开发工具 编译
2. 在 Hbuildx 中的 pages.json 中配置 tabBar 效果
3. 删除默认 index 页面，在 pages.json 删除 index 的路径配置
4. 在 pages.json 下的 globalStyle 节点下配置全局效果 (这里注意：<font color="red">要把其他页面默认生成的样式文件删除 否则不生效</font>)



## 首页

### 配置网络请求

1. 按照文档导入 第三方包 [文档地址](文档地址：https://www.npmjs.com/package/@escook/request-miniprogram)

2. 在 main.js 中完成如下配置

   ```javascript
   import { $http } from '@escook/request-miniprogram'
   
   uni.$http = $http
   // 配置请求根路径
   $http.baseUrl = 'https://api-ugo-web.itheima.net'
   
   // 请求开始之前做一些事情
   $http.beforeRequest = function (options) {
     uni.showLoading({
       title: '数据加载中...',
     })
   }
   
   // 请求完成之后做一些事情
   $http.afterRequest = function () {
     uni.hideLoading()
   }
   ```

### 轮播图区域

1. 请求轮播数据

   1. 定义 data 数据

   2. 在 onLoad 生命周期函数中调用获取轮播图数据的方法

   3. 在 methods 中定义获取轮播图的方法

      ```javascript
      export default {
        data() {
          return {
            // 1. 轮播图的数据列表，默认为空数组
            swiperList: [],
          }
        },
        onLoad() {
          // 2. 在小程序页面刚加载的时候，调用获取轮播图数据的方法
          this.getSwiperList()
        },
        methods: {
          // 3. 获取轮播图数据的方法
          async getSwiperList() {
            // 3.1 发起请求
            const { data: res } = await uni.$http.get('/api/public/v1/home/swiperdata')
            // 3.2 请求失败
            if (res.meta.status !== 200) {
              return uni.showToast({
                title: '数据请求失败！',
                duration: 1500,
                icon: 'none',
              })
            }
            // 3.3 请求成功，为 data 中的数据赋值
            this.swiperList = res.message
          },
        },
      }
      ```

      

2. 渲染 UI 结构

   1. 渲染 UI 结构

      ```javascript
      <template>
        <view>
          <!-- 轮播图区域 -->
          <swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
            <!-- 循环渲染轮播图的 item 项 -->
            <swiper-item v-for="(item, i) in swiperList" :key="i">
              <view class="swiper-item">
                <!-- 动态绑定图片的 src 属性 -->
                <image :src="item.image_src"></image>
              </view>
            </swiper-item>
          </swiper>
        </view>
      </template>
      ```

   2. 美化 UI 结构

      ```scss
      <style lang="scss">
      swiper {
       height: 330rpx;
      
       .swiper-item,
       image {
         width: 100%;
         height: 100%;
       }
      }
      </style>
      ```

      

3. 配置小程序分包

   1. 在项目根目录中，创建分包的根目录，命名为 `subpkg`

   2. 在 `pages.json` 中，和 `pages` 节点平级的位置声明 `subPackages` 节点，用来定义分包相关的结构：

      ```javascript
      {
        "pages": [
          {
            "path": "pages/home/home",
            "style": {}
          },
          {
            "path": "pages/cate/cate",
            "style": {}
          },
          {
            "path": "pages/cart/cart",
            "style": {}
          },
          {
            "path": "pages/my/my",
            "style": {}
          }
        ],
        "subPackages": [
          {
            "root": "subpkg",
            "pages": []
          }
        ]
      }
      ```

   3. 在 `subpkg` 目录上鼠标右键，点击 `新建页面` 选项，并填写页面的相关信息：

   

4. 点击轮播图跳转到详情页面

   将 `<swiper-item></swiper-item>` 节点内的 `view` 组件，改造为 `navigator` 导航组件，并动态绑定 `url 属性` 的值。

   1. 改造之前的 UI 界面

      ```javascript
      <swiper-item v-for="(item, i) in swiperList" :key="i">
        <view class="swiper-item">
          <!-- 动态绑定图片的 src 属性 -->
          <image :src="item.image_src"></image>
        </view>
      </swiper-item>
      ```

   2. 改造后的 UI 结构

      ```javascript
      <swiper-item v-for="(item, i) in swiperList" :key="i">
          <navigator class="swiper-item" :url="'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id">
            <!-- 动态绑定图片的 src 属性 -->
            <image :src="item.image_src"></image>
          </navigator>
      </swiper-item>
      ```

      

5. 封装 uni.$showMsg()

   1. 在 `main.js` 中，为 `uni` 对象挂载自定义的 `$showMsg()` 方法：

      ```javascript
      // 封装的展示消息提示的方法
      uni.$showMsg = function (title = '数据加载失败！', duration = 1500) {
        uni.showToast({
          title,
          duration,
          icon: 'none',
        })
      }
      ```

   2. 今后，在需要提示消息的时候，直接调用 `uni.$showMsg()` 方法即可：

      ```js
      async getSwiperList() {
         const { data: res } = await uni.$http.get('/api/public/v1/home/swiperdata')
         if (res.meta.status !== 200) return uni.$showMsg()
         this.swiperList = res.message
      }
      ```

      ​	   

### 分类导航

1. 获取数据
2. 渲染数据，调整 UI 界面
3. 事件处理函数

### 楼层区域

1. 获取数据
2. 渲染标题、图片
3. 事件处理函数





## 分类

获取数据 + 渲染数据

bug 修复: 在切换一级标题时右侧滚动条不自动到最顶端的问题。

```js
this.scrollTop = this.scrollTop === 0 ? 1 : 0
```

问题：在最后提交文件的时候遇到 本地仓库与远程仓库 无法匹配的问题，导致无法 push cate 分支到远程仓库

解决：在远程仓库手动同步各个分支的文件

## 搜索

#### 自定义搜索组件

##### 自定义 my-search 组件

- 在 components 下创建 my-search 组件
- 在 cate 调用 ` <my-search></my-search>` 
- 渲染美化组件 UI 结构
- 重新计算分类页面窗口可用高度,可用高度 = 屏幕高度 - navigationBar高度 - tabBar高度 - 自定义的search组件高度

##### 自定义属性

- 在组件与 data 平级创建 pros 节点，单独定义 背景颜色 和 圆角尺寸
- 用 属性绑定 的方式 ，为盒子动态绑定样式

##### 为组件封装 click 事件

- 在 组件 封装 click 事件

  ```js
  methods: {
    // 点击了模拟的 input 输入框
    searchBoxHandler() {
      // 触发外界通过 @click 绑定的 click 事件处理函数
      this.$emit('click')
    }
  }
  ```

- 在组件封装 click 事件后可在自定义事件中使用函数`<my-search @click="gotoSearch"></my-search>`

##### 首页搜索框吸顶效果

- 在 home 首页定义 UI　结构

- 在 home 首页定义 事件处理函数：

  ```js
  gotoSearch() {
    uni.navigateTo({
      url: '/subpkg/search/search'
    })
  }
  ```

- 样式吸顶

  ```js
  .search-box {
    // 设置定位效果为“吸顶”
    position: sticky;
    // 吸顶的“位置”
    top: 0;
    // 提高层级，防止被轮播图覆盖
    z-index: 999;
  }
  ```

  

#### 搜索建议

##### 渲染搜索页面基本结构

- 渲染+美化 `<uni-search-bar @input="input" :radius="100" cancelButton="none"></uni-search-bar>`
- 吸顶同 cate 页面
- 获取输入框中的内容

##### 自动获取焦点

- 修改 `components -> uni-search-bar -> uni-search-bar.vue` 组件，把 data 数据中的 `show` 和 `showSync` 的值，从默认的 `false` 改为 `true`

  ```js
  data() {
    return {
      show: true,
      showSync: true,
      searchVal: ""
    }
  }
  ```

  手机扫码预览

##### 防抖处理

- 在 data 中定义防抖的延时器 timerId 如下:

  ```js
  data() {
    return {
      // 延时器的 timerId
      timer: null,
      // 搜索关键词
      kw: ''
    }
  }
  ```

- 修改 input 事件处理函数 如下：

  ```js
  input(e) {
    // 清除 timer 对应的延时器
    clearTimeout(this.timer)
    // 重新启动一个延时器，并把 timerId 赋值给 this.timer
    this.timer = setTimeout(() => {
      // 如果 500 毫秒内，没有触发新的输入事件，则为搜索关键词赋值
      this.kw = e.value
    }, 500)
  }
  
  ```

<font color="red" >总结：在每个延时器开启之前一定要先清除 延时器</font>

##### 关键词查询搜索建议列表

- 在 data 中定义如下的数据节点，存放搜索建议的列表数据

  ```js
  data() {
    return {
      // 搜索结果列表
      searchResults: []
    }
  }
  ```

- 在防抖函数中，调用 方法获取搜索建议列表

  ```js
  this.timer = setTimeout(() => {
    this.kw = e.value
    // 根据关键词，查询搜索建议列表
    this.getSearchList()
  }, 500)
  ```

- 定义搜索建议列表

  ```js
  getSearchList() {
  	//判断关键词是否为空
      if(this.kw.length == 0){
  	this.searchList = []
          return
      }
      // 发起请求
       const { data: res } = await 										uni.$http.get('/api/public/v1/goods/qsearch', { query: this.kw })
    if (res.meta.status !== 200) return uni.$showMsg()
      this.searchResults = res.message
  }
  ```

##### 渲染建议列表

- 渲染+美化

- 点击 item 项目跳转到商品详情页

  ```js
  gotoDetail(goods_id){
  	uni.navitgateTo({
  	 // 指定详情页面的 URL 地址，并传递 goods_id 参数
      url: '/subpkg/goods_detail/goods_detail?goods_id=' + goods_id
  })
  }
  ```

#### 搜索历史

- 渲染+美化

- 使用 v-if v-else 按需渲染 搜索建议 和 搜索历史

- 将搜索关键词存入 historyList  `数组.push('追加名')`

  ```js
  methods: {
    // 根据搜索关键词，搜索商品建议列表
    async getSearchList() {
      // 省略其它不必要的代码...
  
      // 1. 查询到搜索建议之后，调用 saveSearchHistory() 方法保存搜索关键词
      this.saveSearchHistory()
    },
    // 2. 保存搜索关键词的方法
    saveSearchHistory() {
      // 2.1 直接把搜索关键词 push 到 historyList 数组中
      this.historyList.push(this.kw)
    }
  }
  ```

- 解决上述 顺序问题， 倒转数组：使用 计算属性 ，并且注意以下问题

  ```js
  computed: {
    historys() {
      // 注意：由于数组是引用类型，所以不要直接基于原数组调用 reverse 方法，以免修改原数组中元素的顺序
      // 而是应该新建一个内存无关的数组，再进行 reverse 反转
      return [...this.historyList].reverse()
    }
  }
  ```

- 解决上述 重复问题， 数组转化为集合

  ```js
  saveSearchHistory() {
        // 1. 将 Array 数组转化为 Set 对象
      const set = new Set(this.historyList)
       // 2. 调用 Set 对象的 delete 方法，移除对应的元素
      set.delete(this.kw)
      // 3. 调用 Set 对象的 add 方法，向 Set 中添加元素
      set.add(this.kw)
      // 4. 将 Set 对象转化为 Array 数组
      this.historyList = Array.from(set)
  }
  ```

- 搜索历史记录 储存到本地

  ```js
  saveSearchHistory() {
        // 1. 将 Array 数组转化为 Set 对象
      const set = new Set(this.historyList)
       // 2. 调用 Set 对象的 delete 方法，移除对应的元素
      set.delete(this.kw)
      // 3. 调用 Set 对象的 add 方法，向 Set 中添加元素
      set.add(this.kw)
      // 4. 将 Set 对象转化为 Array 数组
      this.historyList = Array.from(set)
      uni.setStorageSync('kw',JSON.stringfy(this.historyList))
  }
  ```

- 在 onLoad 中加载搜索历史记录

  ```js
  onLoad:{
  this.storyList = JSON.parse(uni.geiStorageSync('kw') || '[]')
  }
  ```

- 清空历史记录

- 点击搜索历史跳转到商品列表页面

  ```js
  // 绑定点击事件后
  toGoodsList(kw) {
    uni.navigateTo({
      url: '/subpkg/goods_list/goods_list?query=' + kw
    })
  }
  ```

  

## 商品列表

### 渲染 

### 美化

### 下拉菜单

### 上拉菜单 

### 节流阀 

### 防抖

## 商品详情

### 渲染

- 渲染商品详细信息 **富文本** 将HTML 渲染在页面上

  ```js
  <rich-text :nodes="goods_info.goods_introduce"></rich-text>
  ```

  

### 美化

### 解决价格为 undefine 的问题

在盒子最外部使用 `v-if` 检查是否数组从后端获取到数据



### 商品导航栏区域

- 使用 ` <uni-goods-nav :fill="true" :options="options" :buttonGroup="buttonGroup" @click="onClick" @buttonClick="buttonClick" />` 

  详细文档：[uni-app官网 (dcloud.net.cn)](https://uniapp.dcloud.net.cn/component/uniui/uni-goods-nav.html#)

- 跳转到购物车页面

  ```js
  // 注意这里 tabBar 的跳转 必须使用switchTab函数
  uni.switchTab({
        url: '/pages/cart/cart'
      })
  ```

## 加入购物车

### 配置 vuex

- 在根目录创建 store 文件夹 用来存 vuex 相关的模块

- 在 store 文件夹下创建 store.js

- ```js
  // 1.导入 vue vuex
  import Vue from 'vue'
  import Vuex from 'vuex'
  // 2.将 Vuex 安装为 Vue 的插件
  Vue.use(Vuex)
  // 3.创建 Store 的实例对象
  const store = new Vuex.Store({
      module:{},
  })
  // 4.向外共享 Store 的实例对象
  export default store
  ```

- 在 main.js 中导入 store 实例对象并挂载到Vue的实例上：

  ```js
  // 1. 导入 store 的实例对象
  import store from './store/store.js'
  
  // 省略其他代码...
  const app = new Vue({
      ...App,
      // 2.将 store 挂载到 Vue 实例上
      store
  })
  app.$mount()
  ```

  

### 创建购物车的 store 模块



- 在 store 文件夹下创建 cart.js 文件

- 在 cart.js 初始化 vuex 模块

  ```js
  export default {
  // 为当前模块开启命名空间
      namespaced:true,
      state:() => ({
          // 储存购物车每个商品的信息对象
          // 包含六个属性 { goods_id, goods_name, goods_price, goods_count, goods_small_logo, goods_state }
          cart:[]
      }),
      
      // 模块的 mutations 方法
      mutations:{},
      
      // 模块的 getters 属性
      getters:{},
  }
  ```

- 在 store/store.js 模块中，导入并挂载购物车的 vuex 模块

  ```js
  import Vue from 'vue'
  import Vuex from 'vuex'
  // 1. 导入购物车的 vuex 模块
  import moduleCart from './cart.js'
  
  Vue.use(Vuex)
  
  const store = new Vuex.Store({
    // TODO：挂载 store 模块
    modules: {
      // 2. 挂载购物车的 vuex 模块，模块内成员的访问路径被调整为 m_cart，例如：
      //    购物车模块中 cart 数组的访问路径是 m_cart/cart
      m_cart: moduleCart,
    },
  })
  
  export default store
  ```

  

### 在商品详情页中使用 Store 中的数据

- 在 goods_detail.vue 页面中

  ```js
  // 从 vuex 中按需导出 mapState 辅助方法
  import { mapState } from 'vuex'
  export default {
      computed:{
          ...mapState('m_cart',['cart'])
      },
      // 省略其他代码.. 
  }
  ```

  <blockquote>注意：今后无论映射 mutations 方法，还是 getters 属性，还是 state 中的数据，都需要指定模块的名称，才能进行映射。

- 页面渲染时可以直接使用 映射过来的数据

### 实现加入购物车的功能

- 在 store 目录下的 cart.js 模块中， 封装一个将商品信息加入购物车的 mutations 方法

  ```js
  export default {
    // 为当前模块开启命名空间
    namespaced: true,
  
    // 模块的 state 数据
    state: () => ({
      // 购物车的数组，用来存储购物车中每个商品的信息对象
      // 每个商品的信息对象，都包含如下 6 个属性：
      // { goods_id, goods_name, goods_price, goods_count, goods_small_logo, goods_state }
      cart: [],
    }),
  
    // 模块的 mutations 方法
    mutations: {
        // 判断数组中是否存在该商品， 不存在则加入到数组中，存在则让该商品数量+1
    	addToCart(state,goods){
          const findResult = state.cart.find(x => x.goods_id === goods.goods_id)
        	if(!findResult) {
  			state.cart.push.(goods)
          } else {
              findResult.goods_count++
          }
    }
      },
  
    // 模块的 getters 属性
    getters: {},
  }
  ```

- 将刚定义的 addToCart 映射到商品详情页面

  ```js
  // 按需导入 mapMutations 这个辅助方法
  import { mapMutations } from 'vuex'
  
  export default {
    methods: {
      // 把 m_cart 模块中的 addToCart 方法映射到当前页面使用
      ...mapMutations('m_cart', ['addToCart']),
    },
  }
  ```

- 为商品导航组件 `uni-goods-nav` 绑定 `@buttonClick="buttonClick"` 事件处理函数：

  ```js
  // 右侧按钮的点击事件处理函数
  buttonClick(e) {
     // 1. 判断是否点击了 加入购物车 按钮
     if (e.content.text === '加入购物车') {
  
        // 2. 组织一个商品的信息对象
        const goods = {
           goods_id: this.goods_info.goods_id,       // 商品的Id
           goods_name: this.goods_info.goods_name,   // 商品的名称
           goods_price: this.goods_info.goods_price, // 商品的价格
           goods_count: 1,                           // 商品的数量
           goods_small_logo: this.goods_info.goods_small_logo, // 商品的图片
           goods_state: true                         // 商品的勾选状态
        }
  
        // 3. 通过 this 调用映射过来的 addToCart 方法，把商品信息对象存储到购物车中
        this.addToCart(goods)
  
     }
  }
  ```

### 动态统计购物车中商品的总数量

- 在 `cart.js` 模块中，在 `getters` 节点下定义一个 `total` 方法，用来统计购物车中商品的总数量：

  ```js
  // 模块的 getters 属性
  getters: {
     // 统计购物车中商品的总数量
     total(state) {
        let c = 0
        // 循环统计商品的数量，累加到变量 c 中
        state.cart.forEach(goods => c += goods.goods_count)
        return c
     }
  }
  ```

- 在商品详情页面的 `script` 标签中，按需导入 `mapGetters` 方法并进行使用：

  ```js
  // 按需导入 mapGetters 这个辅助方法
  import { mapGetters } from 'vuex'
  
  export default {
    computed: {
      // 把 m_cart 模块中名称为 total 的 getter 映射到当前页面中使用
      ...mapGetters('m_cart', ['total']),
    },
  }
  ```

- 通过 `watch` 侦听器，监听计算属性 `total` 值的变化，从而**动态为购物车按钮的徽标赋值**：

  ```js
  export default {
    watch: {
      // 1. 监听 total 值的变化，通过第一个形参得到变化后的新值
      total(newVal) {
        // 2. 通过数组的 find() 方法，找到购物车按钮的配置对象
        const findResult = this.options.find((x) => x.text === '购物车')
  
        if (findResult) {
          // 3. 动态为购物车按钮的 info 属性赋值
          findResult.info = newVal
        }
      },
    },
  }
  ```

### 本地储存购物车中的商品

- 在 `cart.js` 模块中，声明一个叫做 `saveToStorage` 的 mutations 方法，此方法负责将购物车中的数据持久化存储到本地：

```js
// 将购物车中的数据持久化存储到本地
saveToStorage(state) {
   uni.setStorageSync('cart', JSON.stringify(state.cart))
}
```

- 修改 `mutations` 节点中的 `addToCart` 方法，在处理完商品信息后，调用步骤 1 中定义的 `saveToStorage` 方法：

  ```js
  addToCart(state, goods) {
     // 根据提交的商品的Id，查询购物车中是否存在这件商品
     // 如果不存在，则 findResult 为 undefined；否则，为查找到的商品信息对象
     const findResult = state.cart.find(x => x.goods_id === goods.goods_id)
  
     if (!findResult) {
        // 如果购物车中没有这件商品，则直接 push
        state.cart.push(goods)
     } else {
        // 如果购物车中有这件商品，则只更新数量即可
        findResult.goods_count++
     }
  
     // 通过 commit 方法，调用 m_cart 命名空间下的 saveToStorage 方法
     this.commit('m_cart/saveToStorage')
  }
  ```

- 修改 `cart.js` 模块中的 `state` 函数，读取本地存储的购物车数据，对 cart 数组进行初始化：

```js
// 模块的 state 数据
state: () => ({
   // 购物车的数组，用来存储购物车中每个商品的信息对象
   // 每个商品的信息对象，都包含如下 6 个属性：
   // { goods_id, goods_name, goods_price, goods_count, goods_small_logo, goods_state }
   cart: JSON.parse(uni.getStorageSync('cart') || '[]')
}),
```

### 优化监听器

- 问题：页面初次渲染的时候不会监听，数据无法渲染出来

- 解决：使用 **对象的形式 **来定义 watch 侦听器 

  ```js
  watch: {
     // 定义 total 侦听器，指向一个配置对象
     total: {
        // handler 属性用来定义侦听器的 function 处理函数
        handler(newVal) {
           const findResult = this.options.find(x => x.text === '购物车')
           if (findResult) {
              findResult.info = newVal
           }
        },
        // immediate 属性用来声明此侦听器，是否在页面初次加载完毕后立即调用
        immediate: true
     }
  }
  ```

### **动态为 tabBar 页面设置数字徽标 抽离为mixins 

- 文档：[混入 — Vue.js (vuejs.org)](https://v2.cn.vuejs.org/v2/guide/mixins.html)

- 在项目根目录中新建 `mixins` 文件夹，并在 `mixins` 文件夹之下新建 `tabbar-badge.js` 文件，用来把设置 tabBar 徽标的代码封装为一个 mixin 文件：

  ```js
  import { mapGetters } from 'vuex'
  
  // 导出一个 mixin 对象
  export default {
    computed: {
      ...mapGetters('m_cart', ['total']),
    },
    onShow() {
      // 在页面刚展示的时候，设置数字徽标
      this.setBadge()
    },
    methods: {
      setBadge() {
        // 调用 uni.setTabBarBadge() 方法，为购物车设置右上角的徽标
        uni.setTabBarBadge({
          index: 2,
          text: this.total + '', // 注意：text 的值必须是字符串，不能是数字
        })
      },
    },
  }
  ```

- 修改 `home.vue`，`cate.vue`，`cart.vue`，`my.vue` 这 4 个 tabBar 页面的源代码，分别导入 `@/mixins/tabbar-badge.js` 模块并进行使用：

```js
// 导入自己封装的 mixin 模块
import badgeMix from '@/mixins/tabbar-badge.js'

export default {
  // 将 badgeMix 混入到当前的页面中进行使用
  mixins: [badgeMix],
  // 省略其它代码...
}
```

## 购物车页面

### 商品列表区域

#### 渲染购物车标题区域

- 将 Store 中 cart 数组映射到当前页面中使用

```js
import badgeMix from '@/mixins/tabbar-badge.js'
// 按需导入 mapState 这个辅助函数
import { mapState } from 'vuex'

export default {
  mixins: [badgeMix],
  computed: {
    // 将 m_cart 模块中的 cart 数组映射到当前页面中使用
    ...mapState('m_cart', ['cart']),
  },
  data() {
    return {}
  },
}
```

#### 商品勾选状态

- 在 components 下面的组件 my-goods 加入 radio 标签 并引入到 购物车页面 

- 在 my-goods 中封装 showRadio 的 prop 属性，控制当前组件是否显示 radio 组件

  ```js
  export default {
    // 定义 props 属性，用来接收外界传递到当前组件的数据
    props: {
      // 商品的信息对象
      goods: {
        type: Object,
        default: {},
      },
      // 是否展示图片左侧的 radio
      showRadio: {
        type: Boolean,
        // 如果外界没有指定 show-radio 属性的值，则默认不展示 radio 组件
        default: false,
      },
    },
  }
  ```

- 利用 v-if 控制 radio 的显示

  ```xml
  <!-- 商品左侧图片区域 -->
  <view class="goods-item-left">
    <!-- 使用 v-if 指令控制 radio 组件的显示与隐藏 -->
    <radio checked color="#C00000" v-if="showRadio"></radio>
    <image :src="goods.goods_small_logo || defaultPic" class="goods-pic"></image>
  </view>
  ```

- 在 `cart.vue` 页面中的商品列表区域，指定 `:show-radio="true"` 属性，从而显示 radio 组件：

  ```xml
  <!-- 商品列表区域 -->
  <block v-for="(goods, i) in cart" :key="i">
    <my-goods :goods="goods" :show-radio="true"></my-goods>
  </block>
  ```

- 修改 `my-goods.vue` 组件，动态为 `radio` 绑定选中状态：

  ```xml
  <!-- 商品左侧图片区域 -->
  <view class="goods-item-left">
    <!-- 存储在购物车中的商品，包含 goods_state 属性，表示商品的勾选状态 -->
    <radio :checked="goods.goods_state" color="#C00000" v-if="showRadio"></radio>
    <image :src="goods.goods_small_logo || defaultPic" class="goods-pic"></image>
  </view>
  ```

#### 为 my-goods 组件封装 radio-change 事件

- 当用户点击 radio 组件，**希望修改当前商品的勾选状态**，此时用户可以为 `my-goods` 组件绑定 `@radio-change` 事件，从而获取当前商品的 `goods_id` 和 `goods_state`：

  ```xml
  <!-- 商品列表区域 -->
  <block v-for="(goods, i) in cart" :key="i">
    <!-- 在 radioChangeHandler 事件处理函数中，通过事件对象 e，得到商品的 goods_id 和 goods_state -->
    <my-goods :goods="goods" :show-radio="true" @radio-change="radioChangeHandler"></my-goods>
  </block>
  ```

- 定义 `radioChangeHandler` 事件处理函数如下：

  ```js
  methods: {
    // 商品的勾选状态发生了变化
    radioChangeHandler(e) {
      console.log(e) // 输出得到的数据 -> {goods_id: 395, goods_state: false}
    }
  }
  ```

- 在 `my-goods.vue` 组件中，为 `radio` 组件绑定 `@click` 事件处理函数如下：

  ```xml
  <!-- 商品左侧图片区域 -->
  <view class="goods-item-left">
    <radio :checked="goods.goods_state" color="#C00000" v-if="showRadio" @click="radioClickHandler"></radio>
    <image :src="goods.goods_small_logo || defaultPic" class="goods-pic"></image>
  </view>
  ```

- 在 `my-goods.vue` 组件的 methods 节点中，定义 `radioClickHandler` 事件处理函数：

  ```js
  methods: {
    // radio 组件的点击事件处理函数
    radioClickHandler() {
      // 通过 this.$emit() 触发外界通过 @ 绑定的 radio-change 事件，
      // 同时把商品的 Id 和 勾选状态 作为参数传递给 radio-change 事件处理函数
      this.$emit('radio-change', {
        // 商品的 Id
        goods_id: this.goods.goods_id,
        // 商品最新的勾选状态
        goods_state: !this.goods.goods_state
      })
    }
  }
  ```



#### 修改购物车中商品的勾选状态

- 在 `store/cart.js` 模块中，声明如下的 `mutations` 方法，用来修改对应商品的勾选状态：

  ```js
  // 更新购物车中商品的勾选状态
  updateGoodsState(state, goods) {
    // 根据 goods_id 查询购物车中对应商品的信息对象
    const findResult = state.cart.find(x => x.goods_id === goods.goods_id)
  
    // 有对应的商品信息对象
    if (findResult) {
      // 更新对应商品的勾选状态
      findResult.goods_state = goods.goods_state
      // 持久化存储到本地
      this.commit('m_cart/saveToStorage')
    }
  }
  ```

- 在 `cart.vue` 页面中，导入 `mapMutations` 这个辅助函数，从而将需要的 mutations 方法映射到当前页面中使用：

  ```js
  import badgeMix from '@/mixins/tabbar-badge.js'
  import { mapState, mapMutations } from 'vuex'
  
  export default {
    mixins: [badgeMix],
    computed: {
      ...mapState('m_cart', ['cart']),
    },
    data() {
      return {}
    },
    methods: {
      ...mapMutations('m_cart', ['updateGoodsState']),
      // 商品的勾选状态发生了变化
      radioChangeHandler(e) {
        this.updateGoodsState(e)
      },
    },
  }
  ```

#### 为 my-goods 封装 NumberBox

- `<uni-number-box :min="1"></uni-number-box>`

- 动态绑定商品的数量值`<uni-number-box :min="1" :value="goods.goods_count"></uni-number-box>` 

- 将商品列表页和购物车页面的 Numberbox 分开显示

  ```js
      // 是否展示价格右侧的 NumberBox 组件
      showNum: {
        type: Boolean,
            // 设定默认关闭
        default: false,
      },
  ```

  使用 v-if 判断，在` <my-goods :goods="goods" :show-radio="true" :show-num="true" @radio-change="radioChangeHandler"></my-goods>` 中传入showNum 的值为true

- 封装 num-change 事件

  还是事件绑定套路

#### 修改购物车中商品的数量

- 在 mutations 中定义
- 到 页面映射
- 在事件绑定函数中调用



#### 滑动删除功能

- uni-swipe-action 做最外层的包裹性质的容器

  ```xml
  <!-- 商品列表区域 -->
  <!-- uni-swipe-action 是最外层包裹性质的容器 -->
  <uni-swipe-action>
    <block v-for="(goods, i) in cart" :key="i">
      <!-- uni-swipe-action-item 可以为其子节点提供滑动操作的效果。需要通过 options 属性来指定操作按钮的配置信息 -->
      <uni-swipe-action-item :options="options" @click="swipeActionClickHandler(goods)">
        <my-goods :goods="goods" :show-radio="true" :show-num="true" @radio-change="radioChangeHandler" @num-change="numberChangeHandler"></my-goods>
      </uni-swipe-action-item>
    </block>
  </uni-swipe-action>
  ```

- 在 date 定义 options 数组

  ```js
  data() {
    return {
      options: [{
        text: '删除', // 显示的文本内容
        style: {
          backgroundColor: '#C00000' // 按钮的背景颜色
        }
      }]
    }
  }
  ```

- 声明 uni-swipe-action-item 组件的 @click 事件处理函数

  ```js
  // 点击了滑动操作按钮
  swipeActionClickHandler(goods) {
    console.log(goods)
  }
  ```

- 美化

  ```scss
  .goods-item {
    // 让 goods-item 项占满整个屏幕的宽度
    width: 750rpx;
    // 设置盒模型为 border-box
    box-sizing: border-box;
    display: flex;
    padding: 10px 5px;
    border-bottom: 1px solid #f0f0f0;
  }
  ```

- 使用 filter 对删除的商品过滤

  ```js
  // 根据 Id 从购物车中删除对应的商品信息
  removeGoodsById(state, goods_id) {
    // 调用数组的 filter 方法进行过滤
    state.cart = state.cart.filter(x => x.goods_id !== goods_id)
    // 持久化存储到本地
    this.commit('m_cart/saveToStorage')
  }
  ```

- 到页面映射调用

  ```js
  methods: {
    ...mapMutations('m_cart', ['updateGoodsState', 'updateGoodsCount', 'removeGoodsById']),
    // 商品的勾选状态发生了变化
    radioChangeHandler(e) {
      this.updateGoodsState(e)
    },
    // 商品的数量发生了变化
    numberChangeHandler(e) {
      this.updateGoodsCount(e)
    },
    // 点击了滑动操作按钮
    swipeActionClickHandler(goods) {
      this.removeGoodsById(goods.goods_id)
    }
  }
  ```



## 登录与支付

### 登录

- 定义组件

- 判断 token 状态渲染登录和未登录页面

- 获取用户信息

  **注意：**这里 getUserProfile() 需要在 `manifest.json` 中定义，具体看官方文档

  ```js
  // 获取微信用户的基本信息
  			getUserProfile() {
  				uni.getUserProfile({
  					desc: '你的授权信息',
  					success: (res) => {
  						// 将信息存到 vuex 中
  						this.updateUserInfo(res.userInfo)
  						this.getToken(res)
  					},
  					fail: (res) => {
  						return uni.$showMsg('您取消了登录授权')
  					}
  				})
  			},
              
  ```

- 登录换取 token 

  ```js
  // 调用登录接口，换取永久的 token
  async getToken(info) {
    // 调用微信登录接口
    const [err, res] = await uni.login().catch(err => err)
    // 判断是否 uni.login() 调用失败
    if (err || res.errMsg !== 'login:ok') return uni.$showError('登录失败！')
  
    // 准备参数对象
    const query = {
      code: res.code,
      encryptedData: info.encryptedData,
      iv: info.iv,
      rawData: info.rawData,
      signature: info.signature
    }
  
    // 换取 token
    const { data: loginResult } = await uni.$http.post('/api/public/v1/users/wxlogin', query)
    if (loginResult.meta.status !== 200) return uni.$showMsg('登录失败！')
    uni.$showMsg('登录成功')
  }
  ```

- token 储存到 vuex 

  将上面代码提到的登录成功改成 vuex 中更新 token 的函数 并传入服务器 发送的 token 即可

### 用户信息

- 将 vuex 中的身份信息映射到当前页面

- 渲染即可

- 退出登录功能 使用 showModal() 退出，并在退出时清空当前 身份信息 token 地址

  ```js
  // 退出登录
  async logout() {
    // 询问用户是否退出登录
    const [err, succ] = await uni.showModal({
      title: '提示',
      content: '确认退出登录吗？'
    }).catch(err => err)
  
    if (succ && succ.confirm) {
       // 用户确认了退出登录的操作
       // 需要清空 vuex 中的 userinfo、token 和 address
       this.updateUserInfo({})
       this.updateToken('')
       this.updateAddress({})
    }
  }
  ```

- 倒计时跳转

  ```js
  data() {
    return {
      // 倒计时的秒数
      seconds: 3，
      // 定时器 id
       timer:null
    }
  }
  
  // 展示倒计时的提示消息
  showTips(n) {
    // 调用 uni.showToast() 方法，展示提示消息
    uni.showToast({
      // 不展示任何图标
      icon: 'none',
      // 提示的消息
      title: '请登录后再结算！' + n + ' 秒后自动跳转到登录页',
      // 为页面添加透明遮罩，防止点击穿透
      mask: true,
      // 1.5 秒后自动消失
      duration: 1500
    })
  }
  
  // 延迟导航到 my 页面
  delayNavigate() {
    // 把 data 中的秒数重置成 3 秒
    this.seconds = 3
    this.showTips(this.seconds)
  
    this.timer = setInterval(() => {
      this.seconds--
  
      if (this.seconds <= 0) {
        clearInterval(this.timer)
        uni.switchTab({
          url: '/pages/my/my'
        })
        return
      }
  
      this.showTips(this.seconds)
    }, 1000)
  }
  
  ```

### 微信支付(未实现)

2023.1.10
