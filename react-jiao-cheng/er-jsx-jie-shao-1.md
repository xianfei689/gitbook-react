---
description: JSX介绍
---

# 二、JSX介绍

### 一、基本认识

```text
const Ele = <h1>hello, world!</h1>
```

> 如上，这种在js中写html的语法就是JSX。它是JS的一种扩展语法（语法糖）。  
> JSX可以生成React元素，并将它渲染到DOM上。  
> 注意：JSX语法需要babel编译才能被识别

### 二、JSX中的嵌入表达式

* 你可以用花括号 **{ }** 来把任意的Javascript表达式嵌入到JSX中

```text
import React,{ Component } from 'react'

const user={
    firstName:'Jack',
    lastName:'Perez'
}
function formateName(user){
    return user.firstName+' '+user.lastName
}
class App extends Component{
    render(){
        return(
            <div>
                <h1>表达式1:{1+1}</h1>
                <h1>表达式2:{user.firstName}</h1>
                <h1>表达式3:{formateName(user)}</h1>
            </div>
        )
    }
}
export default App;
```

> **注意**

* JSX语句可以分割成多行，但最外层只能有一个标签元素
* 推荐用括号将JSX包裹起来，虽然这不是必须的，但这样做可以避免分号自动插入的陷阱。

### 三、JSX本身也是一个表达式

> 在通过编译之后，JSX表达式就变成了常规的JavaScript对象。这意味着你可以在 **if** 语句或 **for** 循环中使用JSX，用它给变量赋值，当做参数接收，或者作为函数的返回值。

```text
import React,{ Component } from 'react'

const user={
    firstName:'Jack',
    lastName:'Perez'
}
# 格式化名称
function formateName(user){
    return user.firstName+' '+user.lastName
}
# 确定返回的JSX
function getGreeting(user){
    if(user){
        return <h1>Hello! {formateName(user)}</h1>
    }else{
        return <h1>Hello! Stranger.</h1>
    }
}
# 构成组件
class App extends Component{
    render(){
        return(
            getGreeting(user)  //hello!  Jack Perez
        )
    }
}
export default App;
```

### 四、用JSX指定属性值

> 对于JSX中html标签上的属性值，有两种方式来指定

1. 双引号 **“ ”** 指定字符串字面量作为属性值
2. 花括号 **{ }** 指定js表达式作为属性值

```text
function getGreeting(user){
    if(user){
    // 字面量
        return <h1 className="title">Hello! {formateName(user)}</h1>
    }else{
    // 表达式
        return <img src={user.avatarUrl}></img>;
    }
}
```

* 注意原html中的class属性因与js中命名冲突，所以需要使用className代表

### 五、JSX的子元素

1. 如果是空标签，应该像XML一样，使用 **/&gt;** 立即闭合它

```text
const Ele = <img src={user.avatarUrl} />
```

1. 如果包含多个子元素

```text
const element = (
	<div>
    	<h1>Hello !</h1>
        <h2>Good to see you here.</h2>
    </div>
)
```

> 注意：  
> 比起 HTML ， JSX 更接近于 JavaScript ， 所以 React DOM 使用驼峰\(camelCase\)属性命名约定, 而不是HTML属性名称。
>
> * 例如，class 在JSX中变为className，tabindex 变为 tabIndex。

### 六、JSX表示对象

> JSX语法最终会被Babel编译成 **React.createElement\(\)** 调用后的对象。  
> 如下两个例子是完全相同的：

```text
# JSX语法
const ele1 = <h1 className="jack">Hello world</h1>;

# 编译语法
const ele2 = React.createElement(
    'h1',
    {className:'dube'},
    'Hello Sophia'
)
# 渲染
class App extends Component{
    render(){
        return(
            <div>
                {ele1}
                {ele2}
            </div>
        )
    }
}
```

### 六、深入理解JSX

> 从本质上讲，JSX只是为React.createElement\(component,props,...children\)函数提供的语法糖。

```text
# JSX代码
<MyButton color="blue" shadowSize={2}>Click Me</MyButton>
# 编译为：
React.createElement(
	MyButton,
    {color:'blue',shadowSize:2},
    'click Me'
)
```

> JSX中的注释  
> JSX中不能直接使用/\*\*/的方式来注释内容，而需要使用{}包括起来

```text
<div>
    {/*这是注释的内容*/}
</div>
```

