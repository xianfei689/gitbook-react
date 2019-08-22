---
description: redux简介&使用
---

# 十、redux简介&使用33

### 一、Redux简介

> React 只是 DOM 的一个抽象层，并不是 Web 应用的完整解决方案。有两个方面，它没涉及：_代码结构、组件间的通信_。 为了解决这个问题，2014年 Facebook 提出了 Flux 架构的概念，引发了很多的实现。2015年，Redux 出现，将 Flux 与函数式编程结合一起，很短时间内就成为了最热门的前端架构。
>
> Redux：简言之，一个状态机。就像vue的vuex一样，负责项目的单独的state状态管理，通过redux可以实现全局的状态管理，所有的state都在一个store对象中保存。当然，redux并不是react的标配，如果项目简单，交互并不负杂，组件间通信不频繁，不用websocket等就可以不用redux。
>
> Redux的设计思想：

1. Web应用是一个状态机，视图与状态是一一对应的。
2. 所有的状态，保存在一个对象里面。

### 二、基本使用

> redux使用最频繁的三个方法： `store.getState()`,`store.dispatch({type:'xx'})`,`store.subscribe(fn)`
>
> 使用redux的基本步骤：

1. **引入**：

```text
import {createStore} from 'redux'
```

1. **创建reducer**

```text
// action.type是必须属性
function counter(state=0,action){
     switch (action.type) {
      case 'INCREMENT':
        return state + 1;
      case 'DECREMENT':
        return state - 1;
      default:
        return state;
      }
}
let store = createSotre(counter);
export default store;
```

1. **组件中引入store的状态**

```text
import store from 'reducer/store.js';

# 伪代码
render(){
    return(<div>
        state:{store.getState()}
        <button onClick={()=>store.dispatch({type:'INCREMENT'})}>加一</button>
        <button onClick={()=>store.dispatch({type:'DECREMENT'})}>减一</button>
    </div>)
}
```

> 如上dispatch传入参数为一个对象，其中type为必须属性  
> store.dispatch\(\)触发state改变  
> store.getState\(\)获取state

1. **监听state的变化，自动刷新view层**

```text
# index.js
const render = ()=>ReactDOM.render(<App />, document.getElementById('root'));
render();
store.subscribe(render);
```

> 如上：`store.subscribe(fn)`传入一个函数来监听state的变化，当dispatch触发时，会自动调用fn。  
> 通过将原来的 ReactDOM.render进行函数包装，传入subscribe实现监听。  
> `store.subscribe(fn)`会返回一个函数，用于清除监听。

1. **触发state变化**

```text
store.dispatch({type:'INCREMENT'})
```

### 二 、Redux核心

> Redux的使用包含以下几个核心概念：

* `Action`： view层触发model改变的方法
* `Reducer`：改变state的处理函数
* `Store`：存储State的对象，包含操作state的方法

## 10.1 基本概念和API

### 总结

> 一般在项目中会有一个单独的 redux文件夹，目录如下：

```text
|_ redux
    |_ actions.js
    |_ actionTypes.js
    |_ reducers.js
    |_ store.js
```

### 一、Store

> Store 就是保存数据的地方，你可以把它看成一个容器。**整个应用只能有一个 Store**。
>
> Redux 提供`createStore`这个函数，用来生成 Store。

```text
import {createStore} from 'redux';
import reducers from './reducers.js';

const store = createStore(reducers);
```

* 上面代码中，createStore函数接受另一个函数\(reducer\)作为参数，返回新生成的 Store 对象。

#### 1.1 store的作用

* 维持应用的state；
* 提供`getState（）`方法获取state；
* 提供`dispatch（action）`方法更新state；
* 提供`subscribe（listener）`方法注册监听器
* 通过`subscribe（listener）`返回的函数注销是监听器；

> 注意：  
> 再次强调一下**Redux 应用只有一个单一的 store**。当需要拆分数据处理逻辑时，你应该使用 reducer 组合 而不是创建多个 store。

#### 1.2、state

> `Store`对象包含所有数据。如果想得到某个时点的数据，就要对 Store 生成快照。这种时点的数据集合，就叫做 State。  
> 当前时刻的 State，可以通过`store.getState()`拿到。

```text
import { createStore } from 'redux';
const store = createStore(fn);

const state = store.getState();
```

#### 1.3 Action

> State 的变化，会导致 View 的变化。但是，用户接触不到 State，只能接触到 View。所以，State 的变化必须是 View 导致的。Action 就是 View 发出的通知，表示 State 应该要发生变化了。

* Action 是一个对象。其中的`type`属性是必须的，表示 Action 的名称。其他属性可以自由设置。

```text
const action = {
  type: 'ADD_TODO',
  payload: 'Learn Redux'
};
```

> 上面代码中，Action 的名称是_ADD\_TODO_，它携带的信息是字符串Learn Redux。  
> 可以这样理解，Action 描述当前发生的事情。改变 State 的唯一办法，就是使用 Action。它会运送数据到 Store。

**1.3.1 Action Creator**

> View 要发送多少种消息，就会有多少种 Action。如果都手写，会很麻烦。可以定义一个函数来生成 Action，这个函数就叫 Action Creator。

```text
const ADD_TODO = '添加 TODO';

function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}

const action = addTodo('Learn Redux');
```

> 上面代码中，addTodo函数就是一个 Action Creator。

#### 1.4 store.dispatch\(\)

> `store.dispatch()`是View发出Action的唯一方法。

```text
import { createStore } from 'redux';
import {addTodo} from './actionCreator';
import reducers from './reducers.js';

const store = createStore(reducers);
const action  = addTodo('Learn Redux')
store.dispatch(action);
```

> 上面代码中，store.dispatch接受一个 Action 对象作为参数，将它发送出去。  
> 结合 Action Creator，生成action。

