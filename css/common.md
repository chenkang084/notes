# css 总结
## 1.网页自适应屏幕高度
header,footer固定高度，main用min-height:calc(100% -(header+footer))
```
<header></header>

<div class="main"></div>

<footer></footer>
```