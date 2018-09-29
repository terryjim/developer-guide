# no-loop与lock-on-active

no-loop

定义当前的规则是否不允许多次循环执行，默认是 false，也就是当前的规则只要满足条件，可以无限次执行

```
rule "Hello World"
no-loop true
    when
        m : Message( status == Message.HELLO, myMessage : message,count<100)
    then
        System.out.println( myMessage );
        m.setMessage( "Goodbye cruel world" );
        //m.setStatus( Message.GOODBYE );  
        m.count++;
        update( m );
end

```

以上代码含义：Message类中定义了count属性，初始为0，定义了HELLO常量，定义了status属性，当status=Message.HELLO时解发此规则。如果没有no-loop\(或设置no-loop为false\)时，会连续执行100次（如果去除count&lt;100条件将无限循环执行下去），否则只执行1次。

注意no-loop只在一个规则中有效，如果由其它规则重新触发此规则则不受影响

```
rule "Hello World"
no-loop false
    when
        m : Message( status == Message.HELLO, myMessage : message,count<100)
    then
        System.out.println( myMessage );
        m.setMessage( "Goodbye cruel world" );
        m.setStatus( Message.GOODBYE );
        m.count++;
        update( m );
end

rule "GoodBye"
    when
        m:Message( status == Message.GOODBYE, myMessage : message )
    then
      m.setMessage( "HELLO DOOM" );
        m.setStatus( Message.HELLO );
        System.out.println( myMessage );
          update( m );
end
```

如以上代码，即使设置了no-loop为false，但由于hello world规则执行时触发了goodbye规则，而又由goodbye触发hello world，所以hello world规则还是会执行100次

