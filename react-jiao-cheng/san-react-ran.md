---
description: React渲染
---

# 三、React渲染

### 一、元素渲染

> 元素\(Elements\)是 React 应用中最小的构建部件（或者说构建块，building blocks）。  
> 不同于浏览器的 DOM 元素， React 元素是普通的对象，非常容易创建。React DOM 会负责更新 DOM ，以匹配React元素（DOM元素与React元素保持一致）。

#### 1.1 ReactDOM.render\(\)

> ReactDOM.render 是 React 的最基本方法，用于将模板转为 HTML 语言，并插入指定的 DOM 节点。

```text
import React from 'react'
import ReactDOM from 'react-dom';

const Ele =<h1>Hello </h1>
// 挂载
ReactDOM.render(Ele,document.getElementById('root'))

```

#### 1.2 渲染一个元素到DOM

> 要渲染一个 React 元素到一个 root DOM 节点，把它们传递给 ReactDOM.render\(\) 方法

```text
const element = <h1>Hello, world</h1>;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

> 添加组件属性，有一个地方需要注意，就是 class 属性需要写成 className ，for 属性需要写成 htmlFor ，这是因为 class 和 for 是 JavaScript 的保留字。

#### 1.3 React只更新必需更新的部分

> React DOM 会将元素及其子元素与之前版本逐一对比, 并只对有必要更新的 DOM 进行更新, 以达到 DOM 所需的状态。（虚拟DOM的作用）

### 二、虚拟DOM

> React非常快速是因为它从不直接操作DOM，而是在DOM的基础上建立了一个抽象层（虚拟DOM），，对数据和状态所做的任何改动，都会被自动且高效的同步到虚拟DOM，最后再批量同步到DOM中。

> 在React中，render执行的结果得到的并不是真正的DOM节点，而仅仅是JavaScript对象，称之为 **虚拟DOM**。  
> 虚拟DOM具有批处理和高效的 **Diff算法**，可以无需担心性能问题而随时“刷新”整个页面，因为虚拟DOM可以确保只对界面上真正变化的部分进行实际的DOM操作。

#### 2.1 传统App与React App的对比

* 传统App innerHTML : render html字符串 + 重新创建所有DOM元素
* ReactApp 虚拟DOM：render 虚拟DOM + diff + 更新必要的DOM元素

#### 2.2 虚拟DOM的原理

> React会在内存中维护一个虚拟DOM树，对这个树进行读或写，实际上是对虚拟DOM进行。当数据变化时，React会自动更新虚拟DOM，然后将新的虚拟DOM和旧的虚拟DOM进行对比，找到变更的部分，得出一个diff，然后将diff放到一个队列里，最终批量更新这些diff到DOM中。  
> ![&#x865A;&#x62DF;DOM&#x539F;&#x7406;](http://p3cnmw3ss.bkt.clouddn.com/01)

#### 2.3 虚拟DOM的优缺点

> 优点：

* 最终表现在DOM上的修改只是变更的部分，可以保证非常高效的渲染。

> 缺点：

* 首次渲染大量DOM时，由于多了一层虚拟DOM的计算，会比innerHTML插入慢。

