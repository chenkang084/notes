---
title: antd-admin使用说明
tags: react antd
notebook: react
---

# antd-admin使用说明

## 推荐文章
> [dva 介绍](https://github.com/dvajs/dva/issues/1) <br>
[dva 入门：手把手教你写应用](https://github.com/sorrycc/blog/issues/8) <br>
[React + Redux 最佳实践](https://github.com/sorrycc/blog/issues/1) <br>
[快速上手](https://github.com/dvajs/dva-docs/blob/master/v1/zh-cn/getting-started.md)<br>
[12 步 30 分钟，完成用户管理的 CURD 应用](https://github.com/sorrycc/blog/issues/18)

### 关于dva 中model的理解

- effects 对应于redux 中的异步操作，dva中使用的是[redux-saga](https://github.com/redux-saga/redux-saga)，你也可以使用redux-thunk，都是对异步redux的操作，因为redux中的reducer，默认是同步执行。
> effects 中的函数有2个参数，第一个参数是传入的action，第二个参数是saga 的操作函数，如{ call, put, takeEvery, takeLatest } <br>
异步操作结束，通过调用reducers中的方法同步state，通过action传递异步获取的数据。


- reducers 对应于redux中的reducer。
> reducers的函数有2个参数，第一个参数是当前state状态，第二个参数是action。<br>
reducer 方法必须返回一个state对象。

- subscriptions 有2个作用；1：做页面初始化工作；2：页面事件监听。

官方推荐的count 案例，似乎没有实现 1秒能按多少下，自己实现了一下，顺便学习了一下dva。
<https://github.com/chenkang084/dva-demos>

