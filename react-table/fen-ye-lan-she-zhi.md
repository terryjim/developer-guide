

# 分页栏设置

1、分页栏样式设置：

```
 getPaginationProps= {() => {           
            return {
              style: {
                background:  "red"
              }
            };
          }}
```

2、若分页栏不满足需要，可自定义PaginationComponent类

以下为所有可自定义的地方：

```
import { ReactTableDefaults } from 'react-table'
Object.assign(ReactTableDefaults, {
  TableComponent: component,
  TheadComponent: component,
  TbodyComponent: component,
  TrGroupComponent: component,
  TrComponent: component,
  ThComponent: component,
  TdComponent: component,
  TfootComponent: component,
  ExpanderComponent: component,
  AggregatedComponent: component,
  PivotValueComponent: component,
  PivotComponent: component,
  FilterComponent: component,
  PaginationComponent: component,
  PreviousComponent: undefined,
  NextComponent: undefined,
  LoadingComponent: component,
  NoDataComponent: component,
  ResizerComponent: component
})

// Or change per instance
<ReactTable
  TableComponent={Component},
  TheadComponent={Component},
  // etc...
  />
```

如果要自定义类最好下载对应源码进行修改

以下为自定义的page类，增加了总记录数功能，total由参数传递过来

    import React, { Component } from 'react'
    import classnames from 'classnames'
    //
    // import _ from './utils'

    const defaultButton = props => (
      <button type="button" {...props} className="-btn">
        {props.children}
      </button>
    )
    //自定义react-table 分布栏（加总条数）
    export default class MyPagination extends Component {
      constructor (props) {
        super()
    console.log(props)
        this.getSafePage = this.getSafePage.bind(this)
        this.changePage = this.changePage.bind(this)
        this.applyPage = this.applyPage.bind(this)

        this.state = {
          page: props.page,
        }
      }

      componentWillReceiveProps (nextProps) {
        this.setState({ page: nextProps.page })
      }

      getSafePage (page) {
        if (Number.isNaN(page)) {
          page = this.props.page
        }
        return Math.min(Math.max(page, 0), this.props.pages - 1)
      }

      changePage (page) {
        page = this.getSafePage(page)
        this.setState({ page })
        if (this.props.page !== page) {
          this.props.onPageChange(page)
        }
      }

      applyPage (e) {
        if (e) {
          e.preventDefault()
        }
        const page = this.state.page
        this.changePage(page === '' ? this.props.page : page)
      }

      render () {
        const {
          // Computed
          pages,
          // Props
          page,
          showPageSizeOptions,
          pageSizeOptions,
          pageSize,
          showPageJump,
          canPrevious,
          canNext,
          onPageSizeChange,
          className,
          PreviousComponent = defaultButton,
          NextComponent = defaultButton,
          total   //由react-table自定义参数传递而来
        } = this.props

        return (
          <div className={classnames(className, '-pagination')} style={this.props.style}>
            <div className="-previous">
              <PreviousComponent
                onClick={() => {
                  if (!canPrevious) return
                  this.changePage(page - 1)
                }}
                disabled={!canPrevious}
              >
                {this.props.previousText}
              </PreviousComponent>
            </div>
            <div className="-center">
              <span className="-pageInfo">
                {this.props.pageText}{' '}
                {showPageJump ? (
                  <div className="-pageJump">
                    <input
                      type={this.state.page === '' ? 'text' : 'number'}
                      onChange={e => {
                        const val = e.target.value
                        const page = val - 1
                        if (val === '') {
                          return this.setState({ page: val })
                        }
                        this.setState({ page: this.getSafePage(page) })
                      }}
                      value={this.state.page === '' ? '' : this.state.page + 1}
                      onBlur={this.applyPage}
                      onKeyPress={e => {
                        if (e.which === 13 || e.keyCode === 13) {
                          this.applyPage()
                        }
                      }}
                    />
                  </div>
                ) : (
                  <span className="-currentPage">{page + 1}</span>
                )}{' '}
                {this.props.ofText} <span className="-totalPages">{pages || 1}</span>
              </span>
              {showPageSizeOptions && (
                <span className="select-wrap -pageSizeOptions">
                  <select onChange={e => onPageSizeChange(Number(e.target.value))} value={pageSize}>
                    {pageSizeOptions.map((option, i) => (
                      // eslint-disable-next-line react/no-array-index-key
                      <option key={i} value={option}>
                        {`${option} ${this.props.rowsText}`}
                      </option>
                    ))}
                  </select>
                </span>
              )}
            </div>
    总数：{total}        <div className="-next">
              <NextComponent
                onClick={() => {
                  if (!canNext) return
                  this.changePage(page + 1)
                }}
                disabled={!canNext}
              >
                {this.props.nextText}
              </NextComponent>
            </div>
          </div>
        )
      }
    }


react-table调用如下：

```
<ReactTable
   total={XXXX},//可自己定义参数传递给相应组件
   PaginationComponent={MyPagination} //Mypagination可接收所有的ReactTable参数包含自定义的参数
  // etc...
  />
```



