# jq数据动态加载后,DOM无法绑定click事件

原因：

​	因为你在动态数据加载之情就已经加载了监听次点击事件的js文件，所以你后来加载的新dom是无法被监听到的

解决方案：

​	采用事件代理的方式解决，代码如下：



    $('.commonContentList').on('click','.applyBtnCommon',function(event) {
            event.preventDefault();
             $('.applyShadow').css({
                display: 'block'
            });
        });