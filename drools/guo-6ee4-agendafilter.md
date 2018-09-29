# 过滤－－AgendaFilter

对于规则的执行的控制，还可以使用org.kie.api.runtime.rule. AgendaFilter来实现。用户可以实现该接口的accept方法，通过规则当中的属性值来控制是否执行规则。

方法体如下：

```
boolean accept(Match match);
```

在该方法当中提供了一个Match参数，通过该参数可以获得当前正在执行的规则对象和属性。该方法要返回一个布尔值，返回true就执行规则，否则不执行。

### AgendaFilter代码实例 {#agendafilter代码实例}

规则文件代码：

```
package com.rules

 rule "test-agenda-group"

    when
    then
        System.out.println("规则test-agenda-group 被触发");
    end

rule other

    when
    then
        System.out.println("规则other被触发");
    end
```

实现的MyAgendaFilter代码：

```
package com.secbro.drools.filter;

import org.kie.api.runtime.rule.AgendaFilter;
import org.kie.api.runtime.rule.Match;

/**
 * Created by zhuzs on 2017/7/19.
 */
public class MyAgendaFilter implements AgendaFilter{

    private String ruleName;

    public MyAgendaFilter(String ruleName) {
        this.ruleName = ruleName;
    }

    @Override
    public boolean accept(Match match) {
        return match.getRule().getName().equals(ruleName) ? true : false;
}
// 省略getter/setter方法
}
```

测试方法：

```
KieServices kieServices = KieServices.Factory.get();
KieContainer kieContainer = kieServices.getKieClasspathContainer();
KieSession kSession = kieContainer.newKieSession("ksession-rule");

AgendaFilter filter = new MyAgendaFilter("test-agenda-group");
kSession.fireAllRules(filter);
kSession.dispose();
```

执行结果：

```
规则test-agenda-group 被触发
```

在执行规则的Filter中传入的规则名称为test-agenda-group，此规则被执行。而对照组的规则other，却未被执行。

