

# Nuxt.js的使用

一个基于vue.js 的服务端渲染应用框架，Nuxt.js 是一个基于 Vue.js 的通用应用框架，Nuxt.js 预设了利用Vue.js开发**服务端渲染**的应用所需要的各种配置

特性

- 基于 Vue.js
- 自动代码分层
- 服务端渲染
- 强大的路由功能，支持异步数据
- 静态文件服务
- ES6/ES7 语法支持
- 打包和压缩 JS 和 CSS
- HTML头部标签管理
- 本地开发支持热加载
- 集成ESLint
- 支持各种样式预处理器： SASS、LESS、 Stylus等等

### **安装模板**

```
vue init nuxt-community/starter-template <project-name>
其中starter-template是一个模板，而nuxt-community则是安装nuxtjs
```

### **目录结构**

Nuxt.js 的应用目录架构提供了良好的代码分层结构，适用于开发或大或小的应用。 当然，你也可以根据自己的偏好组织应用代码。

​	**资源目录：**

​		资源目录 `assets` 用于组织未编译的静态资源如 `LESS`、`SASS` 或 `JavaScript`。

​	**组件目录：**

​		components

​	**布局目录：**

​		布局目录 `layouts` 用于组织应用的布局组件。该目录名为Nuxt.js保留的，不可更改。

​	**中间件目录**

​		`middleware` 目录用于存放应用的中间件。

​	**页面目录**

​		页面目录 `pages` 用于组织应用的路由及视图。Nuxt.js 框架读取该目录下所有的 `.vue` 文件并自动生成		对应的路由配置。该目录名为Nuxt.js保留的，不可更改

​	**插件目录：**

​		插件目录 `plugins` 用于组织那些需要在 `根vue.js应用` 实例化之前需要运行的 Javascript 插件。

​	**静态文件目录:**

​		静态文件目录 `static` 用于存放应用的静态文件，此类文件不会被 Nuxt.js 调用 Webpack 进行构建编译处理。 服务器启动的时候，该目录下的文件会映射至应用的根路径 `/` 下。**举个例子:** /static/robots.txt 映射至 /robots.txt,该目录名为Nuxt.js保留的，不可更改。

​	**Store目录**

