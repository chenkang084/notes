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

