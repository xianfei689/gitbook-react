---
description: React-router初识
---

# 八、React-router初识

### 一、React-router V4

> 目前react-router版本已经是4.0+，以下以4.0+为例展开。React Router V4 相较于前面三个版本有根本性变化，首先是遵循Just Component的 API 设计理念，其次API方面也精简了不少，对新手来说降低了学习难度。  
> React Router V4 遵循了 React 的理念：万物皆组件。因此 升级之后的 Route、Link、Switch等都是一个普通的组件。  
> 附相关参考地址：

* [react-router文档](http://reacttraining.cn/web/guides/quick-start)
* [react-router初识](https://segmentfault.com/a/1190000010174260)

> react-router相关代码库：

* react-router ：React Router核心
* react-router-dom：用于Dom绑定的React-router（相较于react-router而言，多了Link，BrowserRouter这类组件（react-router和react-router-dom二选一引用就好）
* react-router-native：用于React Native的router
* react-router-readux ：react-router和redux的集成
* react-router-config ：静态路由配置的助手工具

#### 1.1 引用

> react-router还是react-router-dmo？  
> 在React的使用中，需要引入两个包：react,react-dom。但在React路由中，react-router和react-router-dom只需二选一引用就好。在react-router-dom中多了Link，BrowserRouter这样的DOM类组件。（如果搭配redux，还需要引用react-router-redux）

```text
# 安装
npm i -S react-router-dom
# 引用
import {BrowserRouter，Link，Switch} from 'react-router-dom'
```



## 8.1 React-router主要组件

### 一 主要组件

> React是一个一切皆组件的框架，所以在使用react-router也是组件，在4.0以上常用的的高级路由组件如下：

* &lt;BrowserRouter&gt;：\(动态网页路由\)使用 HTML5 提供的 history API 来保持 UI 和 URL 的同步；
* &lt;HashRouter&gt;：（静态资源路由）使用 URL 的 hash \(例如：window.location.hash\) 来保持 UI 和 URL 的同步；
* &lt;MemoryRouter&gt;：能在内存保存你 “URL” 的历史纪录\(并没有对地址栏读写\)；
* &lt;NativeRouter&gt;：为使用React Native提供路由支持；
* &lt;StaticRouter&gt;：从不会改变地址；

#### 1.1 初始实例：简单应用

```text
import React, { Component} from 'react'
import ReactDOM from 'react-dom'
import { BrowserRouter as Router, Route, Link, Switch } from 'react-router-dom'

// 组件定义
const App = ()=>(<div>路由内容1</div>)
const Other = ()=>(<div>路由内容2</div>)
const Else = ()=>(<div>路由内容3</div>)

// 路由组件
const RouterApp = () => (
    <Router>
        <div>
            <ul className='navLink'>
                <li><Link to='/app'>到app组件</Link></li>
                <li><Link to='/other'>到Other组件</Link></li>
                <li><Link to='/else'>到Elsep组件</Link></li>
            </ul>
            <div className='routerView'>
                <Route path='/app' component={App}/>
                <Route path='/other' component={Other}/>
                <Route path='/else' component={Else}/>
            </div>
            
        </div>
    </Router>
)
// 渲染
ReactDOM.render(<RouterApp />,document.getElementById('root'))

```

* 注意在react-router 4.0版本及以后，最好不要直接使用Router，而是用其高级路由组件BrowserRouter等代替（因为会报错）
* 注意以上Router组件其实是引用的BrowserRouter
* Link组件替换a标签,**Link组件必须放到Router组件内部**
* 注意： 外层BrowserRouter（Router）组件只能有一个子元素，所以需要用div包裹

### 1.2 Route组件

> Route组件的主要作用为根据当前location匹配的路由path，渲染对应的UI。（一个Route对应于一个组件）

```text
<div className='routerView'>
    <Route path='/app' component={App}/>
    <Route path='/other' component={Other}/>
    <Route path='/else' component={Else}/>
</div>
```

> 相关属性

* path （String）：路由匹配路径，没有path属性的Route总是会匹配 ‘/’
* exact（bool）：为true时，则要求路径与location.pathname必须完全匹配；
* strict（bool）：true的时候，有结尾斜线的路径只能匹配有斜线的location.pathname；

```text
# exact属性示例

// 当路由为/other时匹配，为/other:id等其他带参数时不匹配
 <Route exact path='/other' component={Other}/>  
 <li><Link to='/other:id'>到Other组件（不匹配）</Link></li>
 <li><Link to='/other'>到Other组件</Link></li>
 
 # strict属性示例
 <Route strict path='/other' component={Other}/>  
  <li><Link to='/other'>到Other组件（不匹配）</Link></li>
 <li><Link to='/other/'>到Other组件（匹配）</Link></li>
 <li><Link to='/other/two'>到Other组件（匹配）</Link></li>
```

#### 1.2.1 Route相关属性方法：

> Route组件有三种render method（渲染方法）分别是：**render**,**component**,**children**  
> 也有三种Props分别是：**match,location,history**

* 大部分情况下都是使用component={}的方式渲染Route组件
* render为Route的内联渲染方法

**1、三种render method**

**1）component**

> 只有当访问地址和路由匹配时，一个 React component 才会被渲染，此时此组件接受 route props \(match, location, history\)。

```text
<Route path="/user/:username" component={User} />

const User = ({ match }) => {
  return <h1>Hello {match.params.username}!</h1>
}
```

**2\) render:func**

> 此方法适用于内联渲染，而且不会产生上文说的重复装载问题。

```text
// 内联渲染
<Route path="/home" render={() => <h1>Home</h1} />
```

**3\) children:func**

