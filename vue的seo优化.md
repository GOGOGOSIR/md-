# vue的seo优化

**预渲染**

 	定义：就是当vue-cli构建的项目进行npm run build 的时候，会按照路由的层级进行动态渲染出对应的html文件

​	解决的问题：

​          	**1.搜索引擎优化 :** 当内容异步加载，爬虫不会等待加载

​		2.**速度慢的客户端**：当用户访问您的网站的互联网连接不良时，您希望能够尽快显示他们的内容，甚至在所有的JS被下载和解析之前

​	不足的地方：

- 不能很好地处理用户独特性路由: 比如有个路由是 /my-profile, 预渲染可能不会很好用, 因为这个内容页是根据用户信息变化的，所以页面内容也不是唯一确定的. 你可能会使用类似于这样的路由路径 /users/:username/profile,但是这样也是不合适的.
- 经常变动的文件
- 需要预渲染成千上万的路由文件: 这个可能会导致你编译时间.....额，可能你会编译很长时间

安装：

npm install prerender-spa-plugin --save

在webpack.base.conf.js中添加

```
var path = require('path')
var PrerenderSpaPlugin = require('prerender-spa-plugin')

module.exports = {
  // ...
  plugins: [
    new PrerenderSpaPlugin(
      // 编译后的html需要存放的路径
      path.join(__dirname, '../dist'),
      // 列出哪些路由需要预渲染
      [ '/', '/about', '/contact' ]
    )
  ]
}
```



​	



