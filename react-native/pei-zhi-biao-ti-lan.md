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
    headerStyle: {
      backgroundColor: '#f4511e',
    },
    headerTintColor: '#fff',
    headerTitleStyle: {
      fontWeight: 'bold',
    },
  };

  /* render function, etc */
}
```

标题样式有三个属性：

1. headerStyle：一个应用于header的最外层View的样式对象，如果你设置backgroundColor，他就是header的颜色。
2. headerTintColor：返回按钮和标题都使用这个属性作为它们的颜色。在下面的例子中，我们将tintcolor设置为白色（\#fff），所以返回按钮和标题栏标题将变为白色。
3. headerTitleStyle：如果我们想为标题定制fontFamily，fontWeight和其他Text样式属性，我们可以用它来完成。

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

* navigation- 页面的导航属性，在页面中的路由为navigation.state。
* screenProps- 从导航器组件上层传递的 props
* navigationOptions- 如果未提供新值，将使用的默认或上一个选项

#### 

#### 使用setParams更新navigationOptions

当前页面更新标题栏：

```
<Button
    title="Update the title"
    onPress={() => this.props.navigation.setParams({otherParam: 'Updated!'})}
  />
```

#### 全局通用样式：

```
const RootStack = createStackNavigator(
  {
    Home: HomeScreen,
    Details: DetailsScreen,
  },
  {
    initialRouteName: 'Home',
    /* The header config from HomeScreen is now here */
    /*所有页面标题样式*/
    defaultNavigationOptions: {
      headerStyle: {
        backgroundColor: '#f4511e',
      },
      headerTintColor: '#fff',
      headerTitleStyle: {
        fontWeight: 'bold',
      },
    },
  }
);
```

页面内定义的navigationOptions 会覆盖全局defaultNavigationOptions，也可通过调用navigationOptions参数自定义样式

