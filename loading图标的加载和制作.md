# loading图标的加载和制作

推荐两个网站

[[https://icons8.com/preloaders/en/astronomy](常用图标的下载)](常用图标的下载)

[https://loading.io/](常用图标的下载)(常用图标的下载)

判断 资源是否加载完毕常用到一下代码

```
document.onreadystatechange    页面加载状态改变时的事件
document.readyState     返回当前文档的状态
有以下几个状态：
uninitialized  还未开始载入
loading    载入中
interactive  已加载 文档和用户可以开始交互
complete 已加载完成

document.onreadystatechange = function(){
  if(document.readyState == 'complete'){
    // 加载完毕后的状态的变化
  }
}

var loadedNum = 0
for (var i=0; i<list.length; i++){
  var imgObj = new Image()
  imgObj.onload = function (){
  	loadedNum++
    if (loadedNum==list.length){

    }
  }
  imgObj.src = list[i].picurl // 放在后面试因为ie下会因为缓存而报错
}
```

