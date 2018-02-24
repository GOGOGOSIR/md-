# vue-cli、web pack提取第三方库-----DllPlugin、DllReferencePlugin

## 需要安装的插件有

- extract-text-webpack-plugin
- assets-webpack-plugin
- clean-webpack-plugin

`npm i -D extract-text-webpack-plugin assets-webpack-plugin clean-webpack-plugin`

**目的**

使用webpack插件webpack.DllPlugin与webpack.DllReferencePlugin将不需要改动的第三方插件与自己的业务代码进行分开打包



## 配置文件

- 在 build 文件夹中新建 buildDll.js

```js
var path              = require('path');
var utils             = require('./utils')

var webpack           = require('webpack');
var config            = require('../config')
var utils             = require('./utils')
var dllConfig         = require('./webpack.dll.conf');
var ExtractTextPlugin = require('extract-text-webpack-plugin');
var AssetsPlugin      = require('assets-webpack-plugin');

var chalk = require('chalk')
var rm                = require('rimraf')
var ora = require('ora')
var spinner = ora({
  color: 'green',
  text: 'Dll生产中...'
})
spinner.start()
rm(path.resolve(__dirname, '../static'),  err => {
  if (err) throw err
  webpack(dllConfig,function (err, stats) {
    spinner.stop()
    if (err) throw err
    process.stdout.write(stats.toString({
          colors: true,
          modules: false,
          children: false,
          chunks: false,
          chunkModules: false
        }) + '\n\n')

    console.log(chalk.cyan('  dll succeed ！.\n'))
  })
});
```

- 在 build 文件夹中新建 webpack.dll.conf

```js
var path = require('path');
var webpack = require('webpack');      	//调用webpack内置DllPlugin插件
var config = require('../config')
var ExtractTextPlugin = require('extract-text-webpack-plugin'); // 提取css
var AssetsPlugin = require('assets-webpack-plugin'); // 生成文件名，配合HtmlWebpackPlugin增加打包后dll的缓存
var CleanWebpackPlugin = require('clean-webpack-plugin');//清空文件夹

module.exports = {
    entry: {
        libs: [
            'vue/dist/vue.esm.js',
            'vue-router',
            'vuex',

            'js-md5',
            'js-cookie',

            './src/assets/fastclick',
            './src/assets/base',
            './src/assets/flexible',
            // './src/assets/reset_for_mobile.css',
            './src/assets/localResizeIMG',
            './src/assets/qrious',

            './src/assets/sign',
            './src/assets/uid',
            './src/assets/uuid',


            // 'element-ui',
            // 'element-ui/lib/theme-default/index.css',
            // 'mint-ui',
            // 'mint-ui/lib/style.css',
        ]
    },
    output: {
        path: path.resolve(__dirname, '../static'),
        filename: '[name].[chunkhash:7].js',
        library: '[name]_library'
    },
    plugins: [
        new webpack.DllPlugin({
            path: path.resolve(__dirname, '../static/[name]-mainfest.json'),
            name: '[name]_library',
            context: __dirname // 执行的上下文环境，对之后DllReferencePlugin有用
        }),
        new ExtractTextPlugin('[name].[contenthash:7].css'),
        new webpack.optimize.UglifyJsPlugin({
            compress: {
                warnings: false
            },
        }),
        new AssetsPlugin({
            filename: 'bundle-config.json',
            path: './static'
        }),
        new CleanWebpackPlugin(['static'], {
            root: path.join(__dirname, '../'), // 绝对路径
            verbose: true, // 是否显示到控制台
            dry: false // 不删除所有
        }),
    ],
    module: {
        rules: [{
            test: /\.css$/,
            use: ExtractTextPlugin.extract({
                fallback: "style-loader",
                use: [{
                    loader: 'css-loader',
                    options: {
                        minimize: true //启用压缩
                    }
                }]
            })
        }, {
            test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
            loader: 'url-loader',
            query: {
                limit: 10000,
                name: 'img/[name].[hash:7].[ext]'
            }
        }, {
            test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
            loader: 'url-loader',
            query: {
                limit: 10000,
                name: 'fonts/[name].[hash:7].[ext]'
            }
        }]
    },
}
```

## 修改webpack.base.conf.js

```js
...
//新增
//引入webpack
var webpack = require('webpack');

module.exports = {
  entry: {
    ...
  },
  output: {
    ...
  },
  externals: {
    ...
  },
  resolve: {
    ...
  },
  module: {
    ...
  },
  
  //新增
  // 添加DllReferencePlugin插件
  plugins: [
      new webpack.DllReferencePlugin({
        context: __dirname,
        manifest: require('../static/libs-mainfest.json') // 指向生成的manifest.json
      }),
  ]
}
```

### 修改webpack.dev.conf

```js
...
//新增
var bundleConfig = require("../static/bundle-config.json")//调入生成的的路径json
...
module.exports = merge(baseWebpackConfig, {
  module: {
    ...
  },
  devtool: '#cheap-module-eval-source-map',
  plugins: [
    ...
    new HtmlWebpackPlugin({
      ...
      //新增
      libJsName:bundleConfig.libs.js,
      libCssName:bundleConfig.libs.css,
      env:config.dev.env,
    }),
    ...
  ]
})

```

## 修改webpack.prod.conf

```js
...
//新增
var bundleConfig = require("../static/bundle-config.json")//调入生成的的路径json
var CleanWebpackPlugin = require('clean-webpack-plugin');//清空文件夹
...
var webpackConfig = merge(baseWebpackConfig, {
    module: {
      ...
    },
    ...
    output: {
      ...
    },
    plugins: [
        ...
        new HtmlWebpackPlugin({
            ...
            //新增
            libJsName: bundleConfig.libs.js,
            libCssName: bundleConfig.libs.css,
            ...
        }),
        ...
      	//新增
        new CleanWebpackPlugin(['dist'], {
            root: path.resolve(__dirname, '../'),    // 设置绝对路径
            verbose: true,
            dry: false 
        }),
       ...
    ]
})
...
```

## 在`index.html`引入生成的dll.js、dll.css

```html
...
<body>
    <div id="app"></div>
  	<!-- 新增 -->
    <link rel="stylesheet" href="./static/<%= htmlWebpackPlugin.options.libCssName %>">
    <script src="./static/<%= htmlWebpackPlugin.options.libJsName %>"></script>
</body>
...
```

## 修改package.json

```js
{
  ...
  "scripts": {
    ...
    //新增
    "build:dll": "node build/buildDll.js",
    ...
  },
  ...
}
```

## 注意

- 无论是开发环境还是生产环境，都必须先运行 `npm run build:dll`
- dll文件打包的第三方库没有改变时，不需要再次运行