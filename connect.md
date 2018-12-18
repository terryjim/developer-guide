# react-redux中connect的装饰器用法@connect详解



通常我们需要一个reducer和一个action，然后使用connect来包裹你的Component。假设你已经有一个key为main的reducer和一个action.js. 我们的App.js一般都这么写：

```
import React from 'react'
import {render} from 'react-dom'
import {connect} from 'react-redux'
import {bindActionCreators} from 'redux'
import action from 'action.js'
 
class App extends React.Component{
  render(){
    return <div>hello</div>
  }
}
function mapStateToProps(state){
  return state.main
}
function mapDispatchToProps(dispatch){
  return bindActionCreators(action,dispatch)
}
export default connect(mapStateToProps,mapDispatchToProps)(App)
```

ok了，这样并没有什么问题。看着connect的用法，有没有觉得很熟悉？典型的wrapper嘛，这里必须拿装饰器来装一波啊，稍微改改：

```
import React from 'react'
import {render} from 'react-dom'
import {connect} from 'react-redux'
import {bindActionCreators} from 'redux'
import action from 'action.js'
 
@connect(
 state=>state.main,
 dispatch=>bindActionCreators(action,dispatch)
)
class App extends React.Component{
  render(){
    return <div>hello</div>
  }
}
```

装完了，看起来舒服了。在我们实际项目中，可能是一个模块下面又有很多个小组件，它们都共用同样的action和reducer，我们在每个组件中都这么写，是不是有点太麻烦了？冗余代码太多了。

其实是可以把connect抽取出来的，比如写一个connect.js:

```
import {connect} from 'react-redux'
import {bindActionCreators} from 'redux'
import action from 'action.js'
 
export default connect(
 state=>state.main,
 dispatch=>bindActionCreators(action,dispatch)
)
```

然后在需要用到的组件中这么用：

```
import React from 'react'
import {render} from 'react-dom'
import connect from 'connect.js'
 
@connect
export default class App extends React.Component{
  render(){
    return <div>hello</div>
  }
}
```

这样就ok了，和最开始的用法比起来，是不是明显更装逼更好用？

需要说明的是，这里用了装饰器，需要安装模块babel-plugin-transform-decorators-legacy，然后在babel中配置：

```
{
  "plugins":[
    "transform-decorators-legacy"
  ]
}
```

如果你用的是vscode, 可以在项目根目录下添加jsconfig.json文件来消除代码警告：

| `{"compilerOptions": {"experimentalDecorators":true}}` |
| :--- |


ok了，到这里真的完了。其实关于connect，是可以继续琢磨的，比如可以写一个通用的connect，所有的模块中所有的组件都可以用的那种

