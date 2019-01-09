---
description: React 本身只是一个 DOM 的抽象层，使用组件构建虚拟 DOM。
---

# 从react角度来分析Dva

## React

React 本身只是一个 DOM 的抽象层，使用组件构建虚拟 DOM。

如果开发大应用，还需要解决一个问题。

* 通信：组件之间如何通信？
* 数据流：数据如何和视图串联起来？路由和数据如何绑定？如何编写异步逻辑？等等

## 通信问题

组件会发生三种通信。

* 向子组件发消息
* 向父组件发消息
* 向其他组件发消息

React 只提供了一种通信手段：传参。对于大应用，很不方便。

## 数据流问题

目前流行的数据流方案有：

* Flux，单向数据流方案，以 [Redux](https://github.com/reactjs/redux) 为代表
* Reactive，响应式数据流方案，以 [Mobx](https://github.com/mobxjs/mobx) 为代表
* 其他，比如 rxjs 等



## 目前最流行的数据方案

截止 2017.1，最流行的社区 React 应用架构方案如下。

* 路由： [React-Router](https://github.com/ReactTraining/react-router/tree/v2.8.1)
* 架构： [Redux](https://github.com/reactjs/redux)
* 异步操作： [Redux-saga](https://github.com/yelouafi/redux-saga)

缺点：要引入多个库，项目结构复杂

## Why Dva?

dva 是体验技术部开发的 React 应用框架，将上面三个 React 工具库包装在一起，简化了 API，让开发 React 应用更加方便和快捷。

_**dva = React-Router + Redux + Redux-saga**_

_\*\*\*\*_

## **模型1：**

![](http://zhouxianfei.gitee.io/imgstore/front/react/4.0.png)

### 核心概念 <a id="&#x6838;&#x5FC3;&#x6982;&#x5FF5;"></a>

* State：一个对象，保存整个应用状态
* View：React 组件构成的视图层
* Action：一个对象，描述事件
* connect 方法：一个函数，绑定 State 到 View
* dispatch 方法：一个函数，发送 Action 到 State

###  State 和 View <a id="state-&#x548C;-view"></a>

State 是储存数据的地方，收到 Action 以后，会更新数据。

View 就是 React 组件构成的 UI 层，从 State 取数据后，渲染成 HTML 代码。只要 State 有变化，View 就会自动更新。

## 模型二：

![](http://zhouxianfei.gitee.io/imgstore/front/react/4.1.png)

### dva 应用的最简结构（带 model\) <a id="dva-&#x5E94;&#x7528;&#x7684;&#x6700;&#x7B80;&#x7ED3;&#x6784;&#xFF08;&#x5E26;-model"></a>

```javascript
// 创建应用
const app = dva();

// 注册 Model
app.model({
  namespace: 'count',
  state: 0,
  reducers: {
    add(state) { return state + 1 },
  },
  effects: {
    *addAfter1Second(action, { call, put }) {
      yield call(delay, 1000);
      yield put({ type: 'add' });
    },
  },
});

// 注册视图
app.router(() => <ConnectedApp />);

// 启动应用
app.start('#root');
```

### Model 对象的属性 <a id="model-&#x5BF9;&#x8C61;&#x7684;&#x5C5E;&#x6027;"></a>

* namespace: 当前 Model 的名称。整个应用的 State，由多个小的 Model 的 State 以 namespace 为 key 合成
* state: 该 Model 当前的状态。数据保存在这里，直接决定了视图层的输出
* reducers: Action 处理器，处理同步动作，用来算出最新的 State
* effects：Action 处理器，处理异步动作

### connect 方法 <a id="connect-&#x65B9;&#x6CD5;"></a>

connect 是一个函数，绑定 State 到 View。

