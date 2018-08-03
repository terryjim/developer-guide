# 一些重要参数设置

1、固定react-table高度

如果不设置固定高度，react-table会随内容自动调整高度。设置固定高度后会通过内部scroll方式显示数据。设置方式：

```
    <ReactTable 
        style={{
            height: "400px" // This will force the table body to overflow and scroll, since there is not enough room
          //height: window.screen.height-280;按当前窗口大小减280px
          }}
          …………
      />
```



