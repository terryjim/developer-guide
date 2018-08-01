# 引入redux-from

1、reducers/index.js文件中引入reducer

```
import { combineReducers } from 'redux'
import loading from './loading'
import err from './err'
import success from './success'
...
import { reducer as formReducer } from 'redux-form'
export default combineReducers({form: formReducer,loading,err,success,... })
```

2、具体表单文件中用reduxForm封装，同时定义form的名称

```
import { Field, reduxForm } from 'redux-form'
............
EditCompanyForm = reduxForm({
  form: 'company', // a unique name for this form
  validate,                // redux-form同步验证 
})(EditCompanyForm);
```

3、使用&lt;Field&gt;标签

```
import { Field, reduxForm } from 'redux-form'
......
 <form onSubmit={handleSubmit}>
<Field name="email" component="input" type="email" />
</form>
```



