# 开发指南

１、添加导航react-navigation

```
yarn add react-navigation
yarn add react-native-gesture-handler
react-native link react-native-gesture-handler

//在MainActivity.java 上完成如下修改
package com.reactnavigation.example;
import com.facebook.react.ReactActivity;
+ import com.facebook.react.ReactActivityDelegate;
+ import com.facebook.react.ReactRootView;
+ import com.swmansion.gesturehandler.react.RNGestureHandlerEnabledRootView;

public class MainActivity extends ReactActivity {

  @Override
  protected String getMainComponentName() {
    return "Example";
  }

+  @Override
+  protected ReactActivityDelegate createReactActivityDelegate() {
+    return new ReactActivityDelegate(this, getMainComponentName()) {
+      @Override
+      protected ReactRootView createRootView() {
+       return new RNGestureHandlerEnabledRootView(MainActivity.this);
+      }
+    };
+  }
}
```

2、网络状态检测

```
yarn add @react-native-community/netinfo
react-native link @react-native-community/netinfo
```

３、图标库

```
yarn add react-native-vector-icons
react-native link react-native-vector-icons
```

４、提示框

```
yarn add react-native-root-toast
//yarn add react-native-root-sibling
```

5、加密算法

`yarn add crypto-js`

6、loading遮罩\(无需，直接下载源码修改\)

```
//废弃－－ yarn add react-native-loading-spinner-overlay
```

7、大列表

```
yarn add react-native-spring-scrollview react-native-largelist-v3

react-native link react-native-spring-scrollview
```

8、开屏页

```
npm i react-native-splash-screen --save
react-native link react-native-splash-screen
```