#### 1.5 store.subscribe\(\)

> Store 允许使用`store.subscribe`方法设置监听函数，一旦 State 发生变化，就自动执行这个函数。

```text
import {createStore} from 'redux';
const store = createStore(reducer);

store.subscribe(listener);
```

* 显然，只要把 View 的更新函数（对于 React 项目，就是组件的`render`方法或`setState`方法）放入listen，就会实现 View 的自动渲染。

> `store.subscribe`方法返回一个函数，调用这个函数就可以解除监听。

```text
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
);

unsubscribe(); //解除监听
```

### 二、Reducer

> Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。\(可以理解为一个处理函数\)  
> Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。  
> 整个应用的初始状态，可以作为 State 的默认值。下面是一个实际的例子。

```text
# 创建reducer
const reducer = (state = 0, action) => {
  switch (action.type) {
    case 'ADD':
      return state + action.payload;
    default: 
      return state;
  }
};

# 生成store
import {createStore} from 'redux';
const store = createStore(reducer);

# 触发reducer
store.dispatch({
    type:'ADD',
    payload:2,
})
```

* 上面代码中，createStore接受 Reducer 作为参数，生成一个新的 Store。以后每当store.dispatch发送过来一个新的 Action，就会自动调用 Reducer，得到新的 State。

#### 2.1 纯函数

> Reducer 函数最重要的特征是，它是一个纯函数。也就是说，只要是同样的输入，必定得到同样的输出。  
> 纯函数是函数式编程的概念，必须遵守以下一些约束。

1. 不得必定参数
2. 不能调用系统I/O的API
3. 不能调用Date.now\(\)或者Math.random\(\)等不纯的方法，因为每次会得到不一样的结果。

4. 由于 Reducer 是纯函数，就可以保证同样的State，必定得到同样的 View。但也正因为这一点，Reducer 函数里面不能改变 State，必须返回一个全新的对象，请参考下面的写法。

```text
// State 是一个对象
function reducer(state, action) {
  return Object.assign({}, state, { thingToChange });
  // 或者
  return { ...state, ...newState };
}

// State 是一个数组
function reducer(state, action) {
  return [...state, newItem];
}
```

#### 2.2 Reducer的拆分

> Reducer 函数负责生成 State。由于整个应用只有一个 State 对象，包含所有数据，对于大型应用来说，这个 State 必然十分庞大，导致 Reducer 函数也十分庞大。

* 如下示例：

```text
const chatReducer = (state = defaultState, action = {}) => {
  const { type, payload } = action;
  switch (type) {
    case ADD_CHAT:
      return Object.assign({}, state, {
        chatLog: state.chatLog.concat(payload)
      });
    case CHANGE_STATUS:
      return Object.assign({}, state, {
        statusMessage: payload
      });
    case CHANGE_USERNAME:
      return Object.assign({}, state, {
        userName: payload
      });
    default: return state;
  }
};
```

* 上面代码中，三种 Action 分别改变 State 的三个属性。

```text
ADD_CHAT：chatLog属性
CHANGE_STATUS：statusMessage属性
CHANGE_USERNAME：userName属性
```

* 这三个属性之间没有联系，这提示我们可以把 Reducer 函数拆分。不同的函数负责处理不同属性，最终把它们合并成一个大的 Reducer 即可。

```text
const chatReducer = (state = defaultState, action = {}) => {
  return {
    chatLog: chatLog(state.chatLog, action),
    statusMessage: statusMessage(state.statusMessage, action),
    userName: userName(state.userName, action)
  }
};
```

* 上面代码中，Reducer 函数被拆成了三个小函数，每一个负责生成对应的属性。

> 这种拆分与 React 应用的结构相吻合：一个 React 根组件由很多子组件构成。这就是说，子组件与子 Reducer 完全可以对应。

**combineReducers**

* Redux 提供了一个`combineReducers`方法，用于 Reducer 的拆分。你只要定义各个子 Reducer 函数，然后用这个方法，将它们合成一个大的 Reducer。

```text
import { combineReducers } from 'redux';

const chatReducer = combineReducers({
  chatLog,
  statusMessage,
  userName
})

export default todoApp;
```

* 上面的代码通过combineReducers方法将三个子 Reducer 合并成一个大的函数。
* 这种写法有一个前提，就是 _State 的属性名必须与子 Reducer 同名_。如果不同名，就要采用下面的写法。

```text
const reducer = combineReducers({
  a: doSomethingWithA,
  b: processB,
  c: c
})
```

* 总之，combineReducers\(\)做的就是产生一个整体的 Reducer 函数。该函数根据 State 的 key 去执行相应的子 Reducer，并将返回结果合并成一个大的 State 对象。

#### 2.3 Reducer的工作流程

1. 用户发出Action
2. store自动调用Reducer，并传入两个参数：当前State和收到的Action。
3. Reducer返回新的State
4. Store通过`store.subscribe(listener)`监听到State的变化
5. listener可以通过`store.getState()`得到当前状态。在listener内部触发重新渲染View

```text
function listerner() {
  let newState = store.getState();
  component.setState(newState);   
}
```

## 10.0 踩坑记录

**1. 使用react-redux 的Provider组件传入store时，在子组件中，this.context是空对象？？？**

* 原因：子组件没有声明 contextTypes
* 解决：

```text
class App extends React.Component{
//注意这里必须要显式的写出context参数
    constructor(props,context){
        console.log(this.context);  
    }
    componentDidMount(){
        console.log(this.context) //这里不用显示的写context参数
    }
}
App.contextTypes = {store:PropTypes.object.isRrequired}
```

> 总结：react中，`contex`t需要声明`contextTypes`才能拿到，只有上层组件childContext和context的type 匹配才会对下层组件可见。



