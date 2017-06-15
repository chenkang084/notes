---
title: script标签defer简记
tags: script defer
notebook: js
---

# script标签defer简记

```    
<script defer type="text/javascript" src="commons/test.js"></script>
<script defer type="text/javascript" src="commons/test2.js"></script>
<script type="text/javascript" src="commons/test3.js"></script>

...

<div>
....
</div>
```

以上代码的执行顺序是：
test3.js,test.js,test2.js。
- 其实，test3.js会阻塞页面dom的渲染，页面执行到test3.js的时候，页面会等待test3.js从服务器下载完，并且立即执行test3.js中的内容，所以会阻塞后面dom内容的渲染。
- test.js,test2.js 不会阻塞页面渲染，当页面执行到test.js,test2.js的时候，浏览器会发出get请求资源，并继续执行后续的渲染，及时这时候资源已经返回，也不会立即执行，等页面的dom加载完毕后才开始执行js。
