## 在 App 容器中 调用 Dispatch 和 Navigate

如果在 App 容器中使用 dispatch，你可以使用[`ref`](https://facebook.github.io/react/docs/refs-and-the-dom.html#the-ref-callback-attribute)来调用 dispatch 方法：

```
const AppContainer = createAppContainer(AppNavigator);

class App extends React.Component {
  someEvent() {
    // call navigate for AppNavigator here:
    this.navigator &&
      this.navigator.dispatch(
        NavigationActions.navigate({ routeName: someRouteName })
      );
  }
  render() {
    return (
      <AppContainer
        ref={nav => {
          this.navigator = nav;
        }}
      />
    );
  }
}
```





