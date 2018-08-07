## Props

Props是react用于组件通信的重要媒介

```
<Content abc=123/>   //给组件Content传递值为123的参数abc
```

组件获取props方式：

1、无状态函数编写的组件直接将props作为参数传入组件即可

```
Content=pops=>{
  return <div>Content组件的props.abc:{props.abc}</div>
}
```

2、使用类编写的组件通过this.props获取props

