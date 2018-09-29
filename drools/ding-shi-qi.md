## 定时器 {#定时器}

规则用基于 interval（间隔）和cron的定时器（timer），替代了被标注过时的duration  
属性。timer属性的使用示例：

```
timer ( int: <initial delay> <repeat interval>? )
timer ( int: 30s )
timer ( int: 30s 5m )

timer ( cron: <cron expression> )
timer ( cron:* 0/15 * * * ? )
```

间隔定时器用int来定义，它遵循java.util.Timer对象的使用方法。具有延迟和重复执行的选择。其中第一个参数表示启动之后延迟多长时间执行，第二个参数表示每隔多久执行一次。Cron定时器用cron来定义，使用标准的Unix cron表达式。示例代码如下：

```
rule "Send SMS every 15 minutes"
    timer (cron:* 0/15 * * * ?)
when
    $a : Alarm( on == true )
then
    channels[ "sms" ].insert( new Sms( $a.mobileNumber, "The alarm is still on" );
end
```

 下面以一个模拟的系统报警器来示例一下Timer的使用。规则timer每隔一秒执行一次，当满足触发规则返回结果至ResultEvent对象中，业务系统拿到报警信息，并打印。为了达到模拟的效果，使用了KieSession的fireUntilHalt方法和halt方法。示例代码如下。

 规则文件

```
package com.rules
import java.util.Date
import java.util.List
import com.secbro.drools.testTimer.Server

global com.secbro.drools.testTimer.ResultEvent event

rule "timerTest"
    timer (cron:0/1 * * * * ?)
    when
        server : Server(times > 10)
    then
    System.out.println("已经尝试"+server.getTimes()+"次，超过预警次数！");
    event.getEvents().add(new java.util.Date() + " - 服务器已经尝试" + server.getTimes() + "次，依旧失败，特发次报警信息！");
end
```

Server类：

```
package com.secbro.drools.testTimer;

/**
 * Created by zhuzs on 2017/7/21.
 */
public class Server {
    // 尝试次数
    private int times;

    Server(int times) {
        this.times = times;
    }
//省略getter/setter方法
}
```

返回结果ResultEvent类：

```
package com.secbro.drools.testTimer;

import java.util.ArrayList;
import java.util.List;

/**
 * Created by zhuzs on 2017/7/21.
 */
public class ResultEvent {
    private List<String> events = new ArrayList<>();
//省略getter/setter方法
  }
```

测试类：

```
package com.secbro.drools.testTimer;

import org.junit.Test;
import org.kie.api.KieServices;
import org.kie.api.runtime.KieContainer;
import org.kie.api.runtime.KieSession;
import org.kie.api.runtime.rule.FactHandle;

/**
 * Created by zhuzs on 2017/7/21.
 */
public class TimerRulesTest {

    @Test
    public void timerTest() throws InterruptedException {

        final KieSession kieSession = createKnowledgeSession();
        ResultEvent event = new ResultEvent();
        kieSession.setGlobal("event", event);

        final Server server = new Server(1);

        new Thread(new Runnable() {
            public void run() {
                kieSession.fireUntilHalt();
            }
        }).start();

        FactHandle serverHandle = kieSession.insert(server);

        for (int i = 8; i <= 15; i++) {
            Thread.sleep(2000);
            server.setTimes(++i);
            kieSession.update(serverHandle, server);
        }

        Thread.sleep(3000);
        kieSession.halt();
        System.out.println(event.getEvents());
    }

    private KieSession createKnowledgeSession() {
        KieServices kieServices = KieServices.Factory.get();
        KieContainer kieContainer = kieServices.getKieClasspathContainer();
        KieSession kSession = kieContainer.newKieSession("ksession-rule");
        return kSession;
    }

}
```

kmodule.xml文件：

```
<?xml version="1.0" encoding="UTF-8"?>
<kmodule xmlns="http://www.drools.org/xsd/kmodule">
    <kbase name="rules" packages="com.rules">
        <ksession name="ksession-rule"/>
    </kbase>
</kmodule>
```

控制台打印：

```
已经尝试11次，超过预警次数！
已经尝试11次，超过预警次数！
已经尝试13次，超过预警次数！
已经尝试13次，超过预警次数！
已经尝试15次，超过预警次数！
已经尝试15次，超过预警次数！
已经尝试15次，超过预警次数！

[Fri Jul 21 21:04:11 CST 2017 - 服务器已经尝试11次，依旧失败，特发次报警信息！, Fri Jul 21 21:04:12 CST 2017 - 服务器已经尝试11次，依旧失败，特发次报警信息！, Fri Jul 21 21:04:13 CST 2017 - 服务器已经尝试13次，依旧失败，特发次报警信息！, Fri Jul 21 21:04:14 CST 2017 - 服务器已经尝试13次，依旧失败，特发次报警信息！, Fri Jul 21 21:04:15 CST 2017 - 服务器已经尝试15次，依旧失败，特发次报警信息！, Fri Jul 21 21:04:16 CST 2017 - 服务器已经尝试15次，依旧失败，特发次报警信息！, Fri Jul 21 21:04:17 CST 2017 - 服务器已经尝试15次，依旧失败，特发次报警信息！]

```

很显然，定时器每隔一秒执行一次，当满足规则触发条件时，将结果放入ResultEvent中。

