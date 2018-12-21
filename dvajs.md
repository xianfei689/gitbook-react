---
description: '基于 redux 和 redux-saga 的数据流方案,内置了 react-router 和 fetch，一个轻量级的应用框架。'
---

# DvaJS

### 特点

![](.gitbook/assets/image%20%283%29.png)

![](.gitbook/assets/image%20%289%29.png)

### 数据流向



![](.gitbook/assets/image%20%282%29.png)

分析：

用户交互行为或者浏览器行为（如路由跳转等）触发数据的改变，可以通过 **`dispatch`** 发起一个 **action**，如果是**同步行为**会直接通过 **`Reducers`** 改变 **`State`** ，如果是**异步行为**（副作用）会先触发 **`Effects`** 然后流向 **`Reducers`** 最终改变 **`State`**，

## Models

###       state

         Model 当前的状态。数据保存在这里，直接决定了视图层的输出

###       Action

        该 Model 当前的状态。数据保存在这里，直接决定了视图层的输出。

###       dispatch 

        dispatch 用于触发 action 的函数， action 是改变 State 的唯一途径，但是它只描述了一个行为，而 dipatch 可以看作是触发这个行为的方式，而 Reducer 则是描述如何改变数据的。

###       Reducer

         **Action 处理器，处理同步动作，用来算出最新的 State**。

###       Effect

         **Action 处理器，处理异步动作。**   

        **Effect 是一个 Generator 函数，内部使用 yield 关键字，标识每一步的操作（不管是异步或同步）**

        【 Action 处理器，处理异步动作，基于 Redux-saga 实现。Effect 指的是副作用。根据函数式编程，计算以外的操作都属于 Effect，典型的就是 I/O 操作、数据库读写。】

###       Subscription

          Subscriptions 是一种从 **源** 获取数据的方法，它来自于 elm。 Subscription 语义是订阅，用于订阅一个数据源，然后根据条件 dispatch 需要的 action。数据源可以是当前的时间、服务器的 websocket 连接、keyboard 输入、geolocation 变化、history 路由变化等等

