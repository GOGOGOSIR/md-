# canvas绘图

#### **绘制基本图形**

**矩形**

绘制矩形有三种方式：

​	fillRect(x, y, width, height)	绘制一个填充的矩形

​	strokeRect(x, y, width, height)	绘制一个矩形边框

​	clearRect(x, y, width, height)	清除指定矩形区域，让清除部分完全透明。

```
function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext) {
    var ctx = canvas.getContext('2d');
    ctx.fillRect(25,25,100,100);
    ctx.clearRect(45,45,60,60);
    ctx.strokeRect(50,50,50,50);
  }
}
```

`rect(*x*, *y*, *width*, *height*)`

绘制一个左上角坐标为（x,y），宽高为width以及height的矩形。

当该方法执行的时候，moveTo()方法自动设置坐标参数（0,0）。也就是说，当前笔触自动重置会默认坐标。

**绘制路径**

绘制路径有一下步骤：

1. 创建路径起始点	（当前路径为空，即调用beginPath()之后，或者canvas刚建的时候，第一条路径构造命令通常被视为是moveTo（））beginPath()

   moveTo() 	设置起点

   lineTo()		绘制一条从当前位置到指定x以及y位置的直线

2. 画出路径       stroke()

3. 闭合路径      （这个方法会通过绘制一条从当前点到开始点的直线来闭合图形。如果图形是已经闭合了的，即当前点为开始点，该函数什么也不做） closePath()

4. 填充路径的内容区域生成实心的图形 （所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。但是调用stroke()时不会自动闭合）      fill()

绘制一个三角形

```
function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext){
    var ctx = canvas.getContext('2d');

    ctx.beginPath();
    ctx.moveTo(75,50);
    ctx.lineTo(100,75);
    ctx.lineTo(100,25);
    ctx.fill();
  }
}
```
**绘制圆弧**

方法	

```
 ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise);	
```

圆弧路径的圆心在 *(x, y)* 位置，半径为* r* ，根据*anticlockwise* （默认为顺时针即为true）指定的方向从 *startAngle* 开始绘制，到 *endAngle* 结束

startAngle和endAngle是弧度制的  弧度制的基本单位为Math.PI

**arc()函数中的角度单位是弧度，不是度数。角度与弧度的js表达式:radians=(Math.PI/180)\*degrees**


    function draw() {
      var canvas = document.getElementById('canvas');
      if (canvas.getContext){
        var ctx = canvas.getContext('2d');
        for(var i=0;i<4;i++){
          for(var j=0;j<3;j++){
            ctx.beginPath();
            var x              = 25+j*50;               // x 坐标值
            var y              = 25+i*50;               // y 坐标值
            var radius         = 20;                    // 圆弧半径
            var startAngle     = 0;                     // 开始点
            var endAngle       = Math.PI+(Math.PI*j)/2; // 结束点
            var anticlockwise  = i%2==0 ? false : true; // 顺时针或逆时针
    
            ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise);
    
            if (i>1){
              ctx.fill();
            } else {
              ctx.stroke();
            }
          }
        }
      }
    }

###### 二次贝塞尔曲线及三次贝塞尔曲线

`quadraticCurveTo(cp1x, cp1y, x, y)`

绘制二次贝塞尔曲线，`cp1x,cp1y`为一个控制点，`x,y为`结束点。

`bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`

绘制三次贝塞尔曲线，`cp1x,cp1y`为控制点一，`cp2x,cp2y`为控制点二，`x,y`为结束点。

注意：如果是多条二次贝塞尔曲线拼接在一起时，上一条的结束点为下一条的起点

    function draw() {
      var canvas = document.getElementById('canvas');
      if (canvas.getContext) {
          var ctx = canvas.getContext('2d');
          // 二次贝尔赛曲线
          ctx.beginPath();
          ctx.moveTo(75,25);
          ctx.quadraticCurveTo(25,25,25,62.5);
          ctx.quadraticCurveTo(25,100,50,100);
          ctx.quadraticCurveTo(50,120,30,125);
          ctx.quadraticCurveTo(60,120,65,100);
          ctx.quadraticCurveTo(125,100,125,62.5);
          ctx.quadraticCurveTo(125,25,75,25);
          ctx.stroke();
     }
    }
    draw()
