# 字段级输入校验

1、封装校验类

    export class FieldValidate {
      static required = value => (value || typeof value === 'number' ? undefined : '此字段不允许为空')
      static email = value => (value && !/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(value)
        ? 'email格式不对'
        : undefined)
      static maxLength = max => value =>
        value && value.length > max ? `字符数最多不得超过 ${max} ` : undefined
      static minLength = min => value =>
        value && value.length < min ? `字符数最少不得低于 ${min} ` : undefined
      //如果在Field中直接使用minLength(6)触发时机会有问题，所以要将限制字数直接写死为方法
      static minLength6 =FieldValidate.minLength(6)
      static handphone = value => (value && !/^((13[0-9]|14[579]|15[0-3,5-9]|16[6]|17[0135678]|18[0-9]|19[89])+\d{8})$/i.test(value)
        ? '手机号码格式不对'
        : undefined)
      //房号格式 1-1-1
      static room = value => (value && !/^\w+\-\w$/i.test(value)
        ? '房号格式不对'
        : undefined)
      //多间房号格式 1-1-1,1-1-2,2-1-1
      static rooms = value => (value && !/^\w+\-\w+\-\w(,\w+\-\w+\-\w)*$/i.test(value)
        ? '房号格式不对'
        : undefined)
    }

2、编写component

```
const renderField = ({
  input,
  label,
  type,
  meta: { touched, error, warning }
}) => (
  <div>
    <label>{label}</label>
    <div>
      <input {...input} placeholder={label} type={type} />
      {touched &&
        ((error && <span>{error}</span>) ||
          (warning && <span>{warning}</span>))}
    </div>
  </div>
)
```

3、Field中加入：

```
<Field
          name="name"
          component=renderField
          type="text"        
          validate={[FieldValidate.required,FieldValidate.minLength6]}
          //validate={[FieldValidate.required,FieldValidate.minLength(6)]} 也可使用，但触发时机会有问题，体验不好

         …………
        />
```

3、或直接在form类中定义校验方法，供Field中validate使用，此种情况不如直接定义validate函数做为表单级校验

