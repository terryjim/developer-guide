# javascript、React中的this

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

### _**采用es6中的=&gt;可以避免此问题**_

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

react经常会采用构造函数绑定方法的形式来解决this不确定的问题

```
class App extends Component {
  constructor(props){
     super(props)
     this.handler＝this.handler.bind(this)
  }
  handler(){
    console.log("handle",this)
  }
  render() {
    console.log("render",this)      //加载后打印出App对象
    return (
      <h1 onClick={this.handler}>hello</h1>  //点击hello打印出App对象
    )
  }
}
```

### **采用=&gt;方式实现：**

```
class App extends Component {
  handler(){
    console.log("handle",this)
  }
  handler2=()=>console.log("handle2",this)
  render() {
    console.log("render",this)    //加载后打印出App对象
    return (
      <div>
        <h1 onClick={this.handler}>handle1</h1>   //点击handle1打印undefined,因为window对象没有handle方法
        <h1 onClick={()=>this.handler()}>handle1</h1>  //点击handle1打印App对象
        <h1 onClick={this.handler2}>handle2</h1>//点击handle2打印App对象
      </div>
    );
  }
}
```



