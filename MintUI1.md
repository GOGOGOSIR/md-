# 安装

```nam
npm i mint-ui -S
```

# 配置

## 全部调用

main.js:

```javascript
import Vue from 'vue'
import MintUI from 'mint-ui'	//新增
import 'mint-ui/lib/style.css'	//新增
import App from './App.vue'

Vue.use(MintUI)	//新增

new Vue({
  el: '#app',
  components: { App }
})
```

## 局部调用

安装插件

`npm install babel-plugin-component -D`

main.js:

```javascript
import Vue from 'vue'

// 引入mint-ui 框架样式 (新增)
// 按需加载js (新增)
import MintUI from '@/MintUI/MintUI'

import App from './App.vue'

new Vue({
  el: '#app',
  components: { App }
})
```

将 .babelrc 修改为：

```
{
  "presets": [
    ["env", { "modules": false }],
    "stage-2"
  ],
  "plugins": ["transform-runtime",["component", [
      {
        "libraryName": "mint-ui",
        "style": true
      }
    ]]
  ],
  "comments": false,
  "env": {
    "test": {
      "presets": ["env", "stage-2"],
      "plugins": [ "istanbul" ]
    }
  }
}
```

在 src 目录下创建 MintUI 文件夹

在 MintUI 文件夹先 创建 MintUI.js

```javascript
//必须引入
import Vue from 'vue'

// 调用轮播图
import { Swipe, SwipeItem } from 'mint-ui';

Vue.component(Swipe.name, Swipe);
Vue.component(SwipeItem.name, SwipeItem);

// 调用 提示框
import { MessageBox } from 'mint-ui';

//调用 微标
import { Badge } from 'mint-ui';

Vue.component(Badge.name, Badge);
```

需要调用其他组件，在官网API复制过来就可以