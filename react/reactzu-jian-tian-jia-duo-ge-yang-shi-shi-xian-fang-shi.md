# react组件添加多个样式实现方式

react原生动态添加多个className会报错：

```
import style from './style.css'
<div className={style.class1 style.class2}>abc</div>
//希望得到最终效果是：<div class="class1 class2">abc</div> 实际会出错
```

正确方式为引入classnames库

```
npm install classnames --save
```

使用方式：

```
import classnames from 'classnames'
import style from './style.css'
......
<div className=classnames({
    'class1': true,
    'class2': true
)>
    abc
</div>
//可以省略后面的true

```

其它用法：

```
classNames('foo', 'bar'); // => 'foo bar'
classNames('foo', { bar: true }); // => 'foo bar'
classNames({ 'foo-bar': true }); // => 'foo-bar'
classNames({ 'foo-bar': false }); // => ''
classNames({ foo: true }, { bar: true }); // => 'foo bar'
classNames({ foo: true, bar: true }); // => 'foo bar'

// lots of arguments of various types
classNames('foo', { bar: true, duck: false }, 'baz', { quux: true }); // => 'foo bar baz quux'

// other falsy values are just ignored
classNames(null, false, 'bar', undefined, 0, 1, { baz: null }, ''); // => 'bar 1'

```

传入数组：

```
var arr = ['b', { c: true, d: false }];
classNames('a', arr); // => 'a b c'

```

动态class：

    let buttonType = 'primary';
    classNames({ [`btn-${buttonType}`]: true });

动态class样例：

```
import classnames from 'classnames'


const Button = React.createClass({
  // ...
  render () {
    let btnClass = classNames({
      btn: true,
      'btn-pressed': this.state.isPressed,
      'btn-over': !this.state.isPressed && this.state.isHovered
    });
    return <button className={btnClass}>{this.props.label}</button>;
  }
});
```

以上代码等同于：

```
let Button = React.createClass({
  // ...
  render () {
    var btnClass = 'btn';
    if (this.state.isPressed) btnClass += ' btn-pressed';
    else if (this.state.isHovered) btnClass += ' btn-over';
    return <button className={btnClass}>{this.props.label}</button>;
  }
});
```



