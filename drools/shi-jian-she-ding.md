# 时间设定date-effective、date-expires

## date-effective {#date-effective}

该属性是用来控制规则只有在到达指定时间后才会触发。在规则运行时，引擎会拿当前操作系统的时间与date-effective设置的时间值进行比对，只有当系统时间大于等于date-effective设置的时间值时，规则才会触发执行，否则执行将不执行。在没有设置该属性的情况下，规则随时可以触发。

默认情况下，date-effective可接受的日期格式为“dd-MMM-yyyy”，不接收时间。例如2017 年7 月20 日，在设置为date-effective值时，如果操作系统为英文的，那么应该写成“20-Jul-2017”；如果是中文操作系统则为“20-七月-2017”。

示例代码：

```
rule "test-date"
//    date-effective "20-七月-2017 aa"
//    date-effective "20-七月-2017"
//    date-effective "20-Jul-2017aaa"
    date-effective "20-Jul-2017"
    when
    then
        System.out.println("规则被执行");
    end
```

值得注意的是以上注释掉的格式均能成功命中规则与后面的字符无关，因为默认时间格式只取字符串的指定位数进行格式化。

可以通过设置drools的日期格式化来完成任意格式的时间设定，而不是使用默认的格式。在调用代码之前设置日期格式化格式：

```
System.setProperty("drools.dateformat", "yyyy-MM-dd HH:mm");
```

在规则文件中就可以按照上面设定的格式来传入日期：

```
date-effective "2017-07-20 16:31"
```

## date-expires {#4210-date-expires}

此属性与date-effective的作用相反，用来设置规则的过期时间。时间格式可完全参考date-effective的时间格式。引擎在执行规则时会检查属性是否设置，如果设置则比较当前系统时间与设置时间，如果设置时间大于系统时间，则执行规则，否则不执行。实例代码同样参考date-effective。



