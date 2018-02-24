# vuecli分析项目分布情况

## 修改package.json

```js
{
  ...
  "scripts": {
    ...
    //新增
    "analyz": "NODE_ENV=production npm_config_report=true npm run build",
    ...
  },
  ...
}
```

## 启动

`npm run analyz`

注意

在window下如果直接这样写会报NODE_ENV不是内部或外部的错误

解决方式

首先先安装across-env:`npm install cross-env --save-dev`

然后在package.json中配置

```
{
  ...
  "scripts": {
    ...
    //新增
    "analyz": "cross-env NODE_ENV=production npm_config_report=true npm run build"
    ...
  },
  ...
}
```
只能启动一个，否则会报错