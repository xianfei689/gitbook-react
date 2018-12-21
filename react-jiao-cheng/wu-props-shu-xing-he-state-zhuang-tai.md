---
description: Props（属性）和State（状态）
---

# 五、Props（属性）和State（状态）

 React组件的prop属性为外部传入属性，不可修改  
state为自身状态，用户交互通过改变状态实现重新渲染

## 5.1 Props属性

### 一、Props属性

> state和props主要的区别在于props是不可变的，而state可以根据与用户交互来改变。这就是为什么有些容器组件需要定义 state 来更新和修改数据。 而子组件只能通过 props 来传递数据。

#### 1.1 使用props

```text
import React,{Component} from 'react'
import ReactDOM from 'react-dom'

// 通过this.props.name获取传入的属性
class App extends Component { 
    render() {
        return <h1>{this.props.name}</h1>  
    }
}
// 通过name=sophia 传入值
ReactDOM.render(<App name="sophia"/>,document.getElementById('root'))
```

#### 1.2 默认props

> 你可以通过 getDefaultProps\(\) 方法为 props 设置默认值，实例如下：

1. 常用写法

```text
# 声明组件
class App extends React.Component{ 
	static defaultProps = {  //默认属性一
    	name:'Mary'
    }
}

# 默认属性二
App.defaultProps={
	name:'Mary'
}
```

1. 非ES6写法

```text
# 使用非ES6写法，需要引入create-react-class模块

import createReactClass from 'create-react-class'

const Greeting = createReactClass({
    getDefaultProps: function () { 
        return {
            name:'小明'
        }
    },
    render: function () {
        return <h1>这是非ES6写法的组件{this.props.name}</h1>
    }
})

```

#### 1.3 this.props.children

> **this.props** 对象的属性与组件的属性一一对应，但是有一个例外，就是 **this.props.children** 属性。它表示组件的所有子节点

```text
class App extends Component { 
    render() {
        return (
            <div>
                <ol>
                    {React.Children.map(this.props.children, function (child) {
                         return <li>{child}</li>;
                    })}
               </ol>    
            </div>
        )
    }
}

ReactDOM.render(
<App>
    <h1>one</h1>
    <h1>two</h1>
</App>
,document.getElementById('root'))
```

> 注意：