三次贝塞尔曲线的拼接和上面的类似

    function draw() {
      var canvas = document.getElementById('canvas');
      if (canvas.getContext){
        var ctx = canvas.getContext('2d');
        //三次贝塞尔曲线
        ctx.beginPath();
        ctx.moveTo(75,40);
        ctx.bezierCurveTo(75,37,70,25,50,25);
        ctx.bezierCurveTo(20,25,20,62.5,20,62.5);
        ctx.bezierCurveTo(20,80,40,102,75,120);
        ctx.bezierCurveTo(110,102,130,80,130,62.5);
        ctx.bezierCurveTo(130,62.5,130,25,100,25);
        ctx.bezierCurveTo(85,25,75,37,75,40);
        ctx.fill();
      }
    }
**Path2D**

[`Path2D`](https://developer.mozilla.org/zh-CN/docs/Web/API/Path2D)对象已可以在较新版本的浏览器中使用，用来缓存或记录绘画命令，这样你将能快速地回顾路径

 **填充色彩**

[`fillStyle = color`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/fillStyle)

设置图形的填充颜色。

[`strokeStyle = color`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/strokeStyle)

设置图形轮廓的颜色。

fillStyle实例

```
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  for (var i=0;i<6;i++){
    for (var j=0;j<6;j++){
      ctx.fillStyle = 'rgb(' + Math.floor(255-42.5*i) + ',' + 
                       Math.floor(255-42.5*j) + ',0)';
      ctx.fillRect(j*25,i*25,25,25);
    }
  }
}
```

storkeStyle实例

```
function draw() {
    var ctx = document.getElementById('canvas').getContext('2d');
    for (var i=0;i<6;i++){
      for (var j=0;j<6;j++){
        ctx.strokeStyle = 'rgb(0,' + Math.floor(255-42.5*i) + ',' + 
                         Math.floor(255-42.5*j) + ')';
        ctx.beginPath();
        ctx.arc(12.5+j*25,12.5+i*25,10,0,Math.PI*2,true);
        ctx.stroke();
      }
    }
  }
```

**设置透明度**

globalAlpha = transparencyValue

​	这个属性影响到 canvas 里所有图形的透明度，有效的值范围是 0.0 （完全透明）到 1.0（完全不透明），默认是 1.0

​	`globalAlpha` 属性在需要绘制大量拥有相同透明度的图形时候相当高效。不过，我认为下面的方法可操作性更强一点。

​	因为 `strokeStyle` 和 `fillStyle` 属性接受符合 CSS 3 规范的颜色值，那我们可以用下面的写法来设置具有透明度的颜色。

```
// 指定透明颜色，用于描边和填充样式
ctx.strokeStyle = "rgba(255,0,0,0.5)";
ctx.fillStyle = "rgba(255,0,0,0.5)";
```

globalAlpha实例

```
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  // 画背景
  ctx.fillStyle = '#FD0';
  ctx.fillRect(0,0,75,75);
  ctx.fillStyle = '#6C0';
  ctx.fillRect(75,0,75,75);
  ctx.fillStyle = '#09F';
  ctx.fillRect(0,75,75,75);
  ctx.fillStyle = '#F30';
  ctx.fillRect(75,75,75,75);
  ctx.fillStyle = '#FFF';

  // 设置透明度值
  ctx.globalAlpha = 0.2;

  // 画半透明圆
  for (var i=0;i<7;i++){
      ctx.beginPath();
      ctx.arc(75,75,10+10*i,0,Math.PI*2,true);
      ctx.fill();
  }
}
```

**线型样式**

```
lineWidth = value    线段宽度    设置0 ，负数，infinity，NAN都会被忽略，默认是1
解决： 1.0线宽精度不准确的问题
如果你想要绘制一条从 (3,1) 到 (3,5)，宽度是 1.0 的线条，你会得到像第二幅图一样的结果。实际填充区域（深蓝色部分）仅仅延伸至路径两旁各一半像素。而这半个像素又会以近似的方式进行渲染，这意味着那些像素只是部分着色，结果就是以实际笔触颜色一半色调的颜色来填充整个区域（浅蓝和深蓝的部分）。这就是上例中为何宽度为 1.0 的线并不准确的原因。

要解决这个问题，你必须对路径施以更加精确的控制。已知粗 1.0 的线条会在路径两边各延伸半像素，那么像第三幅图那样绘制从 (3.5,1) 到 (3.5,5) 的线条，其边缘正好落在像素边界，填充出来就是准确的宽为 1.0 的线条。

注意：在这个竖线的例子中，其Y坐标刚好落在网格线上，否则端点上同样会出现半渲染的像素点（但还要注意，这种行为的表现取决于当前的lineCap风格，它默认为butt；您可能希望通过将lineCap样式设置为square正方形，来得到与奇数宽度线的半像素坐标相一致的笔画，这样，端点轮廓的外边框将被自动扩展以完全覆盖整个像素格）。

还请注意，只有路径的起点和终点受此影响：如果一个路径是通过closePath()来封闭的，它是没有起点和终点的；相反的情况下，路径上的所有端点都与上一个点相连，下一段路径使用当前的lineJoin设置（默认为miter），如果相连路径是水平和/或垂直的话，会导致相连路径的外轮廓根据相交点自动延伸，因此渲染出的路径轮廓会覆盖整个像素格。接下来的两个小节将展示这些额外的行样式。

```

```
ctx.lineCap = "butt";   线段末端已方形结束
ctx.lineCap = "round";	线段末端已圆形结束
ctx.lineCap = "square";	 线段末端已方形结束，但是会增加一个宽度和线段相同，高度是线段厚度一半的矩形区域
```

```
线条与线条链接的样式
此属性有3个值： round, bevel and miter。默认值是 miter。注意：如果2个相连部分在同一方向，那么lineJoin不会产生任何效果，因为在那种情况下不会出现连接区域
ctx.lineJoin = "bevel";
在相连部分的末端填充一个额外的以三角形为底的区域， 每个部分都有各自独立的矩形拐角。
ctx.lineJoin = "round";
通过填充一个额外的，圆心在相连部分末端的扇形，绘制拐角的形状。 圆角的半径是线段的宽度。
ctx.lineJoin = "miter";
通过延伸相连部分的外边缘，使其相交于一点，形成一个额外的菱形区域。

例子：
var ctx = document.getElementById('canvas').getContext('2d');
var lineJoin = ['round','bevel','miter'];
ctx.lineWidth = 10;

for (var i = 0; i < lineJoin.length; i++) {
  ctx.lineJoin = lineJoin[i];
  ctx.beginPath();
  ctx.moveTo(-5,5+i*40);
  ctx.lineTo(35,45+i*40);
  ctx.lineTo(75,5+i*40);
  ctx.lineTo(115,45+i*40);
  ctx.lineTo(155,5+i*40);
  ctx.stroke();
}
```

```
限制当两条线相交时交接处最大长度；所谓交接处长度（斜接长度）是指线条交接处内角顶点到外角顶点的长度。
ctx.miterLimit = value; 当给属性赋值时， 0、负数、 Infinity 和 NaN 都会被忽略
只有当 lineJoin 显示为 ">"  时，miterLimit 才有效。边角的角度越小，斜接长度就会越大。为了避免斜接长度过长，我们可以使用 miterLimit 属性。如果斜接长度超过 miterLimit 的值，边角会以 lineJoin 的 " ] " 类型来显示
```
**设置虚线**

```
设置当前线段的样式
void ctx.setLineDash(segments);
参数接受的是一个数组，用来记录线段的长度和线段之间的距离，例如[5,15]则代表线段长是5，线段之间的间距为15，如果设置为[5,15,15],因为个数为奇数它会变成[5,15,15,5,15,15]就相当于线段成都为5，第一段与第二段间距为15，第二条线段长为15，第二与第三之间的间距为5，第三条的长度为15，后面的间距为15，注意：这种在ie下的兼容只有ie11支持

例子
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.setLineDash([5, 15]);

ctx.beginPath();
ctx.moveTo(0,100);
ctx.lineTo(400, 100);
ctx.stroke();
```

```
设置线段的起始偏移量
ctx.lineDashOffset = value;
偏移量是float精度的数字。 初始值为 0.0。

例子
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.setLineDash([4, 16]);
ctx.lineDashOffset = 2;

ctx.beginPath();
ctx.moveTo(0,100);
ctx.lineTo(400, 100);
ctx.stroke();

跑马灯的效果
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");
var offset = 0;

function draw() {
  ctx.clearRect(0,0, canvas.width, canvas.height);
  ctx.setLineDash([4, 2]);
  ctx.lineDashOffset = -offset;
  ctx.strokeRect(10,10, 100, 100);
}

function march() {
  offset++;
  if (offset > 16) {
    offset = 0;
  }
  draw();
  setTimeout(march, 20);
}

march();
```

**渐变**

```
线性渐变
ctx.createLinearGradient(x0, y0, x1, y1);
createLinearGradient 方法接受 4 个参数，表示渐变的起点 (x1,y1) 与终点 (x2,y2)。
创建出 canvasGradient 对象后，我们就可以用 addColorStop 方法给它上色了。
	addColorStop 方法接受 2 个参数，position 参数必须是一个 0.0 与 1.0 之间的数值，表示渐变中颜色所在的相对位置。例如，0.5 表示颜色会出现在正中间。color 参数必须是一个有效的 CSS 颜色值（如 #FFF， rgba(0,0,0,1)，等等）。
	
例子
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

var gradient = ctx.createLinearGradient(0,0,200,0);
gradient.addColorStop(0,"green");
gradient.addColorStop(1,"white");
ctx.fillStyle = gradient;
ctx.fillRect(10,10,200,100);
```

```
径向渐变
ctx.createRadialGradient(x0, y0, r0, x1, y1, r1);
createRadialGradient 方法接受 6 个参数，前三个定义一个以 (x1,y1) 为原点，半径为 r1 的圆，后三个参数则定义另一个以 (x2,y2) 为原点，半径为 r2 的圆。

例子
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  // 创建渐变
  var radgrad = ctx.createRadialGradient(45,45,10,52,50,30);
  radgrad.addColorStop(0, '#A7D30C');
  radgrad.addColorStop(0.9, '#019F62');
  radgrad.addColorStop(1, 'rgba(1,159,98,0)');
  
  var radgrad2 = ctx.createRadialGradient(105,105,20,112,120,50);
  radgrad2.addColorStop(0, '#FF5F98');
  radgrad2.addColorStop(0.75, '#FF0188');
  radgrad2.addColorStop(1, 'rgba(255,1,136,0)');

  var radgrad3 = ctx.createRadialGradient(95,15,15,102,20,40);
  radgrad3.addColorStop(0, '#00C9FF');
  radgrad3.addColorStop(0.8, '#00B5E2');
  radgrad3.addColorStop(1, 'rgba(0,201,255,0)');

  var radgrad4 = ctx.createRadialGradient(0,150,50,0,140,90);
  radgrad4.addColorStop(0, '#F4F201');
  radgrad4.addColorStop(0.8, '#E4C700');
  radgrad4.addColorStop(1, 'rgba(228,199,0,0)');
  
  // 画图形
  ctx.fillStyle = radgrad4;
  ctx.fillRect(0,0,150,150);
  ctx.fillStyle = radgrad3;
  ctx.fillRect(0,0,150,150);
  ctx.fillStyle = radgrad2;
  ctx.fillRect(0,0,150,150);
  ctx.fillStyle = radgrad;
  ctx.fillRect(0,0,150,150);
}
```

**图案样式（填充背景）**

```
是 Canvas 2D API 使用指定的图像 (CanvasImageSource)创建模式的方法。 它通过repetition参数在指定的方向上重复元图像
ctx.createPattern(image, repetition);
参数：
image：
  HTMLImageElement (<img>),
  HTMLVideoElement (<video>),
  HTMLCanvasElement (<canvas>),
  CanvasRenderingContext2D,
  ImageBitmap,
  ImageData,
  Blob.
repetition
  "repeat" (both directions),
  "repeat-x" (horizontal only),
  "repeat-y" (vertical only), 
  "no-repeat" (neither).
如果为空字符串 ('') 或 null (但不是 undefined)，repetition将被当作"repeat"。


使用：
这是一段简单的代码片段，使用 createPattern 方法创建一个指定图像和重复的CanvasPattern 对象。创建完成后，可以使用CanvasPattern.setTransform()方法对图案进行变形。如示例所示，你可以把此模式赋值给当前的fillStyle，当你使用fillRect() 方法时，会在 canvas 上绘制出效果。


例子
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

var img = new Image();
img.src = 'https://mdn.mozillademos.org/files/222/Canvas_createpattern.png';
img.onload = function() {
  var pattern = ctx.createPattern(img, 'repeat');
  ctx.fillStyle = pattern;
  ctx.fillRect(0,0,400,400);
};

或者填充一个三角形
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

var img = new Image();
img.src = 'https://mdn.mozillademos.org/files/222/Canvas_createpattern.png';
img.onload = function() {
  var pattern = ctx.createPattern(img, 'repeat');
  ctx.fillStyle = pattern;
  ctx.beginPath();
  ctx.moveTo(0,0);
  ctx.lineTo(0,300);
  ctx.lineTo(800,0);
  ctx.fill();
};
```

**阴影**

```
ctx.shadowOffsetX = offset;
ctx.shadowOffsetY = offset;
用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0
ctx.shadowBlur = level;
描述模糊效果程度的，float 类型的值。默认值是 0。 负数、 Infinity 或者 NaN 都会被忽略。
ctx.shadowColor = color;
使用 shadowColor 属性设置阴影的颜色


例子：
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');

  ctx.shadowOffsetX = 2;
  ctx.shadowOffsetY = 2;
  ctx.shadowBlur = 2;
  ctx.shadowColor = "rgba(0, 0, 0, 0.5)";
 
  ctx.font = "20px Times New Roman";
  ctx.fillStyle = "Black";
  ctx.fillText("Sample String", 5, 30);
}
```

**填充文本**

```
填充文本的方法：
ctx.fillText(text, x, y [, maxWidth]);
参数：
text:文本内容
x,y:文本的位置
maxWidth: 文本的最大宽度

绘制文本
ctx.strokeText(text, x, y [, maxWidth]);
参数的作用域上类似


关于设置文本的一些方法
ctx.font = value;			类似已css的font方法，用来设置文本的字体类型

ctx.textBaseline = "top" || "hanging" || "middle" || "alphabetic" || "ideographic" || "bottom";
文本的基线对齐属性
top
文本基线在文本块的顶部。
hanging
文本基线是悬挂基线。
middle
文本基线在文本块的中间。
alphabetic
文本基线是标准的字母基线。
ideographic
文字基线是表意字基线；如果字符本身超出了alphabetic 基线，那么ideograhpic基线位置在字符本身的底部。
bottom
文本基线在文本块的底部。 与 ideographic 基线的区别在于 ideographic 基线不需要考虑下行字母。

ctx.textAlign = "left" || "right" || "center" || "start" || "end";    文本的对齐方式
这里的textAlign="center"比较特殊。textAlign的值为center时候文本的居中是基于你在fillText的时候所给的x的值，也就是说文本一半在x的左边，一半在x的右边，start则是文本对齐界线开始的地方

ctx.direction = "ltr" || "rtl" || "inherit"; 文本的文字方向
ltr表示从左到右
rtl表示从右到左



ctx.measureText(text);    文本测量属性

例子
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

var text = ctx.measureText("foo"); // TextMetrics object
text.width; // 16;
```

**使用图像**

引入图像到canvas里需要以下两步基本操作：

1. 获得一个指向[`HTMLImageElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLImageElement)的对象或者另一个canvas元素的引用作为源，也可以通过提供一个URL的方式来使用图片（参见[例子](http://www.html5canvastutorials.com/tutorials/html5-canvas-images/)）
2. 使用`drawImage()`函数将图片绘制到画布上

引入图片的方式

```
1.通过创建image的方式来引用图片
  var img = new Image();   // 创建一个<img>元素
  img.src = 'myImage.png';// 设置图片源地址
  此时图片开始加载，如此时就调用drawImage的话是无效的，因为图片可能还没有加载完毕
  img.onload = function(){
    // 执行drawImage语句
  }
2.通过 data: url 方式嵌入图像
	img.src = 'data:image/gif;base64,R0lGODlhCwALAIAAAAAA3pn/ZiH5BAEAAAEALAAAAAALAAsAAAIUhA+hkcuO4lmNVindo7qyrIXiGBYAOw==';
3.使用视屏帧
  function getMyVideo() {
    var canvas = document.getElementById('canvas');
    if (canvas.getContext) {
      var ctx = canvas.getContext('2d');

      return document.getElementById('myvideo');
    }
  }
  它将为这个视频返回HTMLVideoElement对象，正如我们前面提到的，它可以作为我们的Canvas图片源。
```

绘制图片

```
drawImage(image, x, y)
其中 image 是 image 或者 canvas 对象，x 和 y 是其在目标 canvas 里的起始坐标。

例子
function draw() {
    var ctx = document.getElementById('canvas').getContext('2d');
    var img = new Image();
    img.onload = function(){
      ctx.drawImage(img,0,0);
      ctx.beginPath();
      ctx.moveTo(30,96);
      ctx.lineTo(70,66);
      ctx.lineTo(103,76);
      ctx.lineTo(170,15);
      ctx.stroke();
    }
    img.src = 'images/backdrop.png';
  }
这种方法不能作为图片背景使用

drawImage(image, x, y,width,height)
width和height分别是用来控制图片的大小

例子：利用此方式做图片的水平和垂直的平铺效果
function draw() {
  var ctx = document.getElementById('canvas').getContext('2d');
  var img = new Image();
  img.onload = function(){
    for (var i=0;i<4;i++){
      for (var j=0;j<3;j++){
        ctx.drawImage(img,j*50,i*38,50,38);
      }
    }
  };
  img.src = 'https://mdn.mozillademos.org/files/5397/rhino.jpg';
}

 ctx.drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
 sx, sy, sWidth, sHeight 前4个是定义图像源的切片位置和大小，  
 dx, dy, dWidth, dHeight 后4个则是定义切片的目标显示位置和大小
 
 eg
 window.onload=function(){
    function draw() {
      var ctx = document.getElementById('canvas').getContext('2d');
      ctx.drawImage(document.getElementById('imgbox'),0,0,250,390);
      ctx.drawImage(document.getElementById('headerImg'),
                  120,59,556,354,9,79,248,210);
    } 
    draw();  
  }
 
```

**保存与恢复**

```
ctx.save();和ctx.restore();
这两个方法是用来保存和恢复 canvas 状态的，都没有参数。Canvas 的状态就是当前画面应用的所有样式和变形的一个快照
```

**移动**

```
先介绍 translate 方法，它用来移动 canvas 和它的原点到一个不同的位置。
translate 方法接受两个参数。x 是左右偏移量，y 是上下偏移量，

注意
在做变形之前先保存状态是一个良好的习惯。大多数情况下，调用 restore 方法比手动恢复原先的状态要简单得多。又，如果你是在一个循环中做位移但没有保存和恢复 canvas 的状态，很可能到最后会发现怎么有些东西不见了，那是因为它很可能已经超出 canvas 范围以外了。
```

**旋转**

```
rotate(angle)
这个方法只接受一个参数：旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值。
旋转的中心点始终是 canvas 的原点，如果要改变它，我们需要用到 translate 方法。
```

**缩放**

```
scale(x, y)
scale 方法接受两个参数。x,y 分别是横轴和纵轴的缩放因子，它们都必须是正值。值比 1.0 小表示缩小，比 1.0 大则表示放大，值为 1.0 时什么效果都没有。
默认情况下，canvas 的 1 单位就是 1 个像素。举例说，如果我们设置缩放因子是 0.5，1 个单位就变成对应 0.5 个像素，这样绘制出来的形状就会是原先的一半。同理，设置为 2.0 时，1 个单位就对应变成了 2 像素，绘制的结果就是图形放大了 2 倍。
```

**变形**

```
transform(m11, m12, m21, m22, dx, dy)
```