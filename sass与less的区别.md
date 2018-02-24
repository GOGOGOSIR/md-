# **sass与less的区别**

1. **定义变量的命名方式**

   **sass是用 $**来命名变量，eg：$highlight-color: #F90,

   less是通过@来命名,eg:@nice-blue: #5B83AD;

# sass与less的特有之处

****

**vue引入sass**

安装

`npm install node-sass --save-dev`

`npm install sass-loader --save-dev`

打开webpack.base.config.js在loaders里面加上  module -- rules (vue2.0)

```
 {
   test: /\.scss$/,
   loaders: ["style", "css", "sass"]
 },
```

在style中写上 lang ='scss'

**sass中特有**

1. sass中有一个父选择器的标识符&，用来代指父选择器

   应用场景

   ​	article a {
    		 color: blue;
     		:hover { color: red }
   ​	}

   这意味着`color: red`这条规则将会被应用到选择器`article a :hover`，`article`元素内链接的所有子元素在被`hover`时都会变成红色。这是不正确的！你想把这条规则应用到超链接自身，而后代选择器的方式无法帮你实现。

   解决之道为使用一个特殊的`sass`选择器，即父选择器。在使用嵌套规则时，父选择器能对于嵌套规则如何解开提供更好的控制。它就是一个简单的`&`符号，且可以放在任何一个选择器可出现的地方，比如`h1`放在哪，它就可以放在哪。

   article a {
     color: blue;
     &:hover { color: red }
   }

   当包含父选择器标识符的嵌套规则被打开时，它不会像后代选择器那样进行拼接，而是`&`被父选择器直接替换

2. 同层相邻组合选择器`+`选择`header`元素后紧跟的`p`元素

3. 同层全体组合选择器`~`，选择所有跟在`header`后的同层`p`元素，不管它们之间隔了多少其他元素

4. 导入SASS文件与less中定义公共类很相似

   引入方式@import,它与传统@import引入方式的区别在于

   ​	传统方式的@import引入只有执行到`@import`时，浏览器才会去下载其他`css`文件，这导致页面加载起来特别慢，而sass中的@import则是在生成`css`文件时就把相关文件导入进来

   应用，比如我在外面编写一个名为minix.sass文件用来包含所有公用的方法

   ```
   $blue: #3190e8;
   $bc: #e4e4e4;
   $fc:#fff;

   // 背景图片地址和大小
   @mixin setBg($url) {
     background-image: url($url);
     background-repeat: no-repeat;
     background-size: 100% 100%;
   }

   @mixin setBorderRadius($radius) {
     -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
     -ms-border-radius: $radius;
     -o-border-radius: $radius;
     border-radius: $radius;
   }

   //定位全屏
   @mixin allcover {
     position: absolute;
     top: 0;
     right: 0;
   }

   //定位上下左右居中
   @mixin center {
     position: absolute;
     top: 50%;
     left: 50%;
     transform: translate(-50%, -50%);
   }

   //定位上下居中
   @mixin center_y {
     position: absolute;
     top: 50%;
     transform: translateY(-50%);
   }

   //定位左右居中
   @mixin center_x {
     position: absolute;
     left: 50%;
     transform: translateX(-50%);
   }

   //宽高
   @mixin setWH($width, $height) {
     width: $width;
     height: $height;
   }

   //字体大小、行高、字体
   @mixin setFont($size, $line-height, $family: 'Microsoft YaHei') {
     font: #{$size}/#{$line-height} $family;
   }

   //字体大小，颜色
   @mixin setFontSizeAndColor($size, $color) {
     font-size: $size;
     color: $color;
   }

   //flex 布局和 子元素 对其方式
   @mixin setflex($type1, $type2) {
     display: flex;
     justify-content: $type1;
     align-items: $type2;
   }

   //文字多余的用.代替
   @mixin textHidden() {
     overflow: hidden;
     text-overflow: ellipsis;
     white-space: nowrap;
   }

   //多行文字多余的用.代替
   @mixin textHidden_p($num, $height) {
     display: -webkit-box;
     -webkit-line-clamp: $num;
     -webkit-box-orient: vertical;
     line-height: $height;
     overflow: hidden;
     text-overflow: ellipsis;
   }
   //默认宽度100%，高度自己给，设置box-size， 和padding
   @mixin setCurrentWh($width,$height,$padding){
     width:$width;
     height:$height;
     box-sizing: border-box;
     padding: 0 $padding;
   }

   ```

   ```
   @import '../../../style/mixin';
   ```

   ```
   @include setFontSizeAndColor(0.32rem, #C2C2C3);
   ```

