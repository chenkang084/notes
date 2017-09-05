# react 学习笔记
@(react)
## 1.state与props
- props是只读属性，只有在组件被**实例化**的时候可以赋值，之后的任何时候都无法改变该值。如果试图修改该值时，控制台会报错 only read。
- state与props正好相反，state中保存可变的值。通过this.setState()方法修改对应的值。使用state必须通过es6继承React.Component 类（官方推荐写法），并在构造函数内进行初始化。
```javascript
export default class BoubleBind extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            textVal: ''
        }
    }
}
```
具体参考[官方详解](https://facebook.github.io/react/docs/state-and-lifecycle.html)。
### 1.1 react state 与redux state
react 的state是react自己管理组件状态的工具，适用于该组件管理自己这一层级的状态，通过继承React.Component 类，在构造函数**constructor**中进行初始化，在各自事件中通过setState方法更新state值。

state的值只能在本组件内通过setState方法修改，state向下组件传递时，作为**子组件**的props，props为只读属性，子组件不能**直接**更新该值。父组件修改state的值会影响（反映/更新）到子组件，子组件将执行render方法。
子组件如果需要更新父组件的state值，只能通过props.callback调用父组件的方法，在父组件方法中执行setState方法更新state值，父组件更新state值，会反映到子组件上。

当组件层级比较多的时候，通过回调的方式更新父组件state来改变各级响应，这种方式写起来要经过多次传递，比较容易写错，这个时候，就可以考虑使用redux进行通信。redux可以理解为一个全局的状态管理工具，主要用于组件之间相互通信，尤其是没有关联关系组件之间的通信。

## 2.react 组件
- 在jsx语法中，自定义组件时，首字母必须大写。
`In JSX, lower-case tag names are considered to be HTML tags. `

自定义组件的方法：

```
import React from 'react';

export default (props) =>{
  console.log('xxxxxxxxxxxxxxxvvvvvvvv11')
  console.log(props.children)
  return (
    <div>
      test
    </div>
  )
}

```
类方法定义组件：
```
export default class Mytest extends React.Component{
  constructor(props){
    super(props)
    console.log(this)
  }

  render(){
    return (
      <div>
        test
      </div>
    )
  }
}
```
类方法定义组件，可以使用setState相关方法。

## 3.react 组件生命周期
通过反复试验，得到了组件的生命周期在不同状态下的执行顺序：

当首次装载组件时，按顺序执行 getDefaultProps、getInitialState、componentWillMount、render 和 componentDidMount；

当卸载组件时，执行 componentWillUnmount；

当重新装载组件时，此时按顺序执行 getInitialState、componentWillMount、render 和 componentDidMount，但并不执行 getDefaultProps；

当再次渲染组件时，组件接受到更新状态，此时按顺序执行 componentWillReceiveProps、shouldComponentUpdate、componentWillUpdate、render 和 componentDidUpdate。

参考文章 [React 源码剖析系列 － 生命周期的管理艺术](https://zhuanlan.zhihu.com/p/20312691)

> 重点说明：
  不建议在 getDefaultProps、getInitialState、shouldComponentUpdate、componentWillUpdate、render 和 componentWillUnmount 中调用 setState，特别注意：不能在 shouldComponentUpdate 和 componentWillUpdate中调用 setState，会导致循环调用。

  - componentWillUpdate 
  不能在这个方法里面修改state，可能会造成死循环，该方法调用的时候，其实还没有接收到 **变化后**的state或者props。可以在该方法里面做一些动画操作，[Stack Overflow](https://stackoverflow.com/questions/33075063/what-is-the-exact-usage-of-componentwillupdate-in-reactjs)

  - componentDidUpdate
执行该方式的时候，已经获取到了**变化后**的state或者props,可以在该方法中更新state，但是，最好加入 限定条件，否则也会陷入死循环。