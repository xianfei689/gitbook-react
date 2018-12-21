---
description: 组件Components
---

# 四、组件Components

### 一、认识组件

> 组件使你可以将 UI 划分为一个一个独立，可复用的小部件，并可以对每个部件进行单独的设计。

> React 允许将代码封装成组件（component），然后像插入普通 HTML 标签一样，在网页中插入这个组件

> 从定义上来说， 组件就像JavaScript的函数。组件可以接收任意输入\(称为”props”\)， 并返回 React 元素，用以描述屏幕显示内容



## 4.1 定义组件

#### 1.1 定义组件

**1.1.1 函数式组件**

> 最简单的定义组件的方法是写一个 JavaScript 函数:

```text
function Welcome(props){
	return <h1>Hello, {props.name}</h1>
}
```

* 这个函数是一个有效的 React 组件，因为它接收一个 props 参数, 并返回一个 React 元素。 我们把此类组件称为”函数式\(Functional\)“组件， 因为从字面上看来它就是一个 JavaScript 函数。
* 函数式组件又称为`无状态组件`，因为这类组件只用来做静态展示，因此没有state属性，交互也相对较少。

**1.1.2 类组件**

```text
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

**1.1.3 综合示例**

```text
# App.js(定义两个组件)
import React,{Component} from 'react'

function ComEle(props){
    return <h1>函数式组件{props.name}</h1>
}

class Ele extends Component{
    render(){
        return (<h1>类声明组件,{this.props.name}</h1>)
    }
}

export {ComEle,Ele}

# index.js (渲染组件)
import React,{Component} from 'react'
import ReactDOM from 'react-dom'
import {Ele,ComEle} from './App'


class App extends Component{
    render(){
        return (<div>
                    <Ele  name="jack"/>
                    <ComEle name="hello" />
                </div>)
    }

}

