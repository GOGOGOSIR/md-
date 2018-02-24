# better-scroll使用关键

1. 在vue中要实现上拉加载，下拉刷新的时候，我们在初始化better-scroll实例是，应做到如下

   ```
   <template>
     <div class="wrapper" ref="wrapper">
       <ul class="content">
         <li v-for="item in data">{{item}}</li>
       </ul>
     </div>
   </template>
   <script>
     import BScroll from 'better-scroll'
     export default {
       data() {
         return {
           data: []
         }
       },
       created() {
         requestData().then((res) => {
           this.data = res.data
           this.$nextTick(() => {
             this.scroll = new Bscroll(this.$refs.wrapper, {})
           })
         })
       }
     }
   </script>
   ```

   这里的 this.$nextTick 是一个异步函数，为了确保 DOM 已经渲染，此时我们绑定的dom已经渲染出来了，我们可以正确计算它以及它内层 content 的高度，以确保滚动正常

   ​

2. 上拉加载的一个模板

   ```
   <template>
     <div class="wrapper" ref="wrapper">
       <ul class="content">
         <li v-for="item in data">{{item}}</li>
       </ul>
       <div class="loading-wrapper"></div>
     </div>
   </template>
   <script>
     import BScroll from 'better-scroll'
     export default {
       data() {
         return {
           data: []
         }
       },
       created() {
         this.loadData()
       },
       methods: {
         loadData() {
           requestData().then((res) => {
             this.data = res.data.concat(this.data)
             this.$nextTick(() => {
               if (!this.scroll) {
                 this.scroll = new Bscroll(this.$refs.wrapper, {})
                 this.scroll.on('touchend', (pos) => {
                   // 下拉动作
                   if (pos.y > 50) {
                     this.loadData()
                   }
                 })
               } else {
                 this.scroll.refresh()
               }
             })
           })
         }
       }
     }
   </script>
   ```

   这段代码比之前稍微复杂一些, 当我们在滑动列表松开手指时候， better-scroll 会对外派发一个 touchend 事件，我们监听了这个事件，并且判断了 pos.y > 50（我们把这个行为定义成一次下拉的动作）。如果是下拉的话我们会重新请求数据，并且把新的数据和之前的 data 做一次 concat，也就更新了列表的数据，那么数据的改变就会映射到 DOM 的变化。需要注意的一点，这里我们对 this.scroll 做了判断，如果没有初始化过我们会通过 new BScroll 初始化，并且绑定一些事件，否则我们会调用 this.scroll.refresh 方法重新计算，来确保滚动效果的正常。

   这里，我们就通过 better-scroll 配合 Vue，实现了列表的下拉刷新功能，上拉加载也是类似的套路，一切看上去都是 ok 的。但是，我们发现这里写了大量命令式的代码（这一点不是 Vue.js 推荐的），如果有很多类似滚动的组件，我们就需要写很多类似的命令式且重复性的代码，而且我们把数据请求和 better-scroll 也做了强耦合，这些对于一个追求编程逼格的人来说，就不 ok 了

3. 上面的代码中 better-scroll 是作用在外层 wrapper 容器上的，滚动的部分是 content 元素。这里要注意的是，better-scroll 只处理容器（wrapper）的第一个子元素（content）的滚动，其它的元素都会被忽略。

4. 当我门想使用水平滑动的时候，要想保持垂直的原生滚动就必须加上eventPassthrough: 'vertical'这个属性，但是如果你的css在这个div上设置了overflow-x:auto,则会造成卡顿的情况，解决方案这将overflow设置为hidden

5. 安装： npm install better-scroll --save

6. 导入： import BScroll from 'better-scroll'

7. 使用：

   ```
   new BScroll(self.$refs.commonSlider_1, {
               scrollX: true,
               eventPassthrough: 'vertical',
               click: true,
               bounce: true
             })
   ```

   ​