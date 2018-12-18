# redux基础概念

## 1、Action

Action本质上是javascript普通对象，使用 type字段表示要执行的动作（一般被定义为字符串常量），除此外，action对象结构可随意组成。Action是store的唯一数据来源。

```
{type:'INCREMENT',...}
```

## 2、Reducer

Reducer是形式为\(state,action\)=&gt;state的纯函数，通过传递action修改state的值

```
const counter=(state=0,action)=>{
  switch(action.type){
    case 'INCREMENT':
      return state+1
    case 'DECREMENT':
      return state-1
    default:
      return state
    }
  }
```

纯函数定义：一个函数的返回结果只依赖于它的参数，并且在执行过程里面没有副作用，我们就把这个函数叫做纯函数。

纯函数不应访问外部变量，与外界交换数据只有一个唯一渠道－－参数及返回值，永远不要在reducer中做以下操作：

* 修改传入的参数
* 执行有副作用（Side effects）的操作，如API请求和路由跳转
* 调用非纯函数，如Date.now\(\)或Match.random\(\)

即任何时候传弟的参数一样结果一定一样，不会受其它因素影响。

因此在上述reducer中切记不要修改state参数，通常采用Object.assign或直接使用Immutable.js生成的不可娈数据类型。

## 3、Store

Store是个全局对象，将action、reducer和state联系在一起，负责更新、查询、订阅state等工作。Store有以下功能：

* 维持应用的state
* 提供getState\(\)方法获取state
* 提供dispatch\(action\)方法更新state
* 通过subscribe\(listener\)注册监听器

```
import {createStore } from 'redux'
const store = createStore(counter)
let currentValue=store.getState()    //获取初始化state到currentValue
store.subscribe(()=>{     //注册监听器
  const previousValue=currentValue      //初始 state值
  currentValue=store.getState()      //执行action后修改了state值，即获取当前state
  console.log('pre state:',previousValue,'next state:',currentValue)     //打印action执行前后的state值
}


……。……。…。…。……。……。…………。
//发起action
store.dispatch({type:'INCREMENT'})
store.dispatch({type:'DECREMENT'})
```

## 4、Action创建函数

Action创建函数（action creator）即创建action对象的函数

```
const increment = () => ({type:'INCREMENT',...})
const decrement = () => ({type:'DECREMENT',...})
```

有了Action创建函数，store调用：

```
store.dispatch(increment())
store.dispatch(decrement())
```

## 5、Redux Thunk中间件

Redux Thunk中间件可以让Action创建函数先不返回action对象，而是返回一个函数。通过这个函数延迟dispatch或者只在满足指定条件的情况下dispatch。这个内部函数接受store的两个方法**dispatch、getState**作为参数。

使用redux thunk需要安装`npm install -save redux-thunk`

使用时引入thunk和applyMiddleware，并在createStore中激活

```
import {createStore,applyMiddleware } from 'redux'
import thunk from 'redux-thunk'
.......
const increment = () => ({type:'INCREMENT'})    //+1
const decrement = () => ({type:'DECREMENT'})     //-1
const incrementIfOdd=()=>(dispatch,getState)=>{     //奇数+1，偶数不变
  const value=getState();
  if(value%2===0)
    return
  dispatch(increment())
}

const incrementAsyn=(delay=1000)=>(dispatch)=>{    //延时1s后执行+1操作
  setTimeout(()=>dispatch(increment()),delay)
}

.......
//reducer
const counter=(state=0,action)=>{
  switch(action.type){
    case 'INCREMENT':
      return state+1
    case 'DECREMENT':
      return state-1
    default:
      return state
    }
  }
.............
const store = createStore(counter,applyMiddleware(thunk))
let currentValue=store.getState()    //获取初始化state到currentValue
store.subscribe(()=>{     //注册监听器
  const previousValue=currentValue      //初始 state值
  currentValue=store.getState()      //执行action后修改了state值，即获取当前state
  console.log('pre state:',previousValue,'next state:',currentValue)     //打印action执行前后的state值
}

...................................
//发起action
store.dispatch(increment())
store.dispatch(decrement())
store.dispatch(incrementIfOdd())
store.dispatch(incrementAsyn())
```

