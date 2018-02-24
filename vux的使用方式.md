## vux的使用方式

安装

```
npm install vux --save
安装vux-loader （这个vux文档似乎没介绍，当初没安装结果报了一堆错误）
npm install vux-loader --save-dev
npm install less less-loader --save-dev
```

并且要配置webpack.base.conf.js文件

```
const vuxLoader = require('vux-loader')

const webpackConfig = originalConfig
这里的originalConfig就是原来model.export的那个对象赋值给webpackConfig
const webpackConfig = {
  entry: {
    app: './src/main.js'
  },
  output: {
    path: config.build.assetsRoot,
    filename: '[name].js',
    publicPath: process.env.NODE_ENV === 'production'
      ? config.build.assetsPublicPath
      : config.dev.assetsPublicPath
  },
  resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
    }
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: vueLoaderConfig
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        include: [resolve('src'), resolve('test')]
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('media/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
        }
      }
    ]
  }
}
最后加上
module.exports = vuxLoader.merge(webpackConfig, {
  plugins: ['vux-ui']
})
```

引用vux的组件

```
import {Tab,TabItem} from 'vux'
然后加上
components: {
    Tab,
    TabItem
}
```

引用的时候如果是按照官文文档一样用，则是默认全部引用

部分引用

import Spinner from 'vux/src/components/spinner/index.vue'