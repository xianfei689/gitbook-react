---
description: 安装与使用
---

# 一、安装与使用

### 1.1 安装

> 使用react有两种方式：1、直接在浏览器中引用react；2、通过创建webapp项目的方式使用

#### 1.1 以引用react的方式 使用

```text
<!DOCTYPE html>
<html>
  <head>
    <script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      // ** Our code goes here! **
    </script>
  </body>
</html>
```

* react.js ：react的核心库
* react-dome.js ：提供与DOM相关的功能库
* browser.min.js ：是将 JSX 语法转为 JavaScript 语法，这一步很消耗时间，实际上线的时候，应该将它放到服务器完成。
* 最后的script标签处，注意type为' **text/babel**'：这是因为 React 独有的 JSX 语法，跟 JavaScript 不兼容。凡是使用 JSX 的地方，都要加上 type="text/babel" 。

#### 1.2 脚手架安装使用

> 官网有一款脚手架可以直接生成react项目，使用脚手架生成的项目可以直接拿来使用，修改

```text
# 1、安装脚手架
npm i -g create-react-app
# 2、创建react项目
create-react-app myreact
# 3、启动项目
cd myreact
yarn start  
```

* yarn是一个类似于npm的依赖管理工具，可直接在官网下载安装 [官网](https://yarn.bootcss.com/)

### 二、简单使用

> 在使用_create-react-app_脚手架搭建项目后，默认会创建简单示例代码，我们可以在这个基础之上做修改，从而入门使用。

> src目录结构

```text
|__ App.js   (默认的第一个组件，可视作主容器组件)
|__ app.css (App组件的css)
|__ index.js   (主文件，react的渲染在这个文件中)
|__ index.css  (公共css文件）
|__ registerServiceWorker.js   (react处理离线缓存的生产环境下的文件)
```

* registerServiceWorker就是为react项目注册了一个service worker，用来做资源的缓存，这样你下次访问时，就可以更快的获取资源。而且因为资源被缓存，所以即使在离线的情况下也可以访问应用（此时使用的资源是之前缓存的资源）。注意，registerServiceWorker注册的service worker 只在生产环境中生效（process.env.NODE\_ENV === 'production'）

```text
# App.js
import React,{ Component } from 'react'
import './app.css'
class App extends Component {
  render(){
    return(
      <div className="test">
        <h1>这是主组件{1+2}</h1>
      </div>
    )
  }
}
export default App

# index.js
import React from 'react'
import ReactDom from 'react-dom'
import App from './App'

ReactDom.render(<App/>,document.getElementById('root'))
```

> 注意点：

1. 通过 **class XX extends React.Component {}** 来创建组件
2. 组件通过 **render函数**返回的JSX来渲染内容
3. 通过 **react-dom** 来实现组件在html中的挂载。ReactDom.render\(,document.getElementById\('root'\)\)
4. 组件JSX语法中，css的class名需要用 **className**，防止与js冲突
5. 用花括号 **{ }** 来包含表达式

