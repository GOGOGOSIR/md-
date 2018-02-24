# cube的使用

一般我们使用cube-ui是按需加载

安装： 

npm install cube-ui --save

默认是后编译 所以其他的项目配置见官方文档

[官方文档](https://didi.github.io/cube-ui/#/zh-CN/docs/quick-start#cube-安装-anchor)

为了使代码变得优雅

一般我们要加一个插件

```
npm install --save-dev babel-plugin-transform-modules
```

```
这样写就可以了，如果你是使用的后编译，直接引入的是源码，那么只需要在 .babelrc 文件中增加如下配

{
    "plugins": [
        ["transform-modules", {
            "cube-ui": {
                "transform": "cube-ui/lib/${member}",
                "preventFullImport": true
            }
        }]
    ]
}
而如果是 webpack 1 或者说使用的组件库是已经编译后的，那只需要增设 style 配置项即可：
"plugins": [
  ["transform-modules", {
	 "cube-ui": {
	   "transform": "cube-ui/lib/${member}",
	   "preventFullImport": true,
	   "kebabCase": true,
	   "style": true
	 }
  }]
]
```