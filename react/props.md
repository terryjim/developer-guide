## Props

Props是react用于组件通信的重要媒介

```
<Content a=1 b=2 c=3/>   //给组件Content传递值为1\2\3的参数a\b\c
```

组件获取props方式：

1、无状态函数编写的组件直接将props作为参数传入组件即可

```
Content=pops=>{
  return <div>{props.a},{props.b},{props.c}</div>
}
//也可直接用析构写法
Content=({a,b,c})=>{
  return <div>{a},{b},{c}</div>
}

```

2、使用类编写的组件通过this.props获取props

