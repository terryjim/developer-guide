# 分组

## activation-group {#activation-group}

该属性将若干个规则划分成一个组，统一命名。在执行的时候，具有相同activation-group 属性的规则中_**只要有一个被执行，其它的规则都不再执行**_。可以用类似salience之类属性来实现规则的执行优先级。

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

## agenda-group {#agenda-group}

agenda-group属性的值是一个字符串，通过这个字符串，可以将规则分为若干个Agenda Group。引擎在调用设置了agenda-group属性的规则时需要显示指定某个Agenda Group得到Focus（焦点），否则将不执行该Agenda Group当中的规则。

```
package com.rules
 rule "test agenda-group"
    agenda-group "abc"
    when
    then
        System.out.println("规则test agenda-group 被触发");
    end
rule otherRule
    when
    then
        System.out.println("其他规则被触发");
    end
```

调用代码：

```
KieServices kieServices = KieServices.Factory.get();
KieContainer kieContainer = kieServices.getKieClasspathContainer();
KieSession kSession = kieContainer.newKieSession("ksession-rule");
kSession.getAgenda().getAgendaGroup("abc").setFocus();
kSession.fireAllRules();
kSession.dispose();
```

执行以上代码，打印结果为：

```
规则test agenda-group 被触发
其他规则被触发
```

如果将代码kSession.getAgenda\(\).getAgendaGroup\(“abc”\).setFocus\(\)注释掉，则只会打印出：

```
其他规则被触发
```

## auto-focus {#auto-focus}

通过设置该参数可省去调用代码中的setFocus方法

```
package com.rules
 rule "test agenda-group"
     agenda-group "abc"
     auto-focus true
     when
     then
         System.out.println("规则test agenda-group 被触发");
     end
```

调用代码：

```
KieServices kieServices = KieServices.Factory.get();
KieContainer kieContainer = kieServices.getKieClasspathContainer();
KieSession kSession = kieContainer.newKieSession("ksession-rule");
//kSession.getAgenda().getAgendaGroup("abc").setFocus(); 无需显示调用
kSession.fireAllRules();
kSession.dispose();
```