ReactDOM.render(<App />,document.getElementById('root'))
```

> 注意：  
> 如上 **ComEle函数** 是一个函数式组件，当React遇到一个代表用户定义组件的元素时，它将JSX属性以一个单独对象的形式传递给相应的组件（props对象）  
> 其渲染过程如下：

1. 我们调用了ReactDOM.render\(\)方法并向其中传入了 **&lt;ComEle name="hello"\ /&gt;**
2. React调用ComEle组件，并向其中传入了{name:'Sara'} 作为props对象
3. ComEle组件返回 **&lt;h1&gt;函数式组件{**[**props.name**](http://props.name/)**}&lt;/h1&gt;**



## 4.2 复合组件

#### 二、复合组件

> 一个组件可以拆分为多个更小的组件，这样能更便于小组件的重复使用。

```text
class Comment extends React.Component {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

> 如上示例：它接受一个 _author_对象，text,date作为props，并用于描述一条评论。但它的耦合性很高，不利于利用其中的某个部分，因此我们可以对它内部进行组件封装

**2.1 提取头像组件Avatar**

```text
class Avatar extends Component{
	return (
    	<img className="Avatar"
        		src={props.user.avatarUrl}
                alt={props.user.name}
           />
    )
}

# 简化原Comment组件
class Comment extends React.Component {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author }/>
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

* Avatar组件不关心它在Comment中是如何渲染的
* 建议从组件本身的角度来命名props而不是它被使用的上下文环境

**2.2 提取UserInfo组件**

```text
class UserInfo extends React.Component{
	render(){
    	return (
        	<div className="userInfo">
            	<Avatar user={props.user} />
                <div className="userInfo-name">{props.user.name}</div>
            </div>
        )
    }
}

#  再次简单Comment组件
class Comment extends React.Component {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

> 提取组件可能看起来是一个繁琐的工作，但是在大型的 Apps 中可以回报给我们的是大量的可复用组件。一个好的经验准则是如果你 UI 的一部分需要用多次 \(Button，Panel，Avatar\)，或者本身足够复杂\(App，FeedStory，Comment\)，最好的做法是使其成为可复用组件。



## 4.3 React创建组件的三种方式及其区别

### 一、总结：

> React推出后，出于不同的原因先后出现三种定义react组件的方式，殊途同归；具体的三种方式：

1. 函数式定义的`无状态组件`
2. es5原生方式`React.createClass` 定义的组件
3. es6形式的 `extends React.Component`定义组件

* 引用自[博客](https://www.cnblogs.com/wonyun/p/5930333.html)
* 注意：在react 15.5版本以后，react对象本身不再有createClass方法，而是单独引用'create-react-class'模块来调用

```text
# 15.5版本前
const React = require('react')
const Footer = React.createClass({})

# 15.5 版以后
const react = require('react')
const createClass = require('create-react-class')
const Footer = createClass({})
```

* 更多react 15.5版本前后变化更新，[参考链接](https://zhuanlan.zhihu.com/p/26250968?group_id=834155723037487104)

#### 1.1 _无状态_函数式组件

> 创建无状态函数式组件形式是从`React 0.14版本`开始出现的。它是为了创建**纯展示组件**，这种组件`只负责根据传入的props来展示`，不涉及到要state状态的操作。具体的无状态函数式组件，其官方指出：

* 在大部分React代码中，大多数组件被写成无状态的组件，通过简单组合可以构建成其他的组件等；这种通过多个简单然后合并成一个大应用的设计模式被提倡。

> 无状态函数式组件形式上表现为一个只带有一个`render`方法的组件类，通过函数形式或者ES6 arrow function的形式在创建，并且该组件是`无state状态`的。具体的创建形式如下：

```text
const FnComponent = (props)=><div>Hello {props.name}!</div>
ReactDOM.render(<FnComponent  name="jack"/>,mountNode
```

* 特点：

1. **无状态组件创建形式使代码的可读性更好**，并减少了冗余代码
2. **组件不会被实例化，整体渲染性能得到提升**——（组件被精简成一个render方法的函数来实现的，由于是无状态组件，所以无状态组件就不会在有组件实例化过程，无实例化过程也就不需要分配多余的内存，从而性能得到一定的提升）
3. **组件不能访问`this`对象** ——（无状态组件没有实例化过程，所以无法访问组件this中的对象，例如 `this.ref`、`this.state`等均不能访问，如要访问就不能用这种方式来创建组件）
4. **组件无法访问生命周期的方法**—— （无状态组件是不需要组件生命周期管理和状态管理，所以底层实现这种形式的组件时是不会实现组件的生命周期方法。所以无状态组件是不能参与组件的各个生命周期管理的。当然如果在使用无状态组件时发现需要用到生命周期时可以使用[高阶组件](https://www.jianshu.com/p/63569386befc) ）
5. **无状态组件只能访问输入的props**——（无状态组件被鼓励在大型项目中尽可能以简单的写法来分割原本庞大的组件，未来React也会这种面向无状态组件在譬如无意义的检查和内存分配领域进行一系列优化，所以_只要有可能，尽量使用无状态组件。_）

#### 1.2 React.createClass

> `React.createClass`是react刚开始推荐的创建组件的方式，这是ES5的原生的JavaScript来实现的React组件，其形式如下：

```text
var ES5component = React.createClass({
    propTypes: {//定义传入props中的属性各种类型
        initialValue: React.PropTypes.string
    },
    defaultProps: { //组件默认的props对象
        initialValue: ''
    },
    // 设置 initial state
    getInitialState: function() {//组件相关的状态对象
        return {
            text: this.props.initialValue || 'placeholder'
        };
    },
    handleChange: function(event) {
        this.setState({ //this represents react component instance
            text: event.target.value
        });
    },
    render: function() {
        return (
            <div>
                Type something:
                <input onChange={this.handleChange} value={this.state.text} />
            </div>
        );
    }
});

```

> 与无状态组件相比，React.createClass和后面要描述的React.Component都是创建有状态的组件，这些组件是要被实例化的，并且可以访问组件的生命周期方法。但是随着React的发展，React.createClass形式自身的问题暴露出来：

* React.createClass会自动绑定函数的方法，导致不必要的性能开销，增加代码过时的可能性
* React.createClass的mixins不够自然直接（关于mixins可参考[这篇文章](https://segmentfault.com/a/1190000002704788)）；React.Component形式非常适合高阶组件（Higher Order Components--HOC）,它以更直观的形式展示了比mixins更强大的功能，并且HOC是纯净的JavaScript，不用担心他们会被废弃。

#### 1.3 React.Component

> `React.Component` 是以ES6的形式来创建react的组件的，是React目前极为推荐的创建有状态组件的试，最终会取代React.createClass形式；其相对于`React.createClass`可以更好的实现代码复用

```text
class ES6Component extends React.Component {
    constructor(props) {
        super(props);

        // 设置 initial state
        this.state = {
            text: props.initialValue || 'placeholder'
        };

        // ES6 类中函数必须手动绑定
        this.handleChange = this.handleChange.bind(this);
    }

    handleChange(event) {
        this.setState({
            text: event.target.value
        });
    }

    render() {
        return (
            <div>
                Type something:
                <input onChange={this.handleChange}
               value={this.state.text} />
            </div>
        );
    }
}
ES6Component.propTypes = {
    initialValue: React.PropTypes.string
};
ES6Component.defaultProps = {
    initialValue: ''
};
```

### 二、React.createClass与React.Component区别

#### 2.1 函数this自绑定

> React.createClass创建的组件，其每一个成员函数的this都有React自动绑定，任何时候使用，直接使用`this.method`即可，函数中的`this`会被正确设置。

```text
const Contacts = React.createClass({
	handleClick(){
    	console.log(tihs) //组件实例
    }
    render(){
    	return (
        	<div onClick={this.handleClick}>123</div>
        )
    }
})
```

> `React.Component`创建的组件，其成员函数不会自动绑定this，需要开发者手动绑定，否则this不能获取当前组件的实例对象

```text
class Contact extends React.Component{
	constructor(props){
    	super(props)
    }
    handleClick(){
    	console.log(this);  //null
    }
    render(){
    	return (
        	<div onClick={this.handleClick}>123</div>
        )
    }
}
```

* 关于`React.Component`绑定this的三种方法：

1. 在构造函数`constructor`中绑定：this.methods=this.methods.bind\(this\)
2. 在调用函数时绑定： `onClick={this.handleClick.bind(this)}`
3. 使用`箭头函数`来绑定：`onClick={()=>this.handleClick()}`

#### 2.2 组件属性类型propTypes及默认属性 defaultProps配置不同

> 1、 `React.createClass`在创建组件时，有关组件props的属性类型及组件默认的属性会作为属性实例的属性来配置，其中defaultProps是使用`getDefaultProps`的方法来获取默认组件属性的

```text
const TodoItem = React.createClass({
    propTypes: { //属性类型，type：object
        name: React.PropTypes.string
    },
    getDefaultProps(){   // 默认属性，return Object
        return {
            name: ''    
        }
    }
    render(){
        return <div></div>
    }
})
```

> 2、`React.Component`在创建组件时配置这两个对应信息时，他们是作为组件类的属性，不是组件实例的属性，也就是所谓的类的静态属性来配置的。对应上面配置如下：

```text
class TodoItem extends React.Component {
    static propTypes = {//类的静态属性
        name: React.PropTypes.string
    };

    static defaultProps = {//类的静态属性
        name: ''
    };
}
```

#### 2.3 组件初始状态state的配置不同

> `React.createClass` 创建的组件，其状态state是通过`getInitialState`方法来配置组件相关的状态；  
> `React.Component`创建的组件，其状态state是在`constructor`中像初始化组件属性一样声明的。

```text
# React.createClass方法
const TodoItem = React.createClass({
	getInitialState(){
    	return {
        	isEditing:false
        }
    }
    render(){
    	return <div><div>
    }
})

# React.Component方法
class ToDoItem extends React.Component{
	constructor(props){
    	super(props)
        this.state={
        	isEditing:false
        }
    }
    renter(){
    	return <div></div>
    }
}
```

#### 2.4 对Mixins的支持不同

> Mixins\(混入\)是面向对象编程OOP的一种实现，其作用是为了复用共有的代码，将共有的代码通过抽取为一个对象，然后通过Mixins进该对象来达到代码复用。具体可以参考[React Mixin的前世今生](https://www.w3ctech.com/topic/1599)。

> React.createClass在创建组件时可以使用mixins属性，以数组的形式来混合类的集合。

```text
var SomeMixin = {  
  doSomething() {

  }
};
const Contacts = React.createClass({  
  mixins: [SomeMixin],
  handleClick() {
    this.doSomething(); // use mixin
  },
  render() {
    return (
      <div onClick={this.handleClick}></div>
    );
  }
});
```

> 但是遗憾的是React.Component这种形式并不支持Mixins，至今React团队还没有给出一个该形式下的官方解决方案；但是React开发者社区提供一个全新的方式来取代Mixins,那就是Higher-Order Components，具体细节可以[参考这篇文章](https://zhuanlan.zhihu.com/p/24776678?group_id=802649040843051008)

### 三、如何选择哪种方式创建组件？

> 由于React团队已经声明React.createClass最终会被React.Component的类形式所取代。但是在找到Mixins替代方案之前是不会废弃掉React.createClass形式。所以：**能用React.Component创建的组件的就尽量不用React.createClass形式创建组件。**  
> 除此之外，创建组件的形式选择还应该根据下面来决定：

1. 只要有可能，尽量使用无状态组件创建形式。
2. 否则（如需要state、生命周期方法等），使用`React.Component`这种es6形式创建组件

#### 3.1 补充

> 无状态组件内部其实是可以使用ref功能的，虽然不能通过this.refs访问到，但是可以通过将ref内容保存到无状态组件内部的一个本地变量中获取到。  
> 例如下面这段代码可以使用ref来获取组件挂载到dom中后所指向的dom元素：

```text
function TestComp(props){
    let ref;
    return (<div>
        <div ref={(node) => ref = node}>
            ...
        </div>
    </div>)
}
```

