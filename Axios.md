# Axios

如果要在vue项目中使用axios的话：

1.首先肯定是安装并添加到生产环节的依赖

```javascript
cnpm install axios --save
```

2.在`main.js`中引入`axios`

```javascript
import axios from 'axios'
```

3.设置默认属性

```js
// 设置axios 的默认属性
// axios.defaults.withCredentials = false //(跨域时是否使用证书)
// axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded;charset=UTF-8' //(请求头)
axios.defaults.baseURL = (process.env.NODE_ENV == 'development' ? 'http://192.168.1.234:10086/Api/StartAPI/StartAPI' : 'http://www.hhh.com/Api/StartAPI/StartAPI') //(根据开发环境和生产环境使用不同的请求地址)
```

4.然后因为axios并不是vue的插件所以不能用`Vue.use`,但可以将它添加到Vue的原型中

```javascript
//将axios添加到vue的原型中
Vue.prototype.$http = axios
```

这样我们也可以像调用vue-resourece一样在组件中调用axios的方法.

具体方法可以查看官方文档或者翻译过来的中文文档

[中文api](http://www.kancloud.cn/yunye/axios/234845) [github地址](https://github.com/mzabriskie/axios)

5.在组件中调用

```javascript
created(){
        var that = this
		this.$http.get("/api/db")
	    	.then(function (res){
                  console.log(res.data)
	  	})
	},
```

