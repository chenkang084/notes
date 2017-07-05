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

- routers 目录里相当于container容器，被connect的组件都可以视为容器；
> connect过的组件就可以使用dispatch 方法，如果子组件需要使用dispatch方法，则需要通过属性传递的方式进行调用。

- saga-redux的select
> select 方法是为了获取Store state 上的数据（例如，返回 selector(getState(), ...args) 的结果）。selector: Function - 一个 (state, ...args) => args 函数. 通过当前 state 和一些可选参数，返回当前 Store state 上的部分数据。<br>

> 如果 select 调用时参数为空（即 yield select()），那 effect 会取得整个的 state（和调用 getState() 的结果一样）。