## React Navigation 生命周期事件

一个包含 页面 A 和 B 的 StackNavigator ，当跳转到 A 时，`componentDidMount`方法会被调用； 当跳转到 B 时，`componentDidMount`方法也会被调用，但是 A 依然在堆栈中保持 被加载状态，他的`componentWillUnMount`也不会被调用。

当从 B 跳转到 A，B的`componentWillUnmount`方法会被调用，但是 A 的`componentDidMount`方法不会被调用，应为此时 A 依然是被加载状态



React Navigation 将事件发送到订阅了它们的页面组件： 有4个不同的事件可供订阅：

`willFocus`、`willBlur`、`didFocus`和`didBlur`。

