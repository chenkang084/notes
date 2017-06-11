---
title: react 中state与props
tags: react
notebook: react
---

# react 中state与props



##  1、redux 执行顺序22
------------------------------
>redux 中 react 组件执行顺序：
1.执行mapStateToProps；
2.render方法；
3.componentDidMount方法。

tips：当props发生改变，会依次执行上述方法。

```javascript
import React, { Component } from 'react';
import { connect } from 'react-redux';
import { fetchIssuesIfNeeded } from '../actions/index.js';
import CellView from '../components/CellView.js';

class All extends Component {
  componentDidMount() {
    const { dispatch } = this.props;
    dispatch(fetchIssuesIfNeeded('created', 10000));
  }
  render() {
    if (this.props.isFetching) {
      return null;
    }

    return (
      <div className="list">
        <CellView title="全部" items={this.props.items} />
      </div>
    );
  }
};

function mapStateToProps(state) {
  const {
    isFetching,
    items
  } = state || {
    isFetching: true,
    items: []
  };

  return {
    isFetching,
    items
  }
}

export default connect(mapStateToProps)(All);
```
###1.1 mapStateToProps 
该方法结合react-redux 的 **connect** 方法，将store 绑定到容器的props上面。
 在该容器上面，如果不执行dispatch方法，默认在conncet 容器之后，会最先执行mapStateToProps 方法，再执行render方法，再执行componentDidMount。
当该容器绑定的state发送改变的时候，以上方法会按顺序一次被执行。

## 2、redux-thunk

redux-thunk 主要是针对异步action操作而设计的中间件。正常情况，dispatch 的参数是一个**Object** 对象。
由于dispatch对reducer的操作都是即时操作（即立即生效的操作），针对异步操作，需要引入redux-thunk 作为中间件（也有其他的中间件可以使用）。
redux-thunk 在执行dispatch的时候，需要传入的是一个函数，该函数可以有2个参数，dispatch 和 getState。

## 3、combineReducers
combineReducers方法的作用就是将各个功能的reducer最后合并成为一个最终的reducer，通过createStore 方法生成store。当系统功能比较复杂，不可能将所有的方法都写在一个reducer文件里面，那样随着系统业务的增长，该文件的代码数量会越来越大，不利于后期维护。
通过这个方法，可以将不同业务的reducer拆分开来。
```javascript
//reducers/index.js
//组合多个独立的reducer
export default combineReducers({
    mynav,
    test
})

//src/index.js
//生成store
const store = createStore(
    reducer,
    applyMiddleware(...middleware)
)
//containers/nav2.js
const Nav2 = ({mynav, test, dispatch}) => {
    dispatch({type: 'test'})
    return (
        <div className="link">
            <Link to="/">全部2</Link>
            <Link to="/">归档2</Link>
            <Link to="/">标签2</Link>
        </div>
    )
}
const mapStateToProps = state => ({
    mynav: state.mynav,
    test: state.test,
})
export default connect(mapStateToProps)(Nav2);
```
以上代码基本上就是一个完整的使用方法。
>tips：在获取各个子reducer对应的state的时候，用**state.mynav**获取对应的state；在使用dispatch的时候，可以直接指定需要switch case 对应的action.type(可能我没有讲清楚，就是获取state 需要指定 对应的模块名，而dispatch调用不需要);

##4.react-router 按需加载


