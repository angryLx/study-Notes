# Something About React

## **[性能优化](https://zh-hans.reactjs.org/docs/optimizing-performance.html)**
同一个组件内部，修改其中一个无关联的值，其他模块也在一并重新渲染，这种通常被称作“无用的”渲染。若渲染肉眼可见的慢，可考虑通过覆盖生命周期的变法 [shouldComponentUpdate](#shouldComponentUpdate) 来进行提速。或者使用  [React.PureComponent](#React-PureComponent) 以代替手写 shouldComponentUpdate()，进行props和state的浅比较覆写了shouldComponentUpdate()的实现。
**_`但需注意，PureComponent是浅比较，props或state可变时，就不能在用它了。`_** 需要结合 `不可变数据` 或 `不可变数据结构` 的库来追踪对象的变化，这是应用 shouldComponentUpdate 所需要的。

不可改变树支持ES6的 扩展运算符 或 Object.assign 。不可变数据结构可用：`Immutable.js、Immer， immutability-helper 以及 seamless-immutable。`


#### `shouldComponentUpdate`
若组件只有在props.color和state.count改变时才会重新渲染。
```js
shouldComponentUpdate(nextProps, nextState) {
    if (this.props.color !== nextProps.color) {
      return true;
    }
    if (this.state.count !== nextState.count) {
      return true;
    }
    return false;
  }
```
#### `React.PureComponent`
如果你的组件更复杂一些，你可以使用类似`“浅比较”`的模式来检查 props 和 state 中所有的字段，以此来决定是否组件需要更新。
``` js
class CounterButton extends React.PureComponent {
  constructor(props) {
    super(props);
    this.state = {count: 1};
  }

  render() {
    return (
      <button
        color={this.props.color}
        onClick={() => this.setState(state => ({count: state.count + 1}))}>
        Count: {this.state.count}
      </button>
    );
  }
}
```