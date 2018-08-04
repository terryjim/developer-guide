# javascript中的this

javascript中的this不是在函数声明时定义而是在函数运行时被定义。

```
let student={
  speak:function(){
    console.log(this)
  }
}

student.speak();  //打印出student对象
let studentSpeak=student.speak
studentSpeak();  //打印出window对象
```

由此可见，组件方法的调用者不同会导致this不同！

采用es6中的=&gt;可以避免此问题

```
let student={
  speak:()=>{
    console.log(this)
  }
}

student.speak(); //打印出window对象
let studentSpeak=student.speak
studentSpeak(); //打印出window对象
```



```

```



