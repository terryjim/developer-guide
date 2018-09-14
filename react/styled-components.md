# styled-components

### styled-components 是什么？

* styled-components 是一个常用的 css in js 类库。和所有同类型的类库一样，通过 js 赋能解决了原生 css 所不具备的能力，比如变量、循环、函数等。

### 相对于其他预处理有什么优点？

* 诸如 sass&less 等预处理可以解决部分 css 的局限性，但还是要学习新的语法，而且需要对其编译，其复杂的 webpack 配置也总是让开发者抵触。
* 如果有过sass&less的经验，也能很快的切换到styled-components，因为大部分语法都类似，比如嵌套，& 继承等， styled-componens 很好的解决了学习成本与开发环境问题，很适合 React 技术栈 && React Native 的项目开发。

### 解决了什么问题？

* className 的写法会让原本写css的写法十分难以接受
* 如果通过导入css的方式 会导致变量泄露成为全局 需要配置webpack让其模块化
* 以及上面提到的解决了原生 css 所不具备的能力，能够加速项目的快速开发

###  安装

```
npm install --save styled-components
```

#### 最基础的使用

    import styled from 'styled-components'

    const Title = styled.h1`
        font-size: 1.5em;
        text-align: center;
        color: palevioletred;
    `;
    // 相当于  const Title = styled.h1(xx)
    const Wrapper = styled.section`
        padding: 4em;
        background: papayawhip;
    `;
        render () {
            return (
                <Wrapper>
                    <Title>Hello styled-components</Title>
                </Wrapper>
            )
        }


此时我们可以看到控制台中输出了一个随机的className，这是styled-components帮我们完成的. 注意: 组件名要以大些开头 不然会被解析成普通标签

#### 传递props

    const Button = styled.button`
        background: ${props => props.primary ? 'palevioletred' : 'white'};
        color: ${props => props.primary ? 'white' : 'palevioletred'};
        font-size: 1em;
        margin: 1em;
        padding: 0.25em 1em;
        border: 2px solid palevioletred;
        border-radius: 3px;
    `
    render(
        <div>
            <Button>Normal</Button>
            <Button primary>Primary</Button>
        </div>
    );




在组件传递的props都可以在定义组件时获取到，这样就很容易实现定制某些风格组件

### props高级用法

设置默认值，在未设定必须传值的情况下我们会给一个默认值\(defaultProps\)

    export default class ALbum extends React.Component {

        constructor (props) {
            super(props)
            this.state = {
                // 接收传递的值
                imgSrc: props.imgSrc
            }
        }

        render () {
            const {imgSrc} = this.state
            return (
                <Container imgSrc={imgSrc}>
                </Container>
            )
        }
    }

    // 在这里是可以拿到props的 
    const Container = styled.div`
        background-size: cover;
        background-image: url(${props =>  props.imgSrc});
        width: 100%;    
        height: 300px;
    `

    // 当然没传值也没关系  我们设置默认值
    Container.defaultProps = {
        imgSrc: Cover
    }



#### 塑造组件

这个非常有用 你可能会遇到一些原本就已经是组件了 但是你要为他添加一些样式，这时候该怎么办呢 ?

    // 传递className 在react-native 中要使用 style
    const Link = ({className , children}) => (
        <a className={className}>
            {children}
        </a>
    )

    const StyledLink = styled(Link)`
        color: palevioletred;
    `
    render(
        <div>
            <Link>普通组件</Link>
            <StyledLink>有颜色吗？</StyledLink>
        </div>
    );




#### 组件样式继承

    const
     Button = styled.button
    `
        color: palevioletred;
        font-size: 1em;
        margin: 1em;
        padding: 0.25em 1em;
        border: 2px solid palevioletred;
        border-radius: 3px;
    `
    ;

    const
     TomatoButton = Button.extend
    `
        color: tomato;
        border-color: tomato;
    `
    ;

    // TomatoButton 部分样式继承自 Button 这种情况下不会生成两个class

#### 改变组件标签

在闲的蛋疼的情况下 我们想要改变组件的标签 比如把 button 变成 a 标签

```
// 利用上面定义的 Button 组件 调用 withComponent 方法
const Link = Button.withComponent('a')

```

#### 维护其他属性

在某种情况下，我们可能需要用到第三方库样式，我们可以使用这个方法轻松达到

    const Input = styled.input.attrs({
        // 定义静态 props
        type: 'password',
        // 没传默认使用 1em
        margin: props => props.size || '1em',
        padding: props => props.size || '1em'
    })`
        color: palevioletred;
        font-size: 1em;
        border: 2px solid palevioletred;
        border-radius: 3px;
        // 动态计算props
        margin: ${props => props.margin};
        padding: ${props => props.padding}
    `
    render ( <Input size='1em'></Input>  <Input size='2em'></Input> )


#### 动画

动画会生成一个随机类名 而不会污染到全局

    import { keyframes } from 'styled-components'
    // CSS 动画
    const rotate360 = keyframes`
        from {
            transform: rotate(0);
        }
        to {
            transform: rotate(360deg);
        }
    `
    const Rotate = Button.extend`
        animation: ${rotate360} 2s linear infinite;
    `
    render ( <Rotate>  💅  </Rotate> )

    作者：宇cccc
    链接：https://www.jianshu.com/p/2178abb2ee95
    來源：简书
    简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。


### 结语

styled-components虽然解决了大部分问题，增加了可维护性，但是破坏了原生体验，时常我们需要写更多的代码来达到业务要求，希望未来有更好的方案.

  




