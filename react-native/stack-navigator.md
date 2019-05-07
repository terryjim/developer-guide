# stack navigator

React Navigation 的 stack navigator 为你的应用提供了一种在屏幕之间切换并管理导航历史记录的方式。类似web浏览器导航，当用户与它进行交互时，应用程序会从导航堆栈中新增和删除页面，这会导致用户看到不同的页面。

简单示例：

```
import React from "react";
import { Button, View, Text } from "react-native";
import { createStackNavigator, createAppContainer } from "react-navigation";

class HomeScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
        <Text>Home Screen</Text>
        <Button
          title="Go to Details"
          onPress={() => this.props.navigation.navigate('Details')}
        />
      </View>
    );
  }
}
class DetailsScreen extends React.Component {
  render() {
    return (
      <View style={{ flex: 1, alignItems: "center", justifyContent: "center" }}>
        <Text>Details Screen</Text>
         <Button
          title="Go to Home"
          onPress={() => this.props.navigation.navigate('Home')}
        />
      </View>
    );
  }
}
const AppNavigator = createStackNavigator({
  Home: {
    screen: HomeScreen
  },
  Details:DetailsScreen   //只有一个路由配置的话，可以省略上面的{screen:DetailsScreen}
},
  {
    initialRouteName: "Home"
  });

export default createAppContainer(AppNavigator);
```

步骤：

创建普通页面对象--&gt;createStackNavigator组装--&gt;createAppContainer封装，通过定义的跌幅名字，如上方Home\Detail进行导航

#### navigation.navigate\('Home'\)与navigation.push\('Home'\)区别：

每次调用 \` push \` 时, 我们会向导航堆栈中添加新路由，而navigate则跳转新页面才有用。即当前页面为Home时，navigate\('Home'\)不起作用，而push\('Home'\)会新建一个Home出来（点击返回时会回到上一个Home\)

#### 总结：

this.props.navigation.navigate\('RouteName'\) 将新路由推送到堆栈导航器，如果它尚未在堆栈中，则跳转到该页面。

 \* 我们可以多次调用this.props.navigation.push\('RouteName'\)，并且它会继续推送路由。

 \* 标题栏会自动显示返回按钮，但你可以通过调用 this.props.navigation.goBack\(\) 以编程方式返回。 在Android上，硬件返回按钮会按预期工作。

 \* 您可以使用 this.props.navigation.navigate\('RouteName'\) 返回堆栈中的现有页面，你可以使用 this.props.navigation.popToTop\(\)返回堆栈中的第一个页面。

 \*  navigation  prop适用于所有屏幕组件（组件定义为路由配置中的屏幕，并且被 React Navigation 渲染为路由）。

