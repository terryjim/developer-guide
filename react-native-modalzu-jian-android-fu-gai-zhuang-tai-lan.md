

[https://blog.csdn.net/weixin\_33831196/article/details/87958363](https://blog.csdn.net/weixin_33831196/article/details/87958363)

在App开发中经常需要做模态框，我们第一时间就会想到 React Native 的 Modal 组件，确实Modal组件很方便。但也有一些不尽人意的地方，在安卓App开发的过程中发现，Modal不会覆盖状态栏，就会导致Modal的背景色和状态栏的颜色不一致，即使是设置了沉浸式状态栏，这样破坏了App的整体性和美观。



Modal组件基本用法

* animationType: \['none', 'slide', 'fade'\] Modal展示和收起时的动画效果
* onRequestClose: Platform.OS === 'android' ? PropTypes.func.isRequired : PropTypes.func 安卓物理返回回调，安卓必传
* onShow: func Modal组件展开完成时调用
* transparent: bool Modal背景色是否透明
* visible: bool 控制Modal的展开与收起

基本用法

```
class ModalDemo extends Component {
 
    state = {visible: false}
    
    close() {
        this.setState({visible: false})
    }
 
    render() {
        return (
            <View style={{flex: 1}}>
                <Modal
                    animationType='slide'
                    transparent
                    visible={this.state.visible}
                    onRequestClose={() => { this.close() }}>
 
                    <Text>Modal Demo</Text>
                </Modal>
                <TouchableOpacity onPress={()=>{this.setState({visible: true})}}>
                    <Text>show</Text>
                </TouchableOpacity>
            </View>
        )
    }
}
 
复制代码
--------------------- 
作者：weixin_33831196 
来源：CSDN 
原文：https://blog.csdn.net/weixin_33831196/article/details/87958363 
版权声明：本文为博主原创文章，转载请附上博文链接！
```

### 简单实现Modal覆盖状态栏

```
{
    this.state.visible ?
        <View style={{position: 'absolute', width: '100%', height: '100%', backgroundColor: 'rgba(0,0,0,0.5)'}}>
            <Modal
                animationType='slide'
                transparent
                visible={this.state.visible}
                onRequestClose={() => { this.close() }}>
 
                <Text>Modal Demo</Text>
            </Modal>
        </View> : null
}
```

这样只是实现了覆盖状态栏，还需要对各个View层的点击事件作处理，以至于达到与原始Modal组件相同的效果。

### 基于Modal封装覆盖状态栏的Modal

```
<TouchableOpacity activeOpacity={1} onPress={() => {this.close()}} style={{ position: 'absolute',width: '100%',zIndex: 999,height: '100%',backgroundColor: 'rgba(0, 0, 0, 0.5)'}}>
    <Modal
        animationType='slide'
        transparent
        visible={this.state.visible}
        onRequestClose={() => { this.close() }}>
        <TouchableOpacity onPress={() => {this.close()}} activeOpacity={1}>
            <TouchableWithoutFeedback onPress={() => {}}>
    				<Text>内容</Text>
            </TouchableWithoutFeedback>
        </TouchableOpacity>
    </Modal>
</TouchableOpacity>
```



