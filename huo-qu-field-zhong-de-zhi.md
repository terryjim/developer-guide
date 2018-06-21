1、引入formValueSelector，定义selector

2、在mapStateToProps中关联表单中的输入项，用于更新输入项时将值传递给state

3、其它地方直接引用this.props...即可

```js
import {formValueSelector } from 'redux-form'
let xxxForm = props => {
  const {aaa,bbb} = props;
  console.log(aaa)
  console.log(bbb)  
//……
const selector = formValueSelector('xxxxx')     //form的名称
const mapStateToProps = (state) => {
  const aaa = selector(state, 'aaa')
  const bbb = selector(state, 'bbb')
  return {aaa,bbb}
}
```



