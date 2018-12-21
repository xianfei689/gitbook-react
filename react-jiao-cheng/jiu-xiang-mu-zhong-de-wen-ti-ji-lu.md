---
description: 项目中的问题记录
---

# 九、项目中的问题记录

### 一、搭建项目

#### 1.1 项目脚手架

> 参考[脚手架市场](http://scaffold.ant.design/#/),搜索更多好用的脚手架

**1. 官方脚手架 create-react-app**

> create-react-app 是官方推出的一款脚手架，目录结构简单，默认不支持less，scss。如果需要在项目中使用scss，可以参考README.md的548行，其中有对加入scss的说明。  
> 在不支持less,scss之类预编译工具时，其实可以使用Koala

```text
|__ build  //打包后文件夹
|__ node_modules
|__  public  //公共静态资源
|__  src  //主开发目录
	|__  component  //组件目录
    |__  static  //图片，css等
    |__ index.js  // 入口文件
|__ package.json
|__ yarn.lock
|__ README.md
```

**2、dva-cli**

> _dva 是对 redux 的一层浅封装_。 dva-cli是dva的命令行工具，用于创建一个项目。其可以快速创建一个react，redux技术栈项目

```text
# 安装
npm i -g dva-cli
# 创建新应用(--demo用于创建简单的demo级项目，正常项目不加这个参数)
dva new myApp --demo  
```

**3、antd官方脚手架antd-design-pro**

> antd-design-pro：开箱即用的中后台前端/设计解决方案。结合react,antd,dva的一套上手即用的脚手架

```text
# 安装集成化cli工具
npm i -g ant-design-pro-cli -g
# 创建项目
mkdir pro-demo && cd pro-demo
pro new
```

> NaNundefined1、在React中引入本地图片的问题

```text
# 无效
<img src='./static/imgs/img_tishi.png'/>

# 有效
<img src={require('./static/imgs/img_tishi.png')} alt='tishi'/>
```



## 9.1 antd+react项目初体验

### 一、简介

> 此项目为使用官方create-react-app 脚手架搭建的简易项目，旨在熟悉react及antd语法，api等。  
> [antd文档地址](http://1x.ant.design/components/)

* 目录结构如下：

```text
|__ node_modules
|__public
	|__ index.html
    |__ favicon.ico
    |__ init.js
    |__ manifest.json
 |__ src
 	|__  component
    |__ css
    |__ images
    |__ less
    |__ index.js
    |__ registerServiceWorker.js
    |__ App.js
|__package.json
|__ README.md

```

> 由于官方脚手架对less，sass等预编译工具默认是不支持的，虽然其提供了解决办法，但为了简单，还是直接使用了Koala工具一线转换

### 二、项目中问题记录

#### 1、antd的Form组件以及表单验证

> 组件基本构造：

* `Form`为antd提供的form容器组件，表单组件还有Input,Select等
* `Form.Item`为form组件的表单项容器
* `label`：对应表单项的名称

```text
import {Form,Input,Button,Icon} from 'antd'

class Form extends React.Component{
	render(){
    const FormItem = Form.Item;
    const formItemLayout = {
    		labelCol: { span: 6 },
            wrapperCol:{span:14},
    }
    	return (
        {/*prefixCls：自定义表单className*/}
        	<Form horizontal="true"  prefixCls="loginForm">
            	<FormItem
                	label="手机号"
                    {...formItemLayout}
                >
                	<Input addonBefore={<Icon type="mobile" />} placeholder="请输入手机号"/>
                </FormItem>
            </Form>
        )
    }
}
```

* label：label标签的文件
* `labelCol`：label标签布局，同`Col`组件，设置`span、offset`值，如`{span:3,offset:12}`
* `wrapperCol`：需要为输入控件设置布局样式时，使用该属性，用法同labelCol

> 表单验证

* 验证规则设置
* 待表单项对应规则
* 提交事件处理函数

```text
import {Form,Input,Button,Icon} from 'antd'

class Form extends React.Component{
	 // 提交处理函数
    handleSubmit(e) {
        e.preventDefault();
        this.props.form.validateFields((errors, values) => {
            if (!!errors) {
                console.log('Eorros in form!!')
                return;
            }
            console.log('提交成功！！')
            console.log(values);
        })
    }
    //自定义验证规则
     checkTel(rule, val, cb) {
        const telReg = /^1[3456789]\d{9}$/
        if (val && telReg.test(val)) {
            console.log('验证成功')
            cb()
        } else {
            cb(new Error('请输入正确的手机号哦'))
        }
    }
	
	render(){
    const FormItem = Form.Item;
    const formItemLayout = {
    		labelCol: { span: 6 },
            wrapperCol:{span:14},
    }
    // 获取Form组件验证相关方法
     const { getFieldProps, getFieldError, isFieldValidating } = this.props.form;
    // 验证规则
    const telProps = getFieldProps('tel', {
        rules: [
            { required: true, min: 11, message: '请填写手机号' },
            {validator:this.checkTel}
        ]
    })
    	return (
        {/*prefixCls：自定义表单className*/}
        	<Form horizontal="true"  prefixCls="loginForm">
            	<FormItem
                	label="手机号"
                    {...formItemLayout}
                    hasFeedback
                     help={isFieldValidating('tel')?'校验中':(getFieldError('tel')||[]).join(',')}

                >
                	<Input  {...telProps} addonBefore={<Icon type="mobile" />} placeholder="请输入手机号"/>
                </FormItem>
            </Form>
        )
    }
}
```

* `getFieldProps(id,options)`:用于和表单进行双向绑定,详见antd文档描述（其中options的rules为校验规则）
* `isFieldValidating`:判断一个输入控件是否在校验状态
* `getFieldError`获取某个输入控件的Error
* `hasFeedback` :展示检验状态图标，建议只配合Input组件使用
* `help`：提示信息，如不设置，则会根据校验规则自动生成

#### 2、表单提交及隐藏登录模态框

> 场景：在一个Modal框中，有两个tab，一个是Login组件，一个是Register组件，都是表单，用于用户注册和登录。现在的需求是：在Login组件内，填入信息点击登录请求后，当返回`登录成功时`，隐藏父组件Modal框

```text
|__ 父组件： Modal  （自身方法 modalOk隐藏自己）
	|__ 子组件1 ： Login 登录
    			|___ 发出登录请求，成功后，隐藏Modal。
    |__  子组件2 ： Register 注册
    
    
    
 ## 伪代码实现
 class Modal extends Component{
 	constructor(){
    	super();
        this.state={
        	modalVisible:false
        }
    }
    hideModal(){
    	this.setState({
        	modalVisible:false
        })
    }
    showModal(){
    	this.setState({modalVisible:true})
    }
    render(){
    	return(){
        	<Modal visible={this.state.modalVisible}>
            	<Tabs defaultActiveKey="1" size="small">
                	<TabPane tab="登录" key="1">
                    	<LoginForm onSubmit={this.hideModal.bind(this)}/>
                    </TabPane>
                    <TabPane tab="注册" key="2"></TabPane>
                </Tabs>
            </Modal>
        }
    }
 }
 class LoginForm(){
 	constructor(){
    	super();
    }
    submitForm(){
    	......
        // 成功请求后，隐藏父Modal
        this.onSubmit();
    }
 }
```



## 9.2 fetch请求

### 一、简介

> fetch，XMLHttpRequest的一种替代方案。相比之下，fetch更优。与XMLHttpRequest\(XHR\)类似，fetch\(\)方法允许你发出AJAX请求。区别在于Fetch API使用Promise，因此是一种简洁明了的API，比XMLHttpRequest更加简单易用。（目前Chrome浏览器已经完美支持。）

#### 1.1 `fetch`的基本使用

> `fetch`是全局量window的一个方法, 第一个参数是URL:

```text
// url(必须），options（可选）
fetch(url,{
	method:'get'
})
.then(response=>response.json())
.then(json=>{
	// 处理数据结果
    console.log(json)
})
.catch(err=>{
	console.log(err)
})
```

* 注意：`response`回调结果对象本身有一个`json()`方法，用于将原始数据转换成 JavaScript 对象，如果需要请求JSON数据就可以调用它。

#### 1.2 `fetch`请求 request

**1、请求头 request header**

* 自定义请求头\(通过`new Headers()`来创建请求头\)：

1. `new Headers()` :创建请求头对象
2. `headers.append(key,value)` :添加请求头
3. `headers.has(key)` :判断是否有请求头
4. `headers.get(key)` :获取请求头
5. `headers.set(key,value)`：修改请求头
6. `headers.delete(key)`：删除请求头

```text
// 创建一个空的headers对象
var headers = new Headers();

// 添加（append）自定义请求头信息
headers.append('Content-Type','text/plain');
headers.append('X-My-Custom-Header', 'CustomValue');

// 判断（has），获取（get），以及修改（set）请求头
headers.has('Content-Type'); // true
headers.get('Content-Type'); // "text/plain"
headers.set('Content-Type', 'application/json');

// 删除某条请求头信息（delete）
headers.delete('X-My-Custom-Header');

// 创建对象时设置初始化信息
var headers = new Headers({
    'Content-Type': 'text/plain',
    'X-My-Custom-Header': 'CustomValue'
});

// 将headers对象添加到request方法中
var request = new Request(url,{
	headers:new Headers({'Content-Type':'text/plain'})
})
fetch(request).then(res=>{/*处理响应逻辑*/})
```



## 9.3 简单项目的规范

**1. 关于组件**

> 此次项目采取就近维护原则，组件分文件夹管理，其js，css在同文件中进行管理

