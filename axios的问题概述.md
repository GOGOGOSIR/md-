## axios的问题概述，开发与生产环境的部署

遇到的问题如下：

1. 对post 请求 会将请求方式变为 option

   原因：当你的 ajax 的请求为非简单请求时，浏览器会进行预检，即发送 OPTIONS 请求到服务器，询问是否允许跨域。如果响应中允许你的预检中请求的跨域行为，则浏览器会进行真正的请求。否则，会报 405 错误

   解决方案：

   设置拦截器, 把content-type 设置成application/x-www-form-urlencoded, 降级为简单请求, 就不会发送options请求, 注意如果是post方法, 需要qs模块转换一下参数

2. 对于post请求， 传递的参数会默认转化为字符串的形式

   解决方案：

   ​	axios内部集成了一个qs的方法库，用于将字符串对象转化为对象

   ​	首先我们需要引入 qs 模块， import qs from 'qs'

   ​	然后在request的拦截器中的config.data = qs.stringify({...config.data})

3. 注意，如果我们用axios（{}）的方式去请求时，对于get请求传参数是用params：{}这个字段

   而对于post请求传参则是用data： {}这个字段，这就解释了上面的config.data的意义

4. 然而在实际的项目中 我一般会对axios进行统一的配置

   ```
   import axios from 'axios'
   import qs from 'qs';
   //建立一个单独的fetch.js封装axios请求并作为方法暴露出来
   // 创建axios实例
   const service = axios.create({
     baseURL: process.env.API_HOST, // node环境的不同，对应不同的baseURL
     timeout: 5000, // 请求的超时时间
     //设置默认请求头，使post请求发送的是formdata格式数据
     // axios的header默认的Content-Type好像是'application/json;charset=UTF-8',
     //我的项目都是用json格式传输，如果需要更改的话，可以用这种方式修改
     // headers: {  
     //   "Content-Type": "application/x-www-form-urlencoded"
     // },
     withCredentials: true // 允许携带cookie
   })
   // request拦截器
   service.interceptors.request.use(
     config => {
       // 发送请求之前，要做的业务
       // 将验证服务器的条件设置宽松一点 解决post请求跨域
       config.headers['Content-Type'] = 'application/x-www-form-urlencoded'
       if(config.method === 'post') {
           config.data = qs.stringify({
               ...config.data
           })
       }
       return config
     },
     error => {
       // 错误处理代码
       return Promise.reject(error)
     }
   )

   // response拦截器
   service.interceptors.response.use(
     response => {
       // 数据响应之后，要做的业务
       return response
     },
     error => {
       return Promise.reject(error)
     }
   )
   /*
   // 发送请求前处理request的数据
   axios.defaults.transformRequest = [function (data) {
     let newData = ''
     for (let k in data) {
       newData += encodeURIComponent(k) + '=' + encodeURIComponent(data[k]) + '&'
     }
     return newData
   }]
   */
   export default service


   使用： 
   import _axios from 'api/fetch'
   import {address, ERR_OK} from 'api/config'

   export function getListDesc() {
   	let data = {
         type: `hot`,
         fields: `list[].desc`
       }
       return new Promise((resolve, reject) => {
       	_axios({
       	  method: 'get',
   		  url: '/member/materials.json',
   		  params: data
   		})
   	    .then((res) => {
   	    	if (res.data.error_code === ERR_OK) {
   	    		resolve(res.data.data) 
   	    	}
   	    }).catch((e) => {
             console.log(e)
           })
       })
   }

   export function regsNewUser (telephone, code) {	
   	const data = {
         	telephone: telephone,
        	verification_code: code
       }
       return new Promise((resolve, reject) => {
       	_axios({
       		method: 'post',
       		url: '/member/brokers.json',
       		data: data
       	})
       	.then((res) => {
   	    	if (res.data.error_code === ERR_OK) {
   	    		resolve(res) 
   	    	}
   	    }).catch((e) => {
             console.log(e)
           })
       })
   }
   ```

5. ajax的跨域的问题

   解决方案，使用vue的代理去解决

   在config的index.js中的dev对象内，进行如下的配置

   ```
   dev: {

       // Paths
       assetsSubDirectory: 'static',
       assetsPublicPath: '/',
       proxyTable: {},

       // Various Dev Server settings
       host: 'localhost', // can be overwritten by process.env.HOST
       port: 8080, // can be overwritten by process.env.PORT, if port is in use, a free one will be determined
       autoOpenBrowser: false,
       errorOverlay: true,
       notifyOnErrors: true,
       poll: false, // https://webpack.js.org/configuration/dev-server/#devserver-watchoptions-

       
       /**
        * Source Maps
        */

       // https://webpack.js.org/configuration/devtool/#development
       devtool: 'cheap-module-eval-source-map',

       // If you have problems debugging vue-files in devtools,
       // set this to false - it *may* help
       // https://vue-loader.vuejs.org/en/options.html#cachebusting
       cacheBusting: true,

       cssSourceMap: true,
       // 设置跨域问题
       proxyTable: {
         '/api': {
           target: 'http://test.www.atchuangke.com/v1',//设置你调用的接口域名和端口号 别忘了加http
           changeOrigin: true,
           pathRewrite: {
             '^/api': '/'//这里理解成用‘/api’代替target里面的地址，后面组件中我们掉接口时直接用api代替 比如我要调用'http://40.00.100.100:3002/user/add'，直接写‘/api/user/add’即可
           }
         }
       }
     },
   ```



6. 当我门要切换正式与测试环境的请求接口的时候，我们有两种方式去解决问题

   1. 就是我们定义一个全局的变量 address  但是不推荐这种方式

   2. 我们利用process.env这个玩意，自动去检测你是开发环境还是正式生产环境

      首先我们配置开发环境的配置，在config文件下的dev.env.js

      ```
      'use strict'
      const merge = require('webpack-merge')
      const prodEnv = require('./prod.env')

      module.exports = merge(prodEnv, {
        NODE_ENV: '"development"', // 开发环境
        API_HOST: '"http://test.www.atchuangke.com/v1"'
      })

      ```

      然后在配置正式环境下的配置， 在config文件下的prod.env.js

      ```
      'use strict'
      module.exports = {
        NODE_ENV: '"production"', // 生产环境
        API_HOST: '"http://www.atchuangke.com/v1"'
      }
      ```

   那我们的接口地址可以通过 process.env.API_HOST 来获取

7. 当我们访问某个地址页面时，有时会要求我们重新登录后再访问该页面，也就是身份认证失效了，如token丢失了，或者是token依然存在本地，但是却失效了，所以单单判断本地有没有token值不能解决问题。此时请求时服务器返回的是401错误，授权出错，也就是没有权利访问该页面。 
   我们可以在发送所有请求之前和操作服务器响应数据之前对这种情况过滤。

   ```
   // http request 请求拦截器，有token值则配置上token值
   axios.interceptors.request.use(
       config => {
           if (token) {  // 每次发送请求之前判断是否存在token，如果存在，则统一在http请求的header都加上token，不用每次请求都手动添加了
               config.headers.Authorization = token;
           }
           return config;
       },
       err => {
           return Promise.reject(err);
       });

   // http response 服务器响应拦截器，这里拦截401错误，并重新跳入登页重新获取token
   axios.interceptors.response.use(
       response => {
           return response;
       },
       error => {
           if (error.response) {
               switch (error.response.status) {
                   case 401:
                       // 这里写清除token的代码
                       router.replace({
                           path: 'login',
                           query: {redirect: router.currentRoute.fullPath}//登录成功后跳入浏览的当前页面
                       })
               }
           }
           return Promise.reject(error.response.data) 
   ```