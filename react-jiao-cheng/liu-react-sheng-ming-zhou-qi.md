---
description: React生命周期
---

# 六、React生命周期



## 六、React生命周期

### 一、生命周期

> 生命周期（又称：生命钩子）指组件从持载到DOM，到从DOM卸载时，所经历的几个状态函数。

* 参考：[React组件生命周期](https://segmentfault.com/a/1190000006792687)

> React组件的生命周期可以大致分为三个阶段：`实例化期`，`存在期`，`销毁期`。  
> 在不同的生命时期对应有`钩子函数`

#### 1.1 实例化期

> 实例化期

1. `getDefaultProps`: 设置默认的`props`（此方法只适合React.createClass创建的组件，ES6方式使用 `static defaultProps={}`的形式
2. `getInitialState`：设置默认的状态`state`。ES6方式对应在contructor中直接设置`this.state`
3. `componentWillMount`：该方法会在组件首次渲染之前调用，这是在render方法调用前可修改state的最后一次机会
4. `render`：渲染。
5. `componentDidMount`：在首次真实DOM渲染后调用，当需要访问真实的DOM时，常用，请求外部接口数据一般在这里。（服务端中，该方法不会被调用）

#### 1.2 存在期

> 在实例化后，当`props`或`state`发生变化时，下面方法依次被调用：

1. `componentWillReceiveProps`：当通过父组件更新子组件props时，这个方法就会被调用
2. `shouldComponentUpdate(nextProps,nextState)`：是否该更新组件，默认返回true，当返回false时，后期函数就不会调用，组件不会再次渲染
3. `componentWillUpdate`：组件即将更新，props和state改变后必调用
4. `render`
5. `componentDidUpdate`：更新真实DOM成功后调用，这时可以访问真实的DOM

#### 1.3 销毁期

1. componentWillUnmount：组件被移除之前被调用，可以用于做一些清理工作，在componentDidMount方法中添加的所有任务都需要在该方法中撤销，比如创建的定时器或添加的事件监听器。当我们在组件中使用了setInterval，那我们就需要在这个方法中调用clearTimeout。

