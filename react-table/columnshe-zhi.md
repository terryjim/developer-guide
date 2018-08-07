# column设置

一般设置如下：

```
 columns = [
    {
      id: 'index',
      Header: '', 
      filterable: false,
      width: 30,
      Cell: props => (props.viewIndex + 1)
    },
    {
      accessor: 'id',
      Header: 'id',
      show: false,
    }, {
      accessor: 'title',
      Header: '门禁名称', 
      filterable: false,
    }, {
      accessor: 'hardwareCode',
      Header: '硬件编号', 
      filterable: false,

    }, {
      id: 'status',
      Header: '在线状态', filterable: false,
      accessor: d => d.status === 1 ? <Badge value={d.status} className="mr-1" color="success">在线</Badge> : d.status === 2 ? <Badge className="mr-1" value={d.status} color="danger">不在线</Badge> : <Badge className="mr-1" value={0} color="info">未知状态</Badge>,
      sortMethod: (a, b) => {
        return a.props.value > b.props.value ? 1 : -1;
      }
    },
    {
      accessor: 'updated',
      Header: '最新在线时间', filterable: false,
    },
  ]
```



