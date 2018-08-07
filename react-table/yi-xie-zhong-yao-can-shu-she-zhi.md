# 一些重要参数设置

1、固定react-table高度

如果不设置固定高度，react-table会随内容自动调整高度。设置固定高度后会通过内部scroll方式显示数据。设置方式：

```
    <ReactTable 
        style={{
            height: "400px" // This will force the table body to overflow and scroll, since there is not enough room
          //height: document.body.clientHeight-180;按当前网页可见区域高减180px
          }}
          …………
      />
```

2、loading

显示数据加载中，state中设置变量标识是否加载完数据

```
constructor(props) {
    super(props);
    this.state = {     
    //打开时显示加载中
      loading:true,
      ………………
    };
}
componentWillReceiveProps(nextProps) {
    //收到数据后关闭加载中
    this.setState({loading:false})
     …………
  }  

<ReactTable
style={{
height: "400px" // This will force the table body to overflow and scroll, since there is not enough room
loading={this.state.loading}
}}
…………
/>
```



