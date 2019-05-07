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