> 有时候你可能只想知道访问地址是否被匹配，然后改变下别的东西，而不仅仅是对应的页面。

```text
<Route children={({ match, ...rest }) => (
  {/* Animate组件会一直渲染，内部的&&运算可以保证当路由匹配时才显示Something组件*/}
  <Animate>
    {match && <Something {...rest}/>}
  </Animate>
)}/>
```

**2、Route的三个Props**

**1）path**

* 当当前路么被path匹配时，渲染对应组件

**2） exact：bool**

* 为true时，表示绝对匹配，path为'/one'不能匹配 '/one/two'

**3） strict:bool**

* 为true时，表示要匹配路径末尾的斜杠，path为‘/path/'就不能匹配to为'/path'的

### Link组件

> 链接跳转导航组件，重点在组件属性：

* to（String/Object\)：要跳转的路径或地址；
* replace（Bool）：为true时，点击链接后将使用新地址替换掉访问历史记录里面的原地址；为 false 时，点击链接后将在原有访问历史记录的基础上添加一个新的纪录。默认为 false；_即：返回不起作用_

```text
<li><Link to='/other' replace={true}>到Other组件</Link></li>
```

#### 1、Link组件的属性

> 1）、to：String

* to='/path' 这种为string类型的时，直接匹配路径

> 2）、to:object

* 作用：携带参数跳转到指定路径
* 作用场景：比如你点击的这个链接将要跳转的页面需要展示此链接对应的内容，又比如这是个支付跳转，需要把商品的价格等信息传递过去。

```text
<Link to={{
  pathname: '/course',
  search: '?sort=name',
  state: { price: 18 }
}} />
```

> 3\)、replace：bool

* 为true时，点击链接后将使用新地址替换掉上一次访问的地址（即点链接后无法返回到点击前的页面\[被替换了\]）

### NavLink组件

> NavLink是Link的一个特定版本，会在匹配上当前URL的时候给已经渲染的元素添加样式参数，相关属性如下：

* activeClassName（String）：设置选中样式，默认为active
* activeStyle（Object）：当元素被选中时，为此元素添加样式
* exact（Bool）：为true时，只有当地址完全匹配时，class和style才会应用
* strict（Bool）：为true时，在确定location.pathname是否与当前URL匹配时，将考虑location.pathname后的斜线；
* isActive（func）：判断链接是否激活的额外逻辑功能

```text
// activeClassName选中时样式为selected
<NavLink
  to="/faq"
  activeClassName="selected"
>FAQs</NavLink>

// 选中时样式为activeStyle的样式设置
<NavLink
  to="/faq"
  activeStyle={{
    fontWeight: 'bold',
    color: 'red'
   }}
>FAQs</NavLink>

// 当event id为奇数的时候，激活链接
const oddEvent = (match, location) => {
  if (!match) {
    return false
  }
  const eventID = parseInt(match.params.eventID)
  return !isNaN(eventID) && eventID % 2 === 1
}

<NavLink
  to="/events/123"
  isActive={oddEvent}
>Event 123</NavLink>

```

### Switch组件

> 只渲染出第一个与当前访问地址匹配的 &lt;Route&gt; 或 &lt;Redirect&gt;。

```text
<Route path="/about" component={About}/>
<Route path="/:user" component={User}/>
<Route component={NoMatch}/>
```

* 如上三个路由，当不使用switch包裹时，访问 /about，以上三个组件都会渲染出来。此时swtich的作用体现出来了：只渲染第一个与当前访问地址匹配的组件

```text
<Fade>
  <Switch>
    {/* 用了Switch 这里每次只匹配一个路由，所有只有一个节点。 */}
    <Route/>
    <Route/>
  </Switch>
</Fade>

<Fade>
  <Route/>
  <Route/>
  {/* 不用 Switch 这里可能就会匹配多个路由了，即便匹配不到，也会返回一个null，使动画计算增加了一些麻烦。 */}
</Fade>

```

* 注意：Switch下的子节点只能是Route或者Redirect

### Redirect组件

* &lt;Redirect&gt; 渲染时将导航到一个新地址，这个新地址覆盖在访问历史信息里面的本该访问的那个地址。

**to: string**  
重定向的 URL 字符串

**to: object**  
重定向的 location 对象

**push: bool**  
若为真，重定向操作将会把新地址加入到访问历史记录里面，并且无法回退到前面的页面。

**from: string**  
需要匹配的将要被重定向路径

```text
# 例：根据当前用户是否登录 选择性渲染
import { Route, Redirect } from 'react-router'

<Route exact path="/" render={() => (
  loggedIn ? (
    <Redirect to="/dashboard"/>
  ) : (
    <PublicHomePage/>
  )
)}/>

# 例：组件中重定向
const Test = () => (<Router>
    <div>
        <ul>
            <li><Link to='/child1'>child1去</Link></li>
            <li><Link to='/child2'>child2去</Link></li>
            <li><Link to='/child3'>child3去</Link></li>
        </ul>

        <Route path='/child1' component={Child1}/>
        <Route path='/child2' component={Child2} />
        <Route path='/child3/:test' component={Child3} />
        
    </div>
</Router>)
const Child1 = () => (<div>child1</div>)
const Child2 = ({ match }) => {
    return (<div>子2</div>)
}
const Child3 = ({ match }) => { 
    console.log(match)
    return (<Redirect to="/child2"/>)
}
```

### Prompt组件

> 当用户离开当前页面前做一些提示

```text
# 普通提示
<Prompt message="确定要离开？" />
# 回调提示
<Prompt message={location => (
  `Are you sue you want to go to ${location.pathname}?` 
)} />
```

