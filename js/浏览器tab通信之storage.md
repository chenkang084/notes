---
title: note title
tags: tag1, tag2, tag3
notebook: notebook
---

# 浏览器tab通信之storage
@(js)

```javascript
window.addEventListener("storage", function (e) {
    alert('..............')
    //do something
},false);


//兼容写法
if (window.addEventListener) {
    window.addEventListener("storage", handle_storage, false);
} else {
    window.attachEvent("onstorage", handle_storage);
};
function handle_storage(e) {
    if (!e) { e = window.event; }
    //响应代码部分
    ...
}
```

在支持html5 接口的浏览器中，通过监听**storage**，可以实现浏览器tab直接的通信。
>tips：
> - domain 要一致；
> - 有兼容性问题；
> - 修改自身页面修改storage后没有触发window的storage事件, 但是同时访问A.html和B.html, 在A页面中进行 setItem能触发B页面中window的storage事件, 同样的在B页面中进行setItem能触发A页面中window的storage事件。
>  - key 变化，或者value变化才能触发事件。
