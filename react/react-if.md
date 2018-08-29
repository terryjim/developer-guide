# react-if

简化jsx中的if then语句

原写法：

```
render() {
    return (
        <div>
            <Header />
            {this.renderBody()}
            <Footer />
        </div>
    );
},
 
renderBody() {
 return (this.props.age >= this.props.drinkingAge)
    ? <span className="ok">Have a beer, {this.props.name}!</span>
    : <span className="not-ok">Sorry {this.props.name } you are not old enough.</span>;
}
```

导入react-if后

```
import React from 'react';
import { If, Then, Else } from 'react-if';
…………
render() {
    return (
        <div>
            <Header />
            <If condition={ this.props.age >= this.props.drinkingAge }>
                <Then><span className="ok">Have a beer, {this.props.name}!</span></Then>
                <Else>{() =>
                  <span className="not-ok">Sorry, {this.props.name}, you are not old enough.</span>
                }</Else>
            </If>
            <Footer />
        </div>
    );
}
```



