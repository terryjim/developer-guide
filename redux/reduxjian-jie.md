# redux基础概念

## 1、Action

Action本质上是javascript普通对象，使用 type字段表示要执行的动作（一般被定义为字符串常量），除此外，action对象结构可随意组成。Action是store是唯一数据来源。

```
const logined = ({ token, userName}) => ({
    type: 'LOGINED',
    token,
    userName
})
```

## 2、Reducer

Reducer是形式为\(state,action\)=&gt;state的纯函数，通过传递action修改state的值

```
const count=(state=0,action)=>{
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

Stroe是个全局对象，将action、reducer和state联系在一起，负责更新、查询、订阅state等工作。Store有以下功能：

* 维持应用的state
* 提供getState\(\)方法获取state
* 提供dispatch\(action\)方法更新state
* 通过subscribe\(listener\)注册监听器

```
import {createStore } from 'redux'
const store = createStore(count)
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



