# 定时任务（参考https://blog.csdn.net/qq\_35992900/article/details/80429245）



## 1.SpringBoot定时任务的四种实现方式\(主要\)

1. Timer：这是java自带的java.util.Timer类，这个类允许你调度一个java.util.TimerTask任务。使用这种方式可以让你的程序按照某一个频度执行，但不能在指定时间运行。一般用的较少。
2. ScheduledExecutorService：也jdk自带的一个类；是基于线程池设计的定时任务类,每个调度任务都会分配到线程池中的一个线程去执行,也就是说,任务是并发执行,互不影响。
3. Spring Task：Spring3.0以后自带的task，可以将它看成一个轻量级的Quartz，而且使用起来比Quartz简单许多。
4. Quartz：这是一个功能比较强大的的调度器，可以让你的程序在指定时间执行，也可以按照某一个频度执行，配置起来稍显复杂。

### 1.1使用Timer

这是让你按照固定的频率去执行一个任务，不能指定时间。

```
public class TestTimer {
    public static void main(String[] args) {
        TimerTask timerTask = new TimerTask() {
            @Override
            public void run() {
                System.out.println("task  run:"+ new Date());
            }
        };
        Timer timer = new Timer();
        //安排指定的任务在指定的时间开始进行重复的固定延迟执行。这里是每3秒执行一次
        timer.schedule(timerTask,10,3000);
    }
}
```

### 1.2使用ScheduledExecutorService

和timer类似

```
public class TestScheduledExecutorService {
    public static void main(String[] args) {
        ScheduledExecutorService service = Executors.newSingleThreadScheduledExecutor();
        // 参数：1、任务体 2、首次执行的延时时间
        //      3、任务执行间隔 4、间隔时间单位
        service.scheduleAtFixedRate(()->System.out.println("task ScheduledExecutorService "+new Date()), 0, 3, TimeUnit.SECONDS);
    }
}
```

### 1.3使用Spring Task

在刚开始使用的时候，我们更改一个任务的执行时间，一般是这样的：修改定时任务的执行周期，把服务停下来，改下任务的cron参数，再重启服务就搞搞定了。这种方式很简单，没有可说的，但是有没有一种可能，在不停服务的情况下，就可以动态的修改任务的cron参数呢？那是必须有！

刚刚提到的方法里，我们在主类上面加@EnableScheduling注解，在任务方法前面加上@Scheduled\(cron =“0/5 \* \* \* \* \*”\)注解定义执行时间，但是动态配置的步骤就有点不一样：

1. 在定时任务类上增加@EnabledScheduling注解，并实现SchedulingConfigurer接口。
2. 设置一个静态的cron，用于存放任务执行周期参数。
3. 从数据库获取Cron参数，用于模拟实际业务中外部原因修改了任务执行周期。
4. 设置任务触发器，触发任务执行。

```
@SpringBootApplication
@EnableScheduling//启用定时任务
public class MainApplication {
……。

}

。……。…。……。……。……
public class WhitelistController implements SchedulingConfigurer {//实现接口
    public static String cron; 

    @Override
    public void configureTasks(ScheduledTaskRegistrar taskRegistrar) {
        //项目部署时，会在这里执行一次，从数据库拿到cron表达式
        cron =myService.getCronTime();
       Runnable task = new Runnable() {
           @Override
           public void run() {
              //任务逻辑代码部分.
              System.out.println("I am going:" + LocalDateTime.now());
           }
       };
       Trigger trigger = new Trigger() {
           @Override
           public Date nextExecutionTime(TriggerContext triggerContext) {
              //任务触发，可修改任务的执行周期.
              //每一次任务触发，都会执行这里的方法一次，重新获取下一次的执行时间        
            cron =myService.getCronTime();
              CronTrigger trigger = new CronTrigger(cron);
              Date nextExec = trigger.nextExecutionTime(triggerContext);
              return nextExec;
           }
       };
       taskRegistrar.addTriggerTask(task, trigger);
    }

}
```

因为是要任务执行一次的时候才会去修改时间的cron表达式，_**所以改了cron后，要在下下次任务执行时才会生效**_。

这里核心的主要是使用到了ScheduledTaskRegistrar这个类有一个方法addTriggerTask（Runnable,Trigger） 两个参数，一个Runnable,一个是Trigger，在Runnable中执行业务逻辑代码，在Trigger修改定时任务的执行周期。

### 1.4整合Quartz

暂未研究

