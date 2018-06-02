

# vue实现骨架屏

用到的插件

在devDependencies下安装一下插件

```
"webpack-node-externals": "^1.7.2",
"vue-server-renderer": "^2.5.16"
注意： 这里的vue-server-renderer的版本号要与vue和vue-template-compiler的版本号一致
这里的vue-server-renderer，是用来实现ssr的插件，详情见vue文档
```

实现原理

首先通过webpack将一个骨架样式的.vue文件,转化为一个json文件，在通过vue-server-renderer将这个json文件转化为html文件，并注入index.html中

原文链接

[原文](https://segmentfault.com/a/1190000014832185)

增加的文件

1. 一个skeleton.vue文件
2. 一个skeleton.enter.js文件，这个文件主要用来引用skeleton.vue文件
3. webpack.skeleton.conf.js文件，这个文件将上面的js在指定的文件夹下导出成一个json文件
4. skeleton.js文件，通过vue-server-renderer将json文件转化为html并注入index。html中

具体代码

```
// 骨架模板  skeleton.vue
<template>
  <div class="skeleton page">
    <div class="skeleton-nav"></div>
    <div class="skeleton-swiper"></div>
    <ul class="skeleton-tabs">
      <li v-for="i in 8" class="skeleton-tabs-item"><span></span></li>
    </ul>
    <div class="skeleton-banner"></div>
    <div v-for="i in 6" class="skeleton-productions"></div>
  </div>
</template>

<style>
.skeleton {
  position: relative;
  height: 100%;
  overflow: hidden;
  padding: 15px;
  box-sizing: border-box;
  background: #fff;
}
.skeleton-nav {
  height: 45px;
  background: #eee;
  margin-bottom: 15px;
}
.skeleton-swiper {
  height: 160px;
  background: #eee;
  margin-bottom: 15px;
}
.skeleton-tabs {
  list-style: none;
  padding: 0;
  margin: 0 -15px;
  display: flex;
  flex-wrap: wrap;
}
.skeleton-tabs-item {
  width: 25%;
  height: 55px;
  box-sizing: border-box;
  text-align: center;
  margin-bottom: 15px;
}
.skeleton-tabs-item span {
  display: inline-block;
  width: 55px;
  height: 55px;
  border-radius: 55px;
  background: #eee;
}
.skeleton-banner {
  height: 60px;
  background: #eee;
  margin-bottom: 15px;
}
.skeleton-productions {
  height: 20px;
  margin-bottom: 15px;
  background: #eee;
}
</style>
```

```
// 骨架入口文件 skeleton.enter.js
import Vue from 'vue'
import Skeleton from './page/skeleton/skeleton.vue'

export default new Vue({
  components: {
    Skeleton
  },
  template: '<skeleton />'
})
```

```
// 配置webpack webpack.skeleton.conf.js


const path = require('path')
const webpack = require('webpack')
const nodeExternals = require('webpack-node-externals')
const VueSSRServerPlugin = require('vue-server-renderer/server-plugin')

module.exports = {
  target: 'node',
  entry: {
    skeleton: './src/skeleton.entry.js'
  },
  output: {
    path: path.resolve(__dirname, './skeletondist'),
    publicPath: '/skeletondist/',
    filename: '[name].js',
    libraryTarget: 'commonjs2'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'vue-style-loader',
          'css-loader'
        ]
      },
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      }
    ]
  },
  externals: nodeExternals({
    whitelist: /\.css$/
  }),
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js'
    },
    extensions: ['*', '.js', '.vue', '.json']
  },
  plugins: [
    new VueSSRServerPlugin({
      filename: 'skeleton.json'
    })
  ]
}

```

```
将json转为html的 skeleton.js
//skeleton.js



const fs = require('fs')
const path = require('path')

const createBundleRenderer = require('vue-server-renderer').createBundleRenderer

// 读取`skeleton.json`，以`index.html`为模板写入内容
const renderer = createBundleRenderer(path.join(__dirname, './skeletondist/skeleton.json'), {
  template: fs.readFileSync(path.join(__dirname, './index.html'), 'utf-8')
})

// 把上一步模板完成的内容写入（替换）`index.html`
renderer.renderToString({}, (err, html) => {
  fs.writeFileSync('index.html', html, 'utf-8')
})
```

```
index.html


<html lang="en">
  <head><title>Hello</title></head>
  <body>
    <!--vue-ssr-outlet-->
  </body>
</html>

要添加一个 <!--vue-ssr-outlet-->
```