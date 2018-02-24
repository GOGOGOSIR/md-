# 解决ios的音频自动播放的问题

问题描述：

ios 为了节省用户流量，对于 audio 和 video标签的 preload 和 autopaly 标签 会自动拦截，除非用户手动点击 交互才会执行 。  但是对于背景音乐，又必须加载的时候就要执行，怎么办，直接调用js 来自动触发。

```
<audio id="bgaudio" src="./media/bg-music.mp3" loop="loop" autoplay preload></audio>  
<script type="text/javascript">  
   document.getElementById('bgaudio').play();  
</script>  
```

当然有特别的 ios 设置后  不兼容，我们可以采用下面的方式，不过注意只能在微信浏览器里面打开

```
<audio preload="preload" controls id="car_audio" src="./media/bg-music.mp3" loop autoplay preload></audio>  
  
    <script>  
     setTimeout(function(){  
         $(window).scrollTop(1);  
     },0);  
      document.getElementById('car_audio').play();  
      document.addEventListener("WeixinJSBridgeReady", function () {  
            WeixinJSBridge.invoke('getNetworkType', {}, function (e) {  
                document.getElementById('car_audio').play();  
            });  
      }, false);  
    </script>  
```