* this.props.children 的值有三种可能：如果当前组件没有子节点，它就是 _undefined_ ;如果有一个子节点，数据类型是 _object_；如果有多个子节点，数据类型就是 _array_ 。所以，处理 this.props.children 的时候要小心。
* * React 提供一个工具方法 React.Children 来处理 this.props.children 。我们可以用 React.Children.map 来遍历子节点，而不用担心 this.props.children 的数据类型是 undefined 还是 object。更多的 React.Children 的方法，请参考[官方文档](https://reactjs.org/docs/react-api.html#react.children)。

#### 1.4 Props验证（PropTypes类型检查）

> Props 验证使用 **propTypes**，它可以保证我们的应用组件被正确使用，React.PropTypes 提供很多验证器 \(validator\) 来验证传入数据是否有效。当向 props 传入无效数据时，JavaScript 控制台会抛出警告。

```text
import React,{Component} from 'react'
import ReactDOM from 'react-dom'
import PropTypes from 'prop-types'

class App extends Component { 
	static propsTypes={ //设置props类型一
    	num:PropTypes.number
    }
    render() {
        return (
            <div>
                {this.props.num+1}
            </div>
        )
    }
}
//设置props类型二
App.propTypes = {
    num:PropTypes.number
}
ReactDOM.render(<App num={1}/>,document.getElementById('root'))

```

* 更多 PropTypes的验证类型参考[官方文档](https://reactjs.org/docs/typechecking-with-proptypes.html#proptypes)

> 例二： 只允许一个子元素的验证

```text
import PropTypes from 'prop-types';

class MyComponent extends React.Component {
  render() {
    // This must be exactly one element or it will warn.
    const children = this.props.children;
    return (
      <div>
        {children}
      </div>
    );
  }
}

MyComponent.propTypes = {
  children: PropTypes.element.isRequired
};
# <MyComponent></MyComponent>只能有一个子元素标签，否则报错
```



## 5.2 State状态

### 二、State状态

> React 把组件看成是一个状态机（State Machines）。通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。  
> React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。  
> 在React中，可以将State看作是组件的私有属性，而props则为外部属性（不可修改）

#### 2.1 定义状态

* 2.1.1 ES6 类声明式组件

> 在组件的 **constructor** 构造器中，设置初始状态state。  
> super\(props\) 用于继承所有属性

```text
import React,{Component} from 'react'
import ReactDOM from 'react-dom'
import PropTypes from 'prop-types'

class MyComponent extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            count:props.num+10
        }
    }
    render() {
      return (
        <div>
              <h1>props属性值：{this.props.num}</h1>
              <h1>state状态值：{this.state.count}</h1>
        </div>
      );
    }
}
MyComponent.propTypes = {
    num:PropTypes.number
}
class App extends Component { 
    render() {
        return (
            <div>
                <MyComponent num={0} />
            </div>
        )
    }
}
ReactDOM.render(<App/>,document.getElementById('root'))

```

#### 2.2 事件改变状态

> react中触发状态改变的是用户的交互行为  
> 在组件定义的 **constructor** 中绑定组件的this到事件函数中

```text
import React,{Component} from 'react'
import ReactDOM from 'react-dom'
class App extends Component { 
    constructor(props) { 
        super(props)
        this.state = {
            likeIt:true
        }
    }
    // 定义事件处理函数
    handleLike(lick) {
        this.setState({likeIt:!this.state.likeIt})
    }
    // 绑定事件，注意bind(this)
    render() {
        return (
            <div>
                <div>
                    <button onClick={this.handleLike.bind(this)}>点击切换状态</button>
                </div>
                <div>
                    <h1>Result:{this.state.likeIt?'喜欢':'不喜欢'}</h1>
                </div>  
            </div>
        )
    }
}
ReactDOM.render(<App/>,document.getElementById('root'))

```

> 注意：

1. 事件处理函数定义在类的声明中
2. 改变state用 this.setState\({stateName:value}\)
3. 事件处理函数需要绑定 this，才能在函数中使用this.state拿取到信息

#### 2.3 state的使用注意

**1、不要直接修改state（状态）**

```text
# 错误
this.state.comment = 'hello';
# 正确
this.setState({comment:'hello'})
```

**2、state（状态）更新可能是异步的**

> React 为了优化性能，有可能会将多个 setState\(\) 调用合并为一次更新。  
> 因为 this.props 和 this.state 可能是异步更新的，你不能依赖他们的值计算下一个state\(状态\)。

* 如下代码可能导致counter更新失败

```text
// 错误
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

> 如何解决上述问题？

* 要弥补这个问题，使用另一种 setState\(\) 的形式，它接受一个函数而不是一个对象。这个函数将接收前一个状态作为第一个参数，应用更新时的 props 作为第二个参数：

```text
// 正确
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```



## 5.3 组件间的通信

### 三、组件之间的通信

> 组件之间的通信可以分为三种：父传子，子传父，同级组件互传

#### 3.1 父传子

* 父组件通过 元素属性的方式 \( key=value \)向子组件传递信息
* 子组件通过this.props.key 获取传入的props

```text

class Child extends Component{
    render() {
        return (<h1>子组件获取到的props: {this.props.name}</h1>)
    }
}
class Parent extends Component{
    render() {
        return (
            <div>
                <Child name="Jack"/>
            </div>
        )
    }
}
```

#### 3.2 子传父 事件+state

1. 父组件传入子组件的prop值为父组件的状态
2. 父组件定义 事件处理函数 来响应状态变化
3. 子组件触发事件

```text

class Child extends Component{
    render() {
    // 点击时调用changeName事件，来向父组件传达改变状态的信息
        return (<div>
            <h1>子组件获取到的props: {this.props.name}</h1>
            <button onClick={() => { this.props.changeName() } }>change Name</button>
        </div>)
    }
}
class Parent extends Component{
    constructor() {
        super()
        // 设置初始状态
        this.state = {
            name:'Jack'
        }
    }
    // 传入改变自身状态的事件
    changeName() {  
        this.setState({
            name:'Rose'
        })
    }
    render() {
        return (
            <div>
                <Child name={this.state.name} changeName={this.changeName.bind(this)}/>
            </div>
        )
    }
}
```

#### 3.3 同级组件之间的通信

* 以同级组件的共同父组件为中介完成通信
* 同级组件的数据统一由父组件管理，当子组件要改变时则调用父组件的事件达到改变
* 父组件 state的改变会影响其所有子组件中 属性与其state相关的值的改变

```text

class Child extends Component{
    render() {
        return (<div>
            <h1>子组件获取到的props: {this.props.num}</h1>
            <button onClick={() => { this.props.add() } }>加1</button>
        </div>)
    }
}
class ChildOther extends Component{
    render() {
        return (<div>
            <h1>子组件获取到的props: {this.props.num}</h1>
            <button onClick={() => { this.props.minum() } }>减1</button>
        </div>)
    }
}
class Parent extends Component{
    constructor() {
        super()
        this.state = {
            name: 'Jack',
            num:1
        }
    }
    add() { 
        this.setState({
            num:this.state.num+1
        })
    }
    minum() {
        this.setState({
            num:this.state.num-1
        })
    }
    render() {
        return (
            <div>
                <Child num={this.state.num} add={this.add.bind(this)}/>
                <ChildOther num={this.state.num} minum={this.minum.bind(this)}/>
            </div>
        )
    }
}

```

#### 3.4 全局事件

> 当项目比较大，组件的嵌套层次比较深时，再使用兄弟组件间的通信就会对性能造成一定程度的影响，这时就可以使用 **全局事件** 来解决这类问题

> 官网中提到可以使用全局事件来进行组件间的通信，官网推荐 **Flux**（Facebook官方出的），还有**Relay、Redux、trandux**等第三方类库。这些框架思想都一致，都是统一管理组件state变化情况，达到数据可控目的。本人使用了Redux，建议要会其中一种。对于EventEmitter或PostalJS这类的第三方库是不建议使用的，这类全局事件框架并没有统一管理组件数据变化，用多了会导致数据流不可控。

> 简单的组件交流我们可以使用上面非全局事件的简单方式，但是当项目复杂，组件间层次越来越深，上面的交流方式就不太合适（当然还是要用到的，简单的交流）。强烈建议使用Flux、Relay、Redux、trandux等类库其中一种，这些类库不只适合React，像Angular等都可以使用。



## 5.4 单向数据流&事件

### 一、单向数据流

> 一个组件可以选择将state向下传递，作为其子组件的props属性，且任何State始终由某个特定组件所有，从该state导出的任何数据或UI只能影响树中”下方”的组件。  
> 所有组件都是完全独立的

### 二、处理事件

> 通过 React 元素处理事件跟在 DOM 元素上处理事件非常相似。但是有一些语法上的区别：

* React 事件使用驼峰命名，而不是全部小写。
* 通过 JSX , 你传递一个函数作为事件处理程序，而不是一个字符串。

```text
# HTML 中：
<button onclick="activateLasers()">
  Activate Lasers
</button>

# React中
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

> 在 React 中你不能通过返回 false 来 **阻止默认行为**。必须明确调用 **preventDefault** 。

```text
# HTML中
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
# React中

```

* 这里， e 是一个合成的事件。 React 根据 W3C 规范 定义了这个合成事件，所以你不需要担心跨浏览器的兼容性问题。查看 SyntheticEvent 参考指南了解更多。
* 当使用一个 ES6 类 定义一个组件时，通常的一个事件处理程序是类上的_一个方法_。例如， Toggle 组件渲染一个按钮，让用户在 “ON” 和 “OFF” 状态之间切换：

```text
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 这个绑定是必要的，使`this`在回调中起作用
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

* 在 JavaScript 中，类方法默认没有 `绑定this` 的。如果你忘记绑定 this.handleClick 并将其传递给onClick，那么在直接调用该函数时，this 会是 undefined 。

#### 2.2 传参给事件处理程序

```text
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

* 上述两行代码是等价的，分别使用箭头函数和bind
* 通过 **箭头函数**的方式，事件对象 **必须显式**的进行传递，但是通过 bind 的方式，事件对象以及更多的参数将会被隐式的进行传递。



## 5.5 Refs属性

### 一、Refs属性

> React 支持一种非常特殊的属性 Ref ，你可以用来绑定到 render\(\) 输出的任何组件上。  
> 这个特殊的属性允许你引用 render\(\) 返回的相应的支撑实例（ backing instance ）。这样就可以确保在任何时间总是拿到正确的实例。  
> 参考文章：[关于Refs](http://wiki.jikexueyuan.com/project/react/more-about-refs.html)

#### 1.1 使用方法

> 绑定ref属性

```text
<input ref="myInput" />
```

> 在其他代码中，通过this.refs获取支撑实例：

```text
var input = this.refs.myInput;
var inputValue = input.value;
```

#### 1.2 ref回调属性

> `ref`属性可以是一个回调函数，而不是一个名字。这个回调函数在组件安装后立即执行。被引用的组件作为一个参数传递，且回调函数可以立即使用这个组件，或保存供以后使用\(或实现这两种行为\)。