​		`store` 目录用于组织应用的 [Vuex 状态树](http://vuex.vuejs.org/) 文件。 Nuxt.js 框架集成了 [Vuex 状态树](http://vuex.vuejs.org/) 的相关功能配置，在 `store` 目录下创建一个 `index.js` 文件可激活这些配置。该目录名为Nuxt.js保留的，不可更改

​	**nuxt.config.js文件**

​		`nuxt.config.js` 文件用于组织Nuxt.js 应用的个性化配置，以便覆盖默认配置。该文件名为Nuxt.js保留的，不可更改。

​	**package.json文件**

​		`package.json` 文件用于描述应用的依赖关系和对外暴露的脚本接口。该文件名为Nuxt.js保留的，不可更改。



### **nuxtJs会根据pages目录结构自动生成vue-router模块的路由配置**

pages/
--| user/
-----| index.vue
-----| one.vue
--| index.vue

比如上面的这个例子：

在vue的路由配置就相当于如下：

```
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'user',
      path: '/user',
      component: 'pages/user/index.vue'
    },
    {
      name: 'user-one',
      path: '/user/one',
      component: 'pages/user/one.vue'
    }
  ]
}
```

**动态路由配置**

pages目录的配置如下：

```
pages/
--| _slug/
-----| comments.vue
-----| index.vue
--| users/
-----| _id.vue
--| index.vue
```

在vue的路由配置就相当于如下：

```
router: {
  routes: [
    {
      name: 'index',
      path: '/',
      component: 'pages/index.vue'
    },
    {
      name: 'users-id',
      path: '/users/:id?',
      component: 'pages/users/_id.vue'
    },
    {
      name: 'slug',
      path: '/:slug',
      component: 'pages/_slug/index.vue'
    },
    {
      name: 'slug-comments',
      path: '/:slug/comments',
      component: 'pages/_slug/comments.vue'
    }
  ]
}
你会发现名称为 users-id 的路由路径带有 :id? 参数，表示该路由是可选的。如果你想将它设置为必选的路由，需要在 users/_id 目录内创建一个 index.vue 文件。
和Vue中的router的动态路由很类似
在vue中：
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
获取这个参数则是用this.$route.params，这回 返回一个对象，id就在这个对象的键名为id的键值对中

```

**嵌套路由（二级路由）**

创建内嵌子路由，你需要添加一个 Vue 文件，同时添加一个**与该文件同名**的目录用来存放子视图组件

```
pages/
--| users/
-----| _id.vue
-----| index.vue
--| users.vue
```

在vue的路由配置就相当于如下：

```
router: {
  routes: [
    {
      path: '/users',
      component: 'pages/users.vue',
      children: [
        {
          path: '',
          component: 'pages/users/index.vue',
          name: 'users'
        },
        {
          path: ':id',
          component: 'pages/users/_id.vue',
          name: 'users-id'
        }
      ]
    }
  ]
}
```
### 路由参数校验

Nuxt.js 可以让你在动态路由组件中定义参数校验方法。

举个例子： `pages/users/_id.vue`

```
export default {
  validate ({ params }) {
    // Must be a number
    return /^\d+$/.test(params.id)
  }
}
```

如果校验方法返回的值不为 `true`， Nuxt.js 将自动加载显示 404 错误页面。

### 路由过渡

如果想要为每个页面书写公共样式的话,写在assets文件夹中： 比如我写个叫common.css的文件

```
Nuxt.js 默认使用的过渡效果名称为 page
.page-enter-active, .page-leave-active {
  transition: opacity .5s;
}
.page-enter, .page-leave-active {
  opacity: 0;
}
```

那么在nuxt.config.js应该如下配置：

```

module.exports = {
  css: [
    'assets/main.css'
  ]
}
```

上面是为全局的路由做配置

如果我只想为某个路由做过渡效果，因如下：

```
.test-enter-active, .test-leave-active {
  transition: opacity .5s;
}
.test-enter, .test-leave-active {
  opacity: 0;
}
然后我们将页面组件中的 transition 属性的值设置为 test 即可：
export default {
  transition: 'test'
}
```

### 中间件

> 中间件允许您定义一个自定义函数运行在一个页面或一组页面渲染之前。

每一个中间件应放置在 `middleware/` 目录。文件名的名称将成为中间件名称(`middleware/auth.js`将成为 `auth` 中间件)。

一个中间件接收 [context](https://zh.nuxtjs.org/api#上下文对象) 作为第一个参数：

```
export default function (context) {
  context.userAgent = context.isServer ? context.req.headers['user-agent'] : navigator.userAgent
}
```

中间件执行流程顺序：

1. `nuxt.config.js`
2. 匹配布局
3. 匹配页面

中间件可以异步执行,只需要返回一个 `Promise` 或使用第2个 `callback` 作为第一个参数：

`middleware/stats.js`

```
import axios from 'axios'

export default function ({ route }) {
  return axios.post('http://my-stats-api.com', {
    url: route.fullPath
  })
}
```

然后在你的 `nuxt.config.js` 、 layouts 或者 pages 中使用中间件:

`nuxt.config.js`

```
module.exports = {
  router: {
    middleware: 'stats'
  }  
}
```

`stats` 中间件将在每个路由改变时被调用。

### Nuxt.js 为页面提供的特殊配置项：

| 属性名         | 描述                                       |
| ----------- | ---------------------------------------- |
| asyncData   | 最重要的一个键, 支持 [异步数据处理](https://zh.nuxtjs.org/guide/async-data)，另外该方法的第一个参数为当前页面组件的 [上下文对象](https://zh.nuxtjs.org/api#上下文对象)。 |
| fetch       | 与 `asyncData` 方法类似，用于在渲染页面之前获取数据填充应用的状态树（store）。不同的是 `fetch` 方法不会设置组件的数据。详情请参考 [关于fetch方法的文档](https://zh.nuxtjs.org/api/pages-fetch)。 |
| head        | 配置当前页面的 Meta 标签, 详情参考 [页面头部配置API](https://zh.nuxtjs.org/api/pages-head)。 |
| layout      | 指定当前页面使用的布局（`layouts` 根目录下的布局文件）。详情请参考 [关于 布局 的文档](https://zh.nuxtjs.org/api/pages-layout)。 |
| transition  | 指定页面切换的过渡动效, 详情请参考 [页面过渡动效](https://zh.nuxtjs.org/api/pages-transition)。 |
| scrollToTop | 布尔值，默认: `false`。 用于判定渲染页面前是否需要将当前页面滚动至顶部。这个配置用于 [嵌套路由](https://zh.nuxtjs.org/guide/routing#嵌套路由)的应用场景。 |
| validate    | 校验方法用于校验 [动态路由](https://zh.nuxtjs.org/guide/routing#动态路由)的参数。 |
| middleware  | 指定页面的中间件，中间件会在页面渲染之前被调用， 请参考 [路由中间件](https://zh.nuxtjs.org/guide/routing#中间件)。 |

### Nuxt.js 使用了 vue-meta 更新应用的 头部标签(Head) and html 属性。

Nuxt.js 使用以下参数配置 vue-meta:

```
{

  keyName: 'head', // 设置 meta 信息的组件对象的字段，vue-meta 会根据这 key 值获取 meta 信息

  attribute: 'n-head', // vue-meta 在监听标签时所添加的属性名

  ssrAttribute: 'n-head-ssr', // 让 vue-meta 获知 meta 信息已完成服务端渲染的属性名

  tagIDKeyName: 'hid' // 让 vue-meta 用来决定是否覆盖还是追加 tag 的属性名

}


默认 Meta 标签

Nuxt.js 允许你在 nuxt.config.js 里定义应用所需的所有默认 meta 标签，在 head 字段里配置就可以了：

一个使用自定义 viewport 和 谷歌字体 的配置示例：

head: {
  meta: [
    { charset: 'utf-8' },
    { name: 'viewport', content: 'width=device-width, initial-scale=1' }
  ],
  link: [
    { rel: 'stylesheet', href: 'https://fonts.googleapis.com/css?family=Roboto' }
  ]
}

```

### 数据的异步

Nuxt.js 扩展了 Vue.js，增加了一个叫 `asyncData` 的方法，使得我们可以在设置组件的数据之前能异步获取或处理数据。

`asyncData`方法会在组件（**限于页面组件**）每次加载之前被调用。它可以在服务端或路由更新之前被调用。 在这个方法被调用的时候，第一个参数被设定为当前页面的[上下文对象](https://zh.nuxtjs.org/api#上下文对象)，你可以利用 `asyncData`方法来获取数据，Nuxt.js 会将 `asyncData` 返回的数据融合组件 `data` 方法返回的数据一并返回给当前组件。

**注意：由于`asyncData`方法是在组件 初始化 前被调用的，所以在方法内是没有办法通过 `this` 来引用组件的实例对象**。

Nuxt.js 提供了几种不同的方法来使用 `asyncData` 方法

#### 返回 Promise

```
export default {
  asyncData ({ params }) {
    return axios.get(`https://my-api/posts/${params.id}`)
    .then((res) => {
      return { title: res.data.title }
    })
  }
}
```

#### 使用 async或await

```
export default {
  async asyncData ({ params }) {
    let { data } = await axios.get(`https://my-api/posts/${params.id}`)
    return { title: data.title }
  }
}
```

#### 使用 回调函数

```
export default {
  asyncData ({ params }, callback) {
    axios.get(`https://my-api/posts/${params.id}`)
    .then((res) => {
      callback(null, { title: res.data.title })
    })
  }
}
```

#### 返回 对象

如果组件的数据不需要异步获取或处理，可以直接返回指定的字面对象作为组件的数据。

```
export default {
  data () {
    return { foo: 'bar' }
  }
}
```

#### 错误处理

Nuxt.js 在上下文对象`context`中提供了一个 `error(params)` 方法，你可以通过调用该方法来显示错误信息页面。`params.statusCode` 可用于指定服务端返回的请求状态码。

以返回 `Promise` 的方式举个例子：

```
export default {
  asyncData ({ params, error }) {
    return axios.get(`https://my-api/posts/${params.id}`)
    .then((res) => {
      return { title: res.data.title }
    })
    .catch((e) => {
      error({ statusCode: 404, message: 'Post not found' })
    })
  }
}
```

如果你使用 `回调函数` 的方式, 你可以将错误的信息对象直接传给该回调函数， Nuxt.js 内部会自动调用 `error` 方法：

```
export default {
  asyncData ({ params }, callback) {
    axios.get(`https://my-api/posts/${params.id}`)
    .then((res) => {
      callback(null, { title: res.data.title })
    })
    .catch((e) => {
      callback({ statusCode: 404, message: 'Post not found' })
    })
  }
}
```

#### **注意：**

**需要注意的是，在任何 Vue 组件的[生命周期](https://vuejs.org/v2/guide/instance.html#Lifecycle-Diagram)内， 只有 `beforeCreate` 和 `created` 这两个钩子方法会在 客户端和服务端均被调用。其他钩子方法仅在客户端被调用。**

## 使用第三方模块

我们可以在应用中使用第三方模块，一个典型的例子是在客户端和服务端使用 [axios](https://github.com/mzabriskie/axios) 做 HTTP 请求。

首先我们需要安装 npm 包：

```
npm install --save axios
```

然后在页面内可以这样使用：

```
<template>
  <h1>{{ title }}</h1>
</template>

<script>
import axios from 'axios'

export default {
  async asyncData ({ params }) {
    let { data } = await axios.get(`https://my-api/posts/${params.id}`)
    return { title: data.title }
  }
}
</script>
```

有一个**值得注意的问题**是，如果我们在另外一个页面内也引用了 `axios`，那么在应用打包发布的时候 `axios` 会被打包两次，而实际上我们只需要打包一次。这个问题可以通过在 `nuxt.config.js` 里面配置 `build.vendor` 来解决：

```
module.exports = {
  build: {
    vendor: ['axios']
  }
}
```

经过上面的配置后，我们可以在任何页面里面引入 `axios` 而不用担心它会被重复打包。

## 使用 Vue 插件

假如我们想使用 [vue-notifications](https://github.com/se-panfilov/vue-notifications) 显示应用的通知信息，我们需要在程序运行前配置好这个插件。

首先增加文件 `plugins/vue-notifications.js`：

```
import Vue from 'vue'
import VueNotifications from 'vue-notifications'

Vue.use(VueNotifications)
```

然后, 在 `nuxt.config.js` 内配置 `plugins` 如下：

```
module.exports = {
  plugins: ['~plugins/vue-notifications']
}
```

实际上， `vue-notifications` 会被打包至应用的脚本代码里， 但是它属于第三方库，我们理应将它打包至库文件里以获得更好的缓存效果。（译者注：应用代码比库文件修改频繁，应尽量将第三方库打包至单独的文件中去）。

我们可以更新 `nuxt.config.js` 文件，在 `vendor` 构建配置项里添加 `vue-notifications`：

```
module.exports = {
  build: {
    vendor: ['vue-notifications']
  },
  plugins: ['~plugins/vue-notifications']
}
```

## 只在浏览器里使用的插件

有些插件可能只是在浏览器里使用，所以你可以用 `ssr: false` 变量来配置插件只从客户端还是服务端运行。

举个例子：

`nuxt.config.js`:

```
module.exports = {
  plugins: [
    { src: '~plugins/vue-notifications', ssr: false }
  ]
}
```

`plugins/vue-notifications.js`:

```
import Vue from 'vue'
import VueNotifications from 'vue-notifications'

Vue.use(VueNotifications)
```

同样地，如果有些脚本库你只想在服务端使用，在 Webpack 打包 `server.bundle.js` 文件的时候你可以将 `process.SERVER_BUILD` 变量设置成 `true`。

### vuex状态树

对于每个大项目来说，使用状态树 (store) 管理状态 (state) 十分有必要。这就是为什么 Nuxt.js 内核实现了 [Vuex](https://github.com/vuejs/vuex)。

Nuxt.js 会尝试找到应用根目录下的 `store` 目录，如果该目录存在，它将做以下的事情：

1. 引用 `vuex` 模块
2. 将 `vuex` 模块 加到 vendors 构建配置中去
3. 设置 `Vue` 根实例的 `store` 配置项

Nuxt.js 支持两种使用 `store` 的方式，你可以择一使用：

- **普通方式：** `store/index.js` 返回一个 Vuex.Store 实例
- **模块方式：** `store` 目录下的每个 `.js` 文件会被转换成为状态树[指定命名的子模块](http://vuex.vuejs.org/en/modules.html) （当然，`index` 是根模块）

### 普通方式

使用普通方式的状态树，需要添加 `store/index.js` 文件，并对外暴露一个 Vuex.Store 实例：

```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = () => new Vuex.Store({

  state: {
    counter: 0
  },
  mutations: {
    increment (state) {
      state.counter++
    }
  }
})

export default store
```

> Nuxt.js 内置引用了 `vuex` 模块，所以不需要额外安装。

现在我们可以在组件里面通过 `this.$store` 来使用状态树：

```
<template>
  <button @click="$store.commit('increment')">{{ $store.state.counter }}</button>
</template>
```

### 模块方式

> 状态树还可以拆分成为模块，`store` 目录下的每个 `.js` 文件会被转换成为状态树[指定命名的子模块](http://vuex.vuejs.org/en/modules.html)

使用**状态树模块化**的方式，`store/index.js` 不需要返回 Vuex.Store 实例，而应该直接将 `state`、`mutations` 和 `actions` 暴露出来：

```
export const state = () => ({
  counter: 0
})

export const mutations = {
  increment (state) {
    state.counter++
  }
}
```

其他的模块文件也需要采用类似的方式，如 `store/todos.js` 文件：

```
export const state = () => ({
  list: []
})

export const mutations = {
  add (state, text) {
    state.list.push({
      text: text,
      done: false
    })
  },
  remove (state, { todo }) {
    state.list.splice(state.list.indexOf(todo), 1)
  },
  toggle (state, todo) {
    todo.done = !todo.done
  }
}
```

最终的状态树大概如下：

```
new Vuex.Store({
  state: { counter: 0 },
  mutations: {
    increment (state) {
      state.counter++
    }
  },
  modules: {
    todos: {
      state: {
        list: []
      },
      mutations: {
        add (state, { text }) {
          state.list.push({
            text,
            done: false
          })
        },
        remove (state, { todo }) {
          state.list.splice(state.list.indexOf(todo), 1)
        },
        toggle (state, { todo }) {
          todo.done = !todo.done
        }
      }
    }
  }
})
```

在页面组件 `pages/todos.vue`， 可以像下面这样使用 `todos` 模块：

```
<template>
  <ul>
    <li v-for="todo in todos">
      <input type="checkbox" :checked="todo.done" @change="toggle(todo)">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
    </li>
    <li><input placeholder="What needs to be done?" @keyup.enter="addTodo"></li>
  </ul>
</template>

<script>
import { mapMutations } from 'vuex'

export default {
  computed: {
    todos () { return this.$store.state.todos.list }
  },
  methods: {
    addTodo (e) {
      this.$store.commit('todos/add', e.target.value)
      e.target.value = ''
    },
    ...mapMutations({
      toggle: 'todos/toggle'
    })
  }
}
</script>

<style>
.done {
  text-decoration: line-through;
}
</style>
```

fetch 方法

fetch 方法会在渲染页面前被调用，作用是填充状态树 (store) 数据，与 asyncData 方法类似，不同的是它不会设置组件的数据。

## nuxtServerInit 方法

如果在状态树中指定了 `nuxtServerInit` 方法，Nuxt.js 调用它的时候会将页面的上下文对象作为第2个参数传给它（服务端调用时才会酱紫哟）。当我们想将服务端的一些数据传到客户端时，这个方法是灰常好用的。

举个例子，假设我们服务端的会话状态树里可以通过 `req.session.user` 来访问当前登录的用户。将该登录用户信息传给客户端的状态树，我们只需更新 `store/index.js` 如下：

```
actions: {
  nuxtServerInit ({ commit }, { req }) {
    if (req.session.user) {
      commit('user', req.session.user)
    }
  }
}
```

> 如果你使用*状态树模块化*的模式，只有主模块（即 `store/index.js`）适用设置该方法（其他模块设置了也不会被调用）。

`nuxtServerInit` 方法接收的上下文对象和 `fetch` 的一样，但不包括 `context.redirect()` 和 `context.error()`。