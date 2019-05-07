# 配置标题栏

每个页面组件可以有一个名为`navigationOptions`的静态属性，它是一个对象或一个返回包含各种配置选项的对象的函数。用于设置标题栏的标题的是`title`这个属性。

```
class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Home',
  };

  /* render function, etc */
}

class DetailsScreen extends React.Component {
  static navigationOptions = {
    title: 'Details',
  };

  /* render function, etc */
}
```

#### 标题中使用参数

```
class DetailsScreen extends React.Component {
  static navigationOptions = ({ navigation }) => {
    return {
      title: navigation.getParam('otherParam', 'A Nested Details Screen'),
    };
  };

  /* render function, etc */
}
```

注意不能直接通过props传值，必须通过传递内部navigation对象获取参数。因为它是组件的静态属性，所以`this`不会指向一个组件的实例，因此没有 props 可用。

##### 完整参数如下：

```
static navigationOptions = ({ navigation, navigationOptions, screenProps }) => {
。。。
```



