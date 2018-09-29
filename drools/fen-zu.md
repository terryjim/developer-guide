# 分组

## activation-group {#activation-group}

该属性将若干个规则划分成一个组，统一命名。在执行的时候，具有相同activation-group 属性的规则中只要有一个被执行，其它的规则都不再执行。可以用类似salience之类属性来实现规则的执行优先级。

```
package com.rules

 rule "test-activation-group1"
    activation-group "foo"
    when
    then
        System.out.println("test-activation-group1 被触发");
    end

rule "test-activation-group2"
    activation-group "foo"
    salience 1
    when
    then
        System.out.println("test-activation-group2 被触发");
    end


```

同一activation-group 属性下只执行salience最高值对应的规则。执行规则之后，打印结果：

```
test-activation-group2 被触发
```



