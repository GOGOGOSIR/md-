# ios流畅滚动和隐藏滚动条

```
-webkit-overflow-scrolling:touch; // 更流畅
// 安卓下是可以隐藏的
  &::-webkit-scrollbar {display: none;}
 // ios只能用将颜色设置我transparents
  &::-webkit-scrollbar-thumb {
    background:transparent;
    border: 0px solid transparent;
  }
```