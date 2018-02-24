# 安装

`npm i element-ui -S`

# 配置

- main.js 增加以下内容

```javascript
// 引入ElementUI
// 导入整个UI框架，可能导致文件过大
//import 'element-ui/lib/theme-default/index.css'
//import ElementUI from 'element-ui'
//Vue.use(ElementUI)
// 使用局部引入（缩小vendor文件大小）
import ElementUI from '@/ElementUI/ElementUI'
```

- 可以选择全部调用
- 不过建议用上面的局部调用方法，还需要以下配置

安装插件

`npm install babel-plugin-component -D`

将 .babelrc 修改为：

```
{
  "presets": [
    ["env", { "modules": false }],
    "stage-2"
  ],
  "plugins": ["transform-runtime",["component", [
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-default"
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



在src目录下创建 ElementUI 文件夹，

在ElementUI文件夹下增加 ElementUI.js 文件

ElementUI.js文件内容如下:

```javascript
import Vue from 'vue'
// 导入 组件
import {
  // Pagination,
  // Dialog,
  // Autocomplete,
  // Dropdown,
  // DropdownMenu,
  // DropdownItem,
  // Menu,
  // Submenu,
  // MenuItem,
  // MenuItemGroup,
  // Input,
  // InputNumber,
  // Radio,
  // RadioGroup,
  // RadioButton,
  Checkbox,
  // CheckboxGroup,
  // Switch,
  // Select,
  // Option,
  // OptionGroup,
  // Button,
  // ButtonGroup,
  // Table,
  // TableColumn,
  // DatePicker,
  // TimeSelect,
  // TimePicker,
  // Popover,
  // Tooltip,
  // Breadcrumb,
  // BreadcrumbItem,
  // Form,
  // FormItem,
  // Tabs,
  // TabPane,
  // Tag,
  // Tree,
  // Alert,
  // Slider,
  // Icon,
  // Row,
  // Col,
  // Upload,
  // Progress,
  // Spinner,
  // Badge,
  // Card,
  // Rate,
  // Steps,
  // Step,
  // Carousel,
  // Scrollbar,
  // CarouselItem,
  // Collapse,
  // CollapseItem,
  // Cascader,
  // ColorPicker,
  // Loading,
  // MessageBox,
  // Message
} from 'element-ui'

//调用 组件
  
// Vue.use(Pagination)
// Vue.use(Dialog)
// Vue.use(Autocomplete)
// Vue.use(Dropdown)
// Vue.use(DropdownMenu)
// Vue.use(DropdownItem)
// Vue.use(Menu)
// Vue.use(Submenu)
// Vue.use(MenuItem)
// Vue.use(MenuItemGroup)
// Vue.use(Input)
// Vue.use(InputNumber)
// Vue.use(Radio)
// Vue.use(RadioGroup)
// Vue.use(RadioButton)
Vue.use(Checkbox)
// Vue.use(CheckboxGroup)
// Vue.use(Switch)
// Vue.use(Select)
// Vue.use(Option)
// Vue.use(OptionGroup)
// Vue.use(Button)
// Vue.use(ButtonGroup)
// Vue.use(Table)
// Vue.use(TableColumn)
// Vue.use(DatePicker)
// Vue.use(TimeSelect)
// Vue.use(TimePicker)
// Vue.use(Popover)
// Vue.use(Tooltip)
// Vue.use(Breadcrumb)
// Vue.use(BreadcrumbItem)
// Vue.use(Form)
// Vue.use(FormItem)
// Vue.use(Tabs)
// Vue.use(TabPane)
// Vue.use(Tag)
// Vue.use(Tree)
// Vue.use(Alert)
// Vue.use(Slider)
// Vue.use(Icon)
// Vue.use(Row)
// Vue.use(Col)
// Vue.use(Upload)
// Vue.use(Progress)
// Vue.use(Spinner)
// Vue.use(Badge)
// Vue.use(Card)
// Vue.use(Rate)
// Vue.use(Steps)
// Vue.use(Step)
// Vue.use(Carousel)
// Vue.use(Scrollbar)
// Vue.use(CarouselItem)
// Vue.use(Collapse)
// Vue.use(CollapseItem)
// Vue.use(Cascader)
// Vue.use(ColorPicker)

// Vue.use(Loading.directive)

// Vue.prototype.$loading = Loading.service
// Vue.prototype.$msgbox = MessageBox
// Vue.prototype.$alert = MessageBox.alert
// Vue.prototype.$confirm = MessageBox.confirm
// Vue.prototype.$prompt = MessageBox.prompt
// Vue.prototype.$notify = Notification
// Vue.prototype.$message = Message
```

- 使用方法如上：
- 需要调用哪个就解开注释（需要同时解开导入 和 调用）