5. 如果你的整个网站中有几处小小的样式类似（例如一致的颜色和字体），那么使用变量来统一处理这种情况是非常不错的选择,sass提供了一种混合器的东西 @minxin，可以通过@include来引用某个具体的混合器，也可以给被引用的混合器传递参数

   ```
   @mixin rounded-corners {
     -moz-border-radius: 5px;
     -webkit-border-radius: 5px;
     border-radius: 5px;
   }
   ```

6. 使用选择器继承来精简CSS，写法@extend

   ```
   //通过选择器继承继承样式
   .error {
     border: 1px red;
     background-color: #fdd;
   }
   .seriousError {
     @extend .error;
     border-width: 3px;
   }
   ```

   在上边的代码中，`.seriousError`将会继承样式表中任何位置处为`.error`定义的所有样式。以`class="seriousError"` 修饰的`html`元素最终的展示效果就好像是`class="seriousError error"`。相关元素不仅会拥有一个`3px`宽的边框，而且这个边框将变成红色的，这个元素同时还会有一个浅红色的背景，因为这些都是在`.error`里边定义的样式。

   任何跟`.error`有关的组合选择器样式也会被`.seriousError`以组合选择器的形式继承，如下代码:

   ```
   //.seriousError从.error继承样式
   .error a{  //应用到.seriousError a
     color: red;
     font-weight: 100;
   }
   h1.error { //应用到hl.seriousError
     font-size: 1.2rem;
   }
   ```


**sass文档**