## 6、redux手动连接到react组件

* 将state和发起action的方法连接到组件
* 渲染组件并监听变化

```
//定义render函数以用于store监听调用
render=()=>{
  ReactDOM.render(
    <Count value={store.getState()} onIncrement={()=>store.dispatch({type:'INCREMENT'})} />,
    document.getElementById('root')
  )
}
//渲染组件
render()
store.subscribe(render);  //监听state的变化，一旦发生变化调用render方法
```

手动连接仅作为介绍原理用，有两个明显的缺点

* 无法直接给里面的组件传递state和方法
* 任意state的变化都会导致整个组件树的重新渲染，没有优化性能

实际项目采用react-redux方式实现

## 7、使用react-redux连接

创建了store、action（redux-thunk）后通过两步实现连接：

* 所有组件顶层使用Provider组件给**整个程序**提供store，一般为index.js或引用的App.js中
* ```
  import { Provider } from 'react-redux'
  ……………………
  ReactDOM.render(
    <Provider store={store}>
    .......
    </Provider>, document.getElementById('root')
  )
  ```
* 使用connect\(\)将state和action绑定到组件中

```
import { connect } from 'react-redux'
........................
class Building extends Component{
.............
}
const mapStateToProps = (state) => {
  let buildings = state.cList 
  return { buildings, editedIds }
}
Building = connect(
  mapStateToProps
)(Building)
export default Building;
```

## 8、connect使用方式

connect有一个或两个参数，第一个参数都是state

1、第二个参数为多个action创建函数组成的对象

```
import * as ActionCreators from '../actions'
import Count from '../component/Count'
import { connect } from 'react-redux'

export default connect(
  state=>({count:state.count}),
  ActionCreators
)(Count)
```

2、第二个参数为dispatch函数

```
import {increment,decrement,incrementIfOdd} from '../actions'
import Count from '../component/Count'
import { connect } from 'react-redux'

export default connect(
  state=>({count:state.count}),
  dispatch=>(
    {
      increment:()=>dispatch(increment()),
      decrement:()=>dispatch(decrement()),
      incrementIfOdd:()=>dispatch(incrementIfOdd()),
    }
  )
)(Count)
```

前面两种写法调用方式一样，都将action传入到props中，调用方式如下

```
const Counter=({counter,increment,decrement,incrementIfOdd}){   //析构写法，无状态对象可直接使用props参数，
//即分别为props.counter,props.increment...正常写法为参数props,则下面调用为props.counter,props.increment...

  return(
    <p>
      Clicked:{counter} times
      <button onClick={increment}> + </button>
      <button onClick={decrement}> - </button>
      <button onClick={incrementIfOdd}> odd </button>
    </p>
  )
```

3、第三种为第二种方式的简单写法，调用redux的bindActionCreators减少代码，调用方式一样

```
import {increment,decrement,incrementIfOdd} form '../actions'
```

```
import Counter from '../component/Count'
import { connect } from 'react-redux'
import {bindActionCreators} from 'redux'
import * as ActionCreators form '../actions'

export default connect(
  state=>({count:state.count}),
  dispatch=>bindActionCreators(ActionCreators，dispatch)
)(Counter)
```

4、只传一个state参数，此种方式会自动将dispatch绑定到props中，让组件自己调用需要的action

```
export default connect(
  state=>({count:state.count}) 
)(Counter)
```

调用方式：

```
const Counter=({dispatch}){   //connect只传递一个参数，自动将dispatch绑定到props
  return(
    <p>
      Clicked:{counter} times
      <button onClick={()=>dispatch(increment())}> + </button>
      <button onClick={()=>dispatch(decrement))}> - </button>
      <button onClick={()=>dispatch(incrementIfOdd())}> odd </button>
    </p>
  )
```



