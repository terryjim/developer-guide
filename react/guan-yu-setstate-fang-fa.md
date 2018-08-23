# 关于setState方法

`setState()`的完整表达式

```
setState(updater, [callback])

```

`setState()`方法会把对组件 state 的改变加入到队列中，并且告诉 React 这个组件及其子组件需要重新渲染。

#### 第一个参数：`updater` 函数

`setState(updater, callback)` 方法的第一个参数是一个固定格式的 `updater` 函数：

```
(prevState, props) =
>
 stateChange

```

`prevState` 是一个对之前状态（previous state）的引用，我们是不能直接修改这个参数的值，要想修改 `state` 的值，我们应该根据 `prevState` 和 `props` 参数来创建一个新的 JavaScript 对象。  
 例如：

```
this.setState((prevState, props) => { return {counter: prevState.counter + props.step};});

```

你也可以传一个对象而不是函数，来作为`setState(updater, callback)` 方法的第一个参数，React 会将该参数 merge 到 state 中。  
 例如：

```
this.setState({quantity: 2});
```

#### 第二个参数：`callback`

`setState(updater, callback)` 方法的第二个参数 `callback` 是一个可选参数。

为了更好的性能表现，React 并不能保证 `setState()` 一被调用 state 就能更新。所以，如果在调用 `setState()` 之后，马上就读取 `this.state` 的值的话，可能会出现误差。 因此，这种情况下，推荐使用 `componentDidUpdate` 或者 `setState(updater, callback)` 方法的 `callback` 来获取最新的状态。React 官方更推荐使用 `componentDidUpdate()`，而不是 `callback` 来监听 update 事件（注： 除非 `shouldComponentUpdate()` 方法返回 `false`，`setState()` 将永远都会引发重新渲染）。  


#### 状态更新可能是异步的

考虑到性能问题，如果在同一个周期内，调用了多次，React 可能会将多个 `setState()` 方法的调用批量合成一次更新。比如，你在一个周期内，对一个数值进行多次累加，就会出现类似于下面的这种情况：

```
Object.assign(previousState,  {quantity: state.quantity + 1},  {quantity: state.quantity + 1},  ...)

```

这也就意味着，后面的调用会覆盖掉上一次调用后的修改的 state 值，因此 quantity 只累加了 1 次。

考虑到 `this.props` 和`this.state` 可能是异步更新的，所以，每次调用 `setState()` 方法时，最好不要依赖于 `this.props` 和`this.state` 来计算最新的 `state`。

总之，如果后面的状态依赖于之前的状态，建议使用 `updater` 函数：

```
// Correct
this.setState((prevState, props) => {return {quantity: prevState.quantity + 1};});

```

上面用的是 [箭头函数](https://link.jianshu.com?t=https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) 的形式，其实用普通的函数也是一样的效果：

```
// Correct
this.setState(function(prevState, props) {return {quantity: prevState.quantity + 1;  };});

```

#### 状态更新是一个合并的过程

当你调用 `setState()` 方法时，React 会将将你当前所提供的对象合并到当前的状态中。  
 例如：

```
constructor(props) {    
    super(props);    
    this.state = {      
        posts: [],      
        comments: []
    };
}

componentDidMount() {
    fetchComments().then(response => {this.setState({comments: response.comments});});
}

```

在上面👆的例子中， state 的更新只是替换了 `comments`，`posts`是不会受到任何影响的。

  


  