[http://www.css88.com/doc/sass/#if]()

**vue中使用less**

npm install less less-loader --save

修改webpack.config.js文件，配置loader加载依赖，让其支持外部的less,在原来的代码上添加

```
{
  test: /\.less$/,
  loader: "style-loader!css-loader!less-loader",
},	
```

在style 中加上 lang='less'

**less中特有**

1. 在 LESS 中我们可以定义一些通用的属性集为一个class，然后在另一个class中去调用这些属性可以带参数

   ```
   .border-radius (@radius) {
     border-radius: @radius;
     -moz-border-radius: @radius;
     -webkit-border-radius: @radius;
   }
   ```

   ```
   #header {
     .border-radius(4px);
   }
   ```

   `@arguments`包含了所有传递进来的参数. 如果你不想单独处理每一个参数的话就可以像这样写:

   ```
   .box-shadow (@x: 0, @y: 0, @blur: 1px, @color: #000) {
     box-shadow: @arguments;
     -moz-box-shadow: @arguments;
     -webkit-box-shadow: @arguments;
   }
   .box-shadow(2px, 5px);
   ```

   渲染的最终样式

   ```
     box-shadow: 2px 5px 1px #000;
     -moz-box-shadow: 2px 5px 1px #000;
     -webkit-box-shadow: 2px 5px 1px #000;
   ```

   我们想根据传入的不同的参数呈现不同的效果

   ```
   .mixin (dark, @color) {
     color: darken(@color, 10%);
   }
   .mixin (light, @color) {
     color: lighten(@color, 10%);
   }
   .mixin (@_, @color) {
     display: block;
   }
   ```

   ​

   结果

   ```
   @switch: light;

   .class {
     .mixin(@switch, #888);
   }
   ```

   就会得到下列CSS:

   ```
   .class {
     color: #a2a2a2;
     display: block;
   }
   ```

   如上，`.mixin`就会得到传入颜色的浅色。如果`@switch`设为dark，就会得到深色。

   具体实现如下：

   - 第一个混合定义并未被匹配，因为它只接受`dark`做为首参
   - 第二个混合定义被成功匹配，因为它只接受`light`
   - 第三个混合定义被成功匹配，因为它接受任意值

   只有被匹配的混合才会被使用。变量可以匹配任意的传入值，而变量以外的固定值就仅仅匹配与其相等的传入值。

​       如果先通过if-else的形式判断不同的样式

```
.mixin (@a) when (lightness(@a) >= 50%) {
  background-color: black;
}
.mixin (@a) when (lightness(@a) < 50%) {
  background-color: white;
}
.mixin (@a) {
  color: @a;
}
```

​	when关键字用以定义一个导引序列(此例只有一个导引)。接下来我们运行下列代码：

```
.class1 { .mixin(#ddd) }
.class2 { .mixin(#555) }
```

​	就会得到：

```
.class1 {
  background-color: black;
  color: #ddd;
}
.class2 {
  background-color: white;
  color: #555;
}
```

​	导引中可用的全部比较运算有： **> >= = =< <**。此外，关键字`true`只表示布尔真值，除去关键字true以外的值都被视示布尔假：

```
.truth (@a) when (@a) { ... }
.truth (@a) when (@a = true) { ... }
```

​	导引序列使用逗号‘`,`’—分割，当且仅当所有条件都符合时，才会被视为匹配成功

```
.mixin (@a) when (@a > 10), (@a < -10) { ... }
```

​	导引可以无参数，也可以对参数进行比较运算：

```
@media: mobile;

.mixin (@a) when (@media = mobile) { ... }
.mixin (@a) when (@media = desktop) { ... }

.max (@a, @b) when (@a > @b) { width: @a }
.max (@a, @b) when (@a < @b) { width: @b }
```

​	最后，如果想基于值的类型进行匹配，我们就可以使用is*函式：

```
.mixin (@a, @b: 0) when (isnumber(@b)) { ... }
.mixin (@a, @b: black) when (iscolor(@b)) { ... }
```



1.  `&` 符号的使用—如果你想写串联选择器，而不是写后代选择器，就可以用到`&`了. 这点对伪类尤其有用如 `:hover` 和 `:focus`.

   ```
   .bordered {
     &.float {
       float: left; 
     }
     .top {
       margin: 5px; 
     }
   }
   ```

   渲染后的结果为

   ```
   .bordered.float {
     float: left;  
   }
   .bordered .top {
     margin: 5px;
   }
   ```

2. 任何数字、颜色或者变量都可以参与运算. 来看一组例子:

   ```
   @base: 5%;
   @filler: @base * 2;
   @other: @base + @filler;

   color: #888 / 4;
   background-color: @base-color + #111;
   height: 100% / 2 + @filler;
   ```

3. LESS 提供了一系列的颜色运算函数. 颜色会先被转化成 *HSL* 色彩空间, 然后在通道级别操作:

   ```
   lighten(@color, 10%);     // return a color which is 10% *lighter* than @color
   darken(@color, 10%);      // return a color which is 10% *darker* than @color

   saturate(@color, 10%);    // return a color 10% *more* saturated than @color
   desaturate(@color, 10%);  // return a color 10% *less* saturated than @color

   fadein(@color, 10%);      // return a color 10% *less* transparent than @color
   fadeout(@color, 10%);     // return a color 10% *more* transparent than @color
   fade(@color, 50%);        // return @color with 50% transparency

   spin(@color, 10);         // return a color with a 10 degree larger in hue than @color
   spin(@color, -10);        // return a color with a 10 degree smaller hue than @color

   mix(@color1, @color2);    // return a mix of @color1 and @color2
   ```

   使用

   ```
   @base: #f04615;

   .class {
     color: saturate(@base, 5%);
     background-color: lighten(spin(@base, 8), 25%);
   }
   ```

   你还可以提取颜色信息:

   ```
   hue(@color);        // returns the `hue` channel of @color
   saturation(@color); // returns the `saturation` channel of @color
   lightness(@color);  // returns the 'lightness' channel of @color
   ```

   如果你想在一种颜色的通道上创建另一种颜色，这些函数就显得那么的好用，例如:

   ```
   @new: hsl(hue(@old), 45%, 90%);
   ```

   `@new` 将会保持 `@old`的 *色调*, 但是具有不同的饱和度和亮度.

   ​

4. LESS提供了一组方便的数学函数，你可以使用它们处理一些数字类型的值:

   ```
   round(1.67); // returns `2`
   ceil(2.4);   // returns `3`
   floor(2.6);  // returns `2`
   ```

   如果你想将一个值转化为百分比，你可以使用`percentage` 函数:

   ```
   percentage(0.5); // returns `50%`
   ```

5. 常见的检测函式

   - `iscolor`
   - `isnumber`
   - `isstring`
   - `iskeyword`
   - `isurl`
   - 如果你想判断一个值是纯数字，还是某个单位量，可以使用下列函式：
     - `ispixel`
     - `ispercentage`
     - `isem`

6. less函数的详情的链接地址

   [http://www.bootcss.com/p/lesscss/#guide](percentage(0.5); // returns `50%`)

7. 可以通过@import引入单独的.less文件，less文件名可以省略

   ```
   @import "lib.less";
   @import "lib";
   ```

8. 变量可以用类似ruby和php的方式嵌入到字符串中，像`@{name}`这样的结构:

   ```
   @base-url: "http://assets.fnord.com";
   background-image: url("@{base-url}/images/bg.png");
   ```

9. JavaScript 表达式也可以在.less 文件中使用. 可以通过反引号的方式使用:

   ```
   @var: `"hello".toUpperCase() + '!'`;
   ```

   输出

   ```
   @var: "HELLO!";
   ```

   注意你也可以同时使用字符串插值和避免编译:

   ```
   @str: "hello";
   @var: ~`"@{str}".toUpperCase() + '!'`;
   ```

   输出:

   ```
   @var: HELLO!;
   ```

   它也可以访问JavaScript环境:

   ```
   @height: `document.body.clientHeight`;
   ```

   如果你想将一个JavaScript字符串解析成16进制的颜色值, 你可以使用 `color` 函数:

   ```
   @color: color(`window.colors.baseColor`);
   @darkcolor: darken(@color, 10%);
   ```

   ​

   ​

