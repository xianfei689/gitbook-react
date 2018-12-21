---
description: 组件Components
---

# 四、组件Components



### 一、认识组件

> 组件使你可以将 UI 划分为一个一个独立，可复用的小部件，并可以对每个部件进行单独的设计。

> React 允许将代码封装成组件（component），然后像插入普通 HTML 标签一样，在网页中插入这个组件

> 从定义上来说， 组件就像JavaScript的函数。组件可以接收任意输入\(称为”props”\)， 并返回 React 元素，用以描述屏幕显示内容。



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

\*\*\*\*

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

