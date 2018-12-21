---
description: React组件应用场景
---

# 七、React组件应用场景

 [7.1 条件渲染](https://www.kancloud.cn/sophie_u/react/542180)  
[7.2 列表渲染和Key](https://www.kancloud.cn/sophie_u/react/547082)  
[7.3 表单组件](https://www.kancloud.cn/sophie_u/react/547083)



## 7.1 条件渲染

> 在 React 中，你可以创建不同的组件封装你所需要的行为。然后，只渲染它们之中的一些，取决于你的应用的状态。  
> React 中的条件渲染就和在 JavaScript 中的条件语句一样。使用 JavaScript 操作符如 if 或者条件操作符来创建渲染当前状态的元素，并且让 React 更新匹配的 UI 。

### 1.1 If 条件渲染

> 示例场景：根据用户是否登录，来返回不同的欢迎界面

```text
# 两个不同场景的组件
function UserGreeting(props) {
    return <h1>Welcome back!</h1>;
  }
  
  function GuestGreeting(props) {
    return <h1>Please sign up.</h1>;
}
# 条件不同返回不同组件
function Greeting(props) {
    const isLoggedIn = props.isLoggedIn;
    if (isLoggedIn) {
      return <UserGreeting />;
    }
    return <GuestGreeting />;
}

# 使用Greeting组件
ReactDOM.render(
  // 修改为 isLoggedIn={true} 试试:
  <Greeting isLoggedIn={false} />,
  document.getElementById('root')
);
```

### 1.2、使用元素变量

> 你可以用变量来存储元素。这可以帮助您有条件地渲染组件的一部分，而输出的其余部分不会更改。  
> 例：  
> 有以下两个组件，分别用于显示登出和录入按钮

```text
function LoginButton(props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}

function LogoutButton(props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}
```

> 接下来创建一个_有状态组件_，根据其自身状态来渲染这两个组件，并同时渲染1.1中的Greetin组件

```text

function UserGreeting(props) {
    return <h1>Welcome back!</h1>;
  }
  
  function GuestGreeting(props) {
    return <h1>Please sign up.</h1>;
}
function Greeting(props) {
    const isLoggedIn = props.isLoggedIn;
    if (isLoggedIn) {
      return <UserGreeting />;
    }
    return <GuestGreeting />;
}
function LoginButton(props) {
    return (
      <button onClick={props.onClick}>
        Login
      </button>
    );
  }
  
  function LogoutButton(props) {
    return (
      <button onClick={props.onClick}>
        Logout
      </button>
    );
}
  
class LoginControl extends React.Component{
    constructor(props) {
        super(props);
        this.state = { isLoggedIn: false };
        this.handleLoginClick=this.handleLoginClick.bind(this)
        this.handleLogoutClick=this.handleLogoutClick.bind(this)
    }

    handleLogoutClick() {
        this.setState({
            isLoggedIn:false
        })
    }
    handleLoginClick() {
        this.setState({
            isLoggedIn:true
        })

    }
    render() {
        const isLoggedIn = this.state.isLoggedIn;
        let button = null;
        if (isLoggedIn) {
            button = <LogoutButton onClick={this.handleLogoutClick}/>
        } else {
            button = <LoginButton onClick={this.handleLoginClick}/>
        }
        return (
            <div>
                <Greeting isLoggedIn={isLoggedIn} />
                {button}
            </div>
        )
    }
}
```

### 使用逻辑&&操作符的内联if用法

> 在上面示例中使用if来有条件的渲染组件相对而言比较繁琐，因此可以使用 逻辑操作符&&来简化。



## 7.2 列表渲染和Key

### 一、列表组件

#### 1.1 多组件渲染

> 在React中，转换数组为元素列表的方式，参考arr.map\(\)

```text
const listData = [
    {"category": "Sporting Goods", "price": "$49.99", "stocked": true, "name": "Football"},
    {"category": "Sporting Goods", "price": "$9.99", "stocked": true, "name": "Baseball"},
    {"category": "Sporting Goods", "price": "$29.99", "stocked": false, "name": "Basketball"},
    {"category": "Electronics", "price": "$99.99", "stocked": true, "name": "iPod Touch"},
    {"category": "Electronics", "price": "$399.99", "stocked": false,"name": "iPhone 5"},
    {"category": "Electronics", "price": "$199.99","stocked": true, "name": "Nexus 7"}
]
const ListTpl = listData.map((data) => {
    return <li key={data.price}><span>{data.category}</span><span>{data.price}</span><span>{data.name}</span></li>
})
class List extends Component{
    render() {
        return (<ul>{ListTpl}</ul>)
    }
}
export default List;
```

* 如上，多元素集合 需要用 **{}** 包裹并在JSX 中直接引用

#### 1.2 基本列表组件

> 针对1.1 中的示例，可以将列表的渲染封装为一个完整的组件，通过传入数据来渲染

```text
const listData = [
    {"category": "Sporting Goods", "price": "$49.99", "stocked": true, "name": "Football"},
    {"category": "Sporting Goods", "price": "$9.99", "stocked": true, "name": "Baseball"},
    {"category": "Sporting Goods", "price": "$29.99", "stocked": false, "name": "Basketball"},
    {"category": "Electronics", "price": "$99.99", "stocked": true, "name": "iPod Touch"},
    {"category": "Electronics", "price": "$399.99", "stocked": false,"name": "iPhone 5"},
    {"category": "Electronics", "price": "$199.99","stocked": true, "name": "Nexus 7"}
]

// 封装的组件
function ListUl(props) {
    const ListTpl = props.data.map((data) => {
        return <li key={data.price}><span>{data.category}</span><span>{data.price}</span><span>{data.name}</span></li>
    })  
    return (<ul>{ListTpl}</ul>)
}
// 引用组件
class List extends Component{
    render() {
        return <ListUl data={listData}/>
    }
}
export default List;
```

### 二、键（Keys）

> 键\(Keys\) 帮助 React 标识哪个项被修改、添加或者移除了。数组中的每一个元素都应该有一个唯一不变的键\(Keys\)来标识

* 挑选 key 最好的方式是使用一个在它的同辈元素中不重复的标识字符串。多数情况你可以使用数据中的 IDs 作为 keys



## 7.3 表单组件

### 一、表单（Forms）

> HTML 表单元素与 React 中的其他 DOM 元素有所不同，因为表单元素自然地保留了一些内部状态。例如，这个纯 HTML 表单接受一个单独的 name

#### 1.1 受控组件

> 在 HTML 中，表单元素如 &lt;input&gt;，&lt;textarea&gt; 和 &lt;select&gt; 表单元素通常保持自己的状态，并根据用户输入进行更新。而在 React 中，可变状态一般保存在组件的 state\(状态\) 属性中，并且只能通过 setState\(\) 更新。

```text
<input
    type="text"
    value={this.state.value}
    onChange={(e) => {
        this.setState({
            value: e.target.value.toUpperCase(),
        });
    }}
/>
```

> 如上：在React中 的表单元素input，绑定了change事件，其value值与组件state属性相关联，每当表单的状态发生变化,都会被写入组件的state中,这种组件叫 **受控组件**  
> 从形式上看，受控组件即绑定有**value值**的组件（且需要有onChange事件响应函数对应当value值变化时的处理），非受控组件就是没有绑定value值的组件

```text
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

* 
