# 1. React基础语法

官网：https://react.docschina.org/docs/getting-started.html

## 1.1 jsx语法

### 1.1.1 jsx初体验

```html
<html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
    </head>
<body>

  <!-- 准备容器 -->
  <div id="test"></div>

  <!-- 引入react核心库, -->
  <script type="text/javascript" src="../js/react.development.js"></script>
  <!-- 引入react-dom，用于支持react操作DOM -->
  <script type="text/javascript" src="../js/react-dom.development.js"></script>
  <!-- 引入babel，用于将jsx转为js -->
  <script type="text/javascript" src="../js/babel.min.js"></script>

  <script type="text/babel"> /* 此处是babel类型 */
    // 1. 创建虚拟DOM
    const vDom = (
      <h1 id="title">
        <span>Hello, React</span>
      </h1>
    )
    // 2. 渲染虚拟DOM到页面
    ReactDOM.render(vDom, document.getElementById('test'))
  </script>
</body>
</html>
```

### 1.1.2 jsx语法规则

​      1.定义虚拟DOM时，不要写引号。

​      2.标签中混入JS表达式时要用{}。比如注释：{/* */}，一般不在这写注释

​      3.样式的类名指定不要用class，要用className。

​      4.内联样式，要用style={{key:value}}的形式去写。

​      5.只有一个根标签

​      6.标签必须闭合

​      7.标签首字母

​        (1).若小写字母开头，则将该标签转为html中同名元素，若html中无该标签对应的同名元素，则报错。

​        (2).若大写字母开头，react就去渲染对应的组件，若组件没有定义，则报错。

```jsx
<script type="text/babel"> /* 此处是babel类型 */
    const myId = 'aTgUiGu'
    // 1. 创建虚拟DOM
    const vDom = (
    <div>
        <h2 className="title" id={myId.toLowerCase()}>
            <span style={{color: 'white', fontSize="29px"}}>{myId.toUppercase()}</span>
        </h2>
        <h2 className="title" id={myId.toLowerCase()}>
            <span style={{color: 'white', fontSize="29px"}}>{myId.toUppercase()}</span>
        </h2>
        // 标签必须闭合
        <input type="text"/>
    </div>
    )
    // 2. 渲染虚拟DOM到页面
    ReactDOM.render(vDom, document.getElementById('test'))
</script>
```

### 1.1.3 jsx练习

```
/* 
    一定注意区分：【js语句(代码)】与【js表达式】
    1.表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方
    可以用const a = 值来接的就是表达式
    下面这些都是表达式：
        (1). a
        (2). a+b
        (3). demo(1)
        (4). arr.map() 
        (5). function test () {}
    2.语句(代码)：
    下面这些都是语句(代码)：
        (1).if(){}
        (2).for(){}
        (3).switch(){case:xxxx}
*/
```

```jsx
<script type="text/babel" >

    //模拟一些数据
    const data = ['Angular','React','Vue']
    //1.创建虚拟DOM
    const VDOM = (
    <div>
        <h1>前端js框架列表</h1>
        <ul>
            {
                data.map((item,index)=>{
                    return <li key={index}>{item}</li>
                })
            }
        </ul>

    </div>
    )
    //2.渲染虚拟DOM到页面
    ReactDOM.render(VDOM,document.getElementById('test'))
	</script>
```



## 1.2 组件/模块

模块：向外提供特定功能的js程序, 一般就是一个js文件

​           作用：复用js, 简化js的编写, 提高js运行效率

组件：用来实现局部功能效果的代码和资源的集合(html/css/js/image等等)

​		   作用：复用编码, 简化项目编码, 提高运行效率

之后的代码只写...部分

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>1_函数式组件</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
		...
	</script>
</body>
</html>
```

## 1.3 组件类型

### 1.3.1 函数式组件

执行了ReactDOM.render(`<MyComponent/>`.......)之后，发生了什么？

​     1.React解析组件标签，找到了MyComponent组件。

​     2.发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中。

```jsx
//1.创建函数式组件
function MyComponent() {
    console.log(this); // 此处this为undefined，babel开启了严格模式
    return <h2>Hello, React</h2>
}
//2.渲染组件到页面
ReactDOM.render(<MyComponent/>, document.getElementById('test'))
```

### 1.3.2 类式组件

执行了ReactDOM.render(`<MyComponent/>`.......)之后，发生了什么？

* React解析组件标签，找到了MyComponent组件。

* 发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法。

* 将render返回的虚拟DOM转为真实DOM，随后呈现在页面中。

```jsx
class MyComponent extends React.Component {
    render() {
        // render放在MyComponent原型对象上
        // render由实例调用，this指向实例
        console.log(this);
        return <h2>Hello, React</h2>
    }
}

ReactDOM.render(<MyComponent/>, document.getElementById('test'))
```

## 1.4 state

组件实例三大属性：state, props, refs

state组件状态

```jsx
class MyComponent extends React.Component {

    // 类中直接赋值，会成为类实例的属性
    state = {isHot: true, wind: '微风'}
	// render调用1+n次，
    render() {
        const {isHot, wind} = this.state
        return <h2 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}, {wind}			</h2>
    }
    // 为了确保函数的this绑定在实例上，使用箭头函数
    // 如果使用普通的函数声明，将其作为onClick的回调函数，会发生this绑定丢失
    // 在严格模式下，this会绑定在undefined上
    changeWeather = () => {
        const isHot = this.state.isHot
        // state的修改只能通过setState，是合并式修改，不是覆盖式修改
        this.setState({isHot: !isHot})
    }
}

ReactDOM.render(<MyComponent/>, document.getElementById('test'))
```

另一种防止this丢失的方法

```jsx
class MyComponent extends React.Component {

    constructor() {
        // 用this之前必须调super
        super()
        // 实例化时，this.changeWeather显式的将this绑定给实例，并且生成一个新函数
        // onClick绑定的回调函数，this就确定指向实例了
        this.changeWeather = this.changeWeather.bind(this)
    }
    // 类中直接赋值，会成为类实例的属性
    state = {isHot: true, wind: '微风'}
    render() {
        const {isHot, wind} = this.state
        return <h2 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}, {wind}			</h2>
    }

    // 普通函数的this，调用时才能确定
    changeWeather() {
        const isHot = this.state.isHot
        // state的修改只能通过setState，是合并式修改，不是覆盖式修改
        this.setState({isHot: !isHot})
    }
}

ReactDOM.render(<MyComponent/>, document.getElementById('test'))
```

## 1.5 props

### 1.5.1 基本使用

```jsx
class MyComponent extends React.Component {
    render() {
        const {name, age, sex} = this.props 
        // this.props.name = '小张' 会报错，props只读
        return (
            <ul>
                <li>姓名：{name}</li>
                <li>年龄：{age}</li>
                <li>性别：{sex}</li>
            </ul>
        )
    }
}

// 两种传值方式
const age = 18
const person = {name: '小张', age: 22, sex: '男'}
ReactDOM.render(<MyComponent name="小王" age={age} sex="男"/>, 
                document.getElementById('test'))
// 渲染给不同的DOM容器
ReactDOM.render(<MyComponent {...person}/>, document.getElementById('test2'))
```

### 1.5.2 引入限制和默认值

需要另外引入一个库

```html
<!-- 引入prop-types，用于对组件标签属性进行限制 -->
<script type="text/javascript" src="../js/prop-types.js"></script>
```

```jsx
class MyComponent extends React.Component {
    static propTypes = {
        name: PropTypes.string.isRequired,
        age: PropTypes.number,
        sex: PropTypes.string
    }

    static defaultProps = {
        age: 18,
        sex: '男'
    }

    render() {
        const {name, age, sex} = this.props
        return (
            <ul>
                <li>姓名：{name}</li>  
                <li>年龄：{age}</li>  
                <li>性别：{sex}</li>  
            </ul>
        )
    }
}
ReactDOM.render(<MyComponent name="小王"/>, document.getElementById('test'))
```

### 1.5.3 函数式组件使用props

```jsx
function MyComponent(props) {
    const {name, age, sex} = props
    return (
        <ul>
            <li>姓名: {name}</li>  
            <li>年龄: {age}</li>  
            <li>性别: {sex}</li>  
        </ul>
    )
}

MyComponent.propTypes = {
    name: PropTypes.string,
    age: PropTypes.number,
    sex: PropTypes.string
}

MyComponent.defaultProps = {
    age: 18,
    sex: '男'
}

ReactDOM.render(<MyComponent name="小王"/>, document.getElementById('test'))
```

### 1.5.4 验证类型

基本类型：

```js
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,
```

[更多类型](https://react.docschina.org/docs/typechecking-with-proptypes.html)

## 1.6 refs

### 1.6.1 字符串形式的ref

存在效率问题，将来可能被废弃

```jsx
class MyComponent extends React.Component {
      render() {
        return (
          <div>
            <input type="text" placeholder="点击按钮提示数据" ref="input1"/>  
            <button onClick={this.showValue1}>按钮</button>
            <input type="text" onBlur={this.showValue2} placeholder="失去焦点提示数据" ref="input2" />
          </div>
        )
      }

      showValue1 = () => {
        const {input1} = this.refs
        console.log(input1.value);
      }
      // 当数据就在触发的节点上时，可以用event.target，避免ref的过渡使用
      showValue2 = () => {
        // const {input2} = this.refs
        // console.log(input2.value);
        console.log(event.target.value);
      }

}
ReactDOM.render(<MyComponent/>, document.getElementById('test'))
```

### 1.6.2 回调函数形式的ref

ref接函数，函数的参数就是当前节点

```jsx
class MyComponent extends React.Component {
      render() {
        return (
          <div>
            <input type="text" placeholder="点击按钮提示数据" ref={node => this.input1 = node}/>  
            <button onClick={this.showValue1}>按钮</button>
            <input type="text" onBlur={this.showValue2} placeholder="失去焦点提示数据" ref="input2" />
          </div>
        )
      }

      showValue1 = () => {
        const {input1} = this
        console.log(input1.value);
        console.log(this);
      }
      showValue2 = () => {
        // const {input2} = this.refs
        // console.log(input2.value);
        console.log(event.target.value);
      }

    }
    ReactDOM.render(<MyComponent/>, document.getElementById('test'))
```

### 1.6.3 createRef()

```jsx
class MyComponent extends React.Component {

      // 需要多少ref，就调用多少次React.createRef()
      myRef = React.createRef()
      myRef1 = React.createRef()

      render() {
        return (
          <div>
            <input type="text" placeholder="点击按钮提示数据" ref={this.myRef}/>  
            <button onClick={this.showValue1}>按钮</button>
            <input type="text" onBlur={this.showValue2} placeholder="失去焦点提示数据" ref={this.myRef1} />
          </div>
        )
      }

      showValue1 = () => {
        // myRef是一个容器，myRef.current指向当前节点
        console.log(this.myRef);
        console.log(this.myRef.current.value);
      }
      showValue2 = () => {
        console.log(this.myRef1.current.value);
      }
}
ReactDOM.render(<MyComponent/>, document.getElementById('test'))
```

## 1.7 事件处理

(1).通过onXxx属性指定事件处理函数(注意大小写)

​      a.React使用的是自定义(合成)事件, 而不是使用的原生DOM事件 —————— 为了更好的兼容性

​      b.React中的事件是通过事件委托方式处理的(委托给组件最外层的元素) ————————为了的高效

(2).通过event.target得到发生事件的DOM元素对象 ——————————不要过度使用ref

## 1.8 表单收集数据

### 1.8.1 非受控组件

数据随用随取，与state无关

```jsx
class Login extends React.Component {
      render() {
        return (
          <form onSubmit={this.handleSubmit}>
            用户名：<input type="text" placeholder="请输入用户名" 
                          ref={node => this.username=node}/> 
            密码：<input type="text" placeholder="请输入密码" 
                          ref={node => this.password=node} />
            <button>登录</button>
          </form>
        );
      }

      handleSubmit = (event) => {
        event.preventDefault()
        const {username, password} = this
        console.log(`用户名为：${username.value}, 密码为：${password.value}`);
      }
}
ReactDOM.render(<Login/>, document.getElementById('test'))
```

### 1.8.2 受控组件

受控组件，将表单数据保存在state中，要用的时候再从state中取

```jsx
class Login extends React.Component {
      
      state = {
        username: '',
        password: ''
      }

      render() {
        return (
          <form onSubmit={this.handleSubmit}>
            用户名：<input type="text" placeholder="请输入用户名" 
                          ref={node => this.username=node}
                          onChange={this.saveUserName} /> 
            密码：<input type="text" placeholder="请输入密码" 
                          ref={node => this.password=node} 
                          onChange={this.savePassword} />
            <button>登录</button>
          </form>
        );
      }

      handleSubmit = (event) => {
        event.preventDefault()
        const {username, password} = this.state
        console.log(`用户名为：${username}, 密码为：${password}`);
      }

      savePassword = (event) => {
        this.setState({password: event.target.value})
      }

      saveUserName = (event) => {
        this.setState({username: event.target.value})
      }

}
ReactDOM.render(<Login/>, document.getElementById('test'))
```

## 1.9 函数柯里化

```
高阶函数：如果一个函数符合下面2个规范中的任何一个，那该函数就是高阶函数。
1.若A函数，接收的参数是一个函数，那么A就可以称之为高阶函数。
2.若A函数，调用的返回值依然是一个函数，那么A就可以称之为高阶函数。
常见的高阶函数有：Promise、setTimeout、arr.map()等等

函数的柯里化：通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式。 
function sum(a){
	return(b)=>{
		return (c)=>{
			return a+b+c
			}
		}
	}
```

```jsx
class Login extends React.Component {

      state = {
        username: '',
        password: ''
      }
      
      render() {
        return (
          <form onSubmit={this.handleSubmit}>
            用户名：<input type="text" placeholder="请输入用户名" 
                          ref={node => this.username=node}
                          onChange={this.saveFormData('username')} /> 
            密码：<input type="text" placeholder="请输入密码" 
                          ref={node => this.password=node} 
                          onChange={this.saveFormData('password')} />
            <button>登录</button>
          </form>
        );
      }

      handleSubmit = (event) => {
        event.preventDefault()
        const {username, password} = this.state
        console.log(`用户名为：${username}, 密码为：${password}`);
      }

      /* 通用的保存表单数据方法 */
      // 调用时将表单数据类型传入
      saveFormData = (dataType) => {
        // onChange绑定的回调函数是返回的内部函数
        return event => {
          this.setState({[dataType]: event.target.value})
        }
      }


}
ReactDOM.render(<Login/>, document.getElementById('test'))
```

不用柯里化的实现：

```jsx
class Login extends React.Component {

      state = {
        username: '',
        password: ''
      }
      
      render() {
        return (
          <form onSubmit={this.handleSubmit}>
            用户名：<input type="text" placeholder="请输入用户名" 
                          ref={node => this.username=node}
                          onChange={event => this.saveFormData('username', event)} /> 
            密码：<input type="text" placeholder="请输入密码" 
                          ref={node => this.password=node} 
                          onChange={event => this.saveFormData('password', event)} />
            <button>登录</button>
          </form>
        );
      }

      handleSubmit = (event) => {
        event.preventDefault()
        const {username, password} = this.state
        console.log(`用户名为：${username}, 密码为：${password}`);
      }

      /* 通用的保存表单数据方法 */
      // 在调用的时候，将dataType和event都传入
      saveFormData = (dataType, event) => {
        this.setState({[dataType]: event.target.value})
      }

}
ReactDOM.render(<Login/>, document.getElementById('test'))
```

## 1.10 生命周期

### 1.10.1 旧版

![2_react生命周期(旧)](C:\documents\编程\React\源码\react_basic\12_组件的生命周期\2_react生命周期(旧).png)

```
1. 初始化阶段: 由ReactDOM.render()触发---初次渲染
        1.	constructor()
        2.	componentWillMount()
        3.	render()
        4.	componentDidMount() =====> 常用
一般在这个钩子中做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息
2. 更新阶段: 由组件内部this.setSate()或父组件render触发
        1.	shouldComponentUpdate()
        2.	componentWillUpdate()
        3.	render() =====> 必须使用的一个
        4.	componentDidUpdate()
3. 卸载组件: 由ReactDOM.unmountComponentAtNode()触发
		1.	componentWillUnmount()  =====> 常用
一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消息
```

```jsx
//创建组件
class Count extends React.Component{

    //构造器
    constructor(props){
        console.log('Count---constructor');
        super(props)
        //初始化状态
        this.state = {count:0}
    }

    //加1按钮的回调
    add = ()=>{
        //获取原状态
        const {count} = this.state
        //更新状态
        this.setState({count:count+1})
    }

    //卸载组件按钮的回调
    death = ()=>{
        ReactDOM.unmountComponentAtNode(document.getElementById('test'))
    }

    //强制更新按钮的回调
    force = ()=>{
        this.forceUpdate()
    }

    //组件将要挂载的钩子
    componentWillMount(){
        console.log('Count---componentWillMount');
    }

//组件挂载完毕的钩子
componentDidMount(){
    console.log('Count---componentDidMount');
}

//组件将要卸载的钩子
componentWillUnmount(){
    console.log('Count---componentWillUnmount');
}

//控制组件更新的“阀门”，默认返回true
shouldComponentUpdate(){
    console.log('Count---shouldComponentUpdate');
    return true
}

//组件将要更新的钩子
componentWillUpdate(){
    console.log('Count---componentWillUpdate');
}

//组件更新完毕的钩子
componentDidUpdate(){
    console.log('Count---componentDidUpdate');
}

render(){
    console.log('Count---render');
    const {count} = this.state
    return(
        <div>
            <h2>当前求和为：{count}</h2>
            <button onClick={this.add}>点我+1</button>
            <button onClick={this.death}>卸载组件</button>
            <button onClick={this.force}>不更改任何状态中的数据，强制更新一下</button>
        </div>
    )
}
}

//父组件A
class A extends React.Component{
    //初始化状态
    state = {carName:'奔驰'}

changeCar = ()=>{
    this.setState({carName:'奥拓'})
}

render(){
    return(
        <div>
            <div>我是A组件</div>
            <button onClick={this.changeCar}>换车</button>
            <B carName={this.state.carName}/>
        </div>
    )
}
}

//子组件B
class B extends React.Component{
    //组件将要接收新的props的钩子，第一次传的不算
    componentWillReceiveProps(props){
        console.log('B---componentWillReceiveProps',props);
    }

    //控制组件更新的“阀门”
    shouldComponentUpdate(){
        console.log('B---shouldComponentUpdate');
        return true
    }
    //组件将要更新的钩子
    componentWillUpdate(){
        console.log('B---componentWillUpdate');
    }

    //组件更新完毕的钩子
    componentDidUpdate(){
        console.log('B---componentDidUpdate');
    }

    render(){
        console.log('B---render');
        return(
            <div>我是B组件，接收到的车是:{this.props.carName}</div>
        )
    }
}

//渲染组件
ReactDOM.render(<Count/>,document.getElementById('test'))
```

### 1.10.2 新版

![3_react生命周期(新)](C:\documents\编程\React\源码\react_basic\12_组件的生命周期\3_react生命周期(新).png)

```
1. 初始化阶段: 由ReactDOM.render()触发---初次渲染
        1.	constructor()
        2.	getDerivedStateFromProps 
        3.	render()
        4.	componentDidMount() =====> 常用
一般在这个钩子中做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息
2. 更新阶段: 由组件内部this.setSate()或父组件重新render触发
        1.	getDerivedStateFromProps
        2.	shouldComponentUpdate()
        3.	render()
        4.	getSnapshotBeforeUpdate
        5.	componentDidUpdate()
3. 卸载组件: 由ReactDOM.unmountComponentAtNode()触发
		1.	componentWillUnmount()  =====> 常用
一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消息
```

```jsx
class Count extends React.Component{
			
    //构造器
    constructor(props){
        console.log('Count---constructor');
        super(props)
        //初始化状态
        this.state = {count:0}
    }

    //加1按钮的回调
    add = ()=>{
        //获取原状态
        const {count} = this.state
        //更新状态
        this.setState({count:count+1})
    }

    //卸载组件按钮的回调
    death = ()=>{
        ReactDOM.unmountComponentAtNode(document.getElementById('test'))
    }

    //强制更新按钮的回调
    force = ()=>{
        this.forceUpdate()
    }

    //若state的值在任何时候都取决于props，那么可以使用getDerivedStateFromProps
    static getDerivedStateFromProps(props,state){
        console.log('getDerivedStateFromProps',props,state);
        return null
    }

    //在更新之前获取快照
    getSnapshotBeforeUpdate(){
        console.log('getSnapshotBeforeUpdate');
        return 'atguigu'
    }

    //组件挂载完毕的钩子
    componentDidMount(){
        console.log('Count---componentDidMount');
    }

    //组件将要卸载的钩子
    componentWillUnmount(){
        console.log('Count---componentWillUnmount');
    }

    //控制组件更新的“阀门”
    shouldComponentUpdate(){
        console.log('Count---shouldComponentUpdate');
        return true
    }

    //组件更新完毕的钩子
    componentDidUpdate(preProps,preState,snapshotValue){
        console.log('Count---componentDidUpdate',preProps,preState,snapshotValue);
    }

    render(){
        console.log('Count---render');
        const {count} = this.state
        return(
            <div>
                <h2>当前求和为：{count}</h2>
                <button onClick={this.add}>点我+1</button>
                <button onClick={this.death}>卸载组件</button>
                <button onClick={this.force}>不更改任何状态中的数据，强制更新一下</button>
            </div>
        )
    }
}

//渲染组件
ReactDOM.render(<Count count={199}/>,document.getElementById('test'))
```

# 2. 脚手架

快速创建应用

```bash
npx create-react-app my-app（项目名）
cd my-app
npm start/yarn start
```

## 2.1 脚手架自带文件

```
public ---- 静态资源文件夹
		favicon.icon ------ 网站页签图标
		index.html -------- 主页面
		logo192.png ------- logo图
		logo512.png ------- logo图
		manifest.json ----- 应用加壳的配置文件
		robots.txt -------- 爬虫协议文件
src ---- 源码文件夹
		App.css -------- App组件的样式
		App.js --------- App组件
		App.test.js ---- 用于给App做测试
		index.css ------ 样式
		index.js ------- 入口文件
		logo.svg ------- logo图
		reportWebVitals.js
			--- 页面性能分析文件(需要web-vitals库的支持)
		setupTests.js
			---- 组件单元测试的文件(需要jest-dom库的支持)
```

index.html内容

```html
<!DOCTYPE html>
<html lang="en">
  <head>
		<meta charset="utf-8" />
		<!-- %PUBLIC_URL%代表public文件夹的路径 -->
		<link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
		<!-- 开启理想视口，用于做移动端网页的适配 -->
		<meta name="viewport" content="width=device-width, initial-scale=1" />
		<!-- 用于配置浏览器页签+地址栏的颜色(仅支持安卓手机浏览器) -->
    <meta name="theme-color" content="red" />
    <meta
      name="description"
      content="Web site created using create-react-app"
		/>
		<!-- 用于指定网页添加到手机主屏幕后的图标 -->
		<link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
		<!-- 应用加壳时的配置文件 -->
		<link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <title>React App</title>
  </head>
  <body>
		<!-- 若浏览器不支持js则展示标签中的内容 -->
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <!-- 组件的容器 -->
    <div id="root"></div>
  </body>
</html>

```

## 2.2 toDoList案例

功能

* 头部新增任务
* 是否勾选
* 悬浮改变背景色
* 悬浮显示删除按钮
* 删除功能
* 底部已完成/完成数量
* 所有任务勾选，全选自动勾选
* 全选勾选，所有任务勾选
* 清除已完成任务

通信方式：

* 子组件使用父组件的数据，使用props接收数据
* 子组件改变父组件的数据，使用props接收一个函数，函数的this绑定在父组件身上



### 2.2.1 App.js

```js
// 先引入第三方库
import React, {Component} from 'react'
// 再引入自己写的组件，引入文件夹时，自动会去找其中的index.js/index.jsx
import Header from './components/Header'
import List from './components/List'
import Footer from './components/Footer'
// 最后导入样式
import './App.css';

class App extends Component {

  // 初始化状态，状态在哪，修改状态就在哪
  state = {
    toDoList: [
      {id: '001', name: '打扑克', done: false},
      {id: '002', name: '写代码', done: false},
      {id: '003', name: '写论文', done: false},
      {id: '004', name: '吃饭', done: true}
    ]
  }

  // toDoList首部添加任务
  addToDoObj = (toDoObj) => {
    const {toDoList} = this.state
    const newToDoList = [toDoObj, ...toDoList]
    this.setState({toDoList: newToDoList}) 
  }

  // 传入id和是否勾选，更改对应的任务
  updateCheck = (id, done) => {
    const {toDoList} = this.state
    const newToDoList =  toDoList.map(item => {
      if(item.id === id) {
        return {...item, done}
      }else {
        return item
      }
    })
    this.setState({toDoList: newToDoList})
  }
  
  // 根据id删除某个任务
  deleteObj = id => {
    const {toDoList} = this.state
    const newToDoList = toDoList.filter(item => item.id !== id)
    this.setState({toDoList: newToDoList})
  }

  // 处理全选的勾选与否
  checkAll = (done) => {
    const {toDoList} = this.state
    const newToDoList = toDoList.map(item => {
      return {...item, done}
    })
    this.setState({toDoList: newToDoList})
  }

  // 清除所有任务
  deleteAllChecked = () => {
    const newToDoList = this.state.toDoList.filter(item => !item.done)
    this.setState({toDoList: newToDoList})
  }

  render() {
    const {toDoList} = this.state
    return (
      <div className="todo-container">
        <div className="todo-wrap">
          <Header addToDoObj={this.addToDoObj} />
          <List toDoList={toDoList} updateCheck={this.updateCheck}
                deleteObj={this.deleteObj} />
          <Footer toDoList={toDoList} checkAll={this.checkAll} 
                  deleteAllChecked={this.deleteAllChecked} />
        </div>
      </div>
    );
  }
  
}

export default App;

```

### 2.2.2 Header

```jsx
import React, {Component} from 'react'
import PropTypes from 'prop-types'
import {nanoid} from 'nanoid'
import './index.css'

export default class Header extends Component {

  static propTypes = {
    addToDoObj: PropTypes.func.isRequired
  }

  // 按回车键，增加列表项
  handleKeyUp = (event) => {
    const {keyCode, target} = event
    if(keyCode !== 13) return 
    if(target.value.trim() === '') {
      alert('输入不能为空')
    }
    // yarn add nanoid, nanoid()生成唯一id
    const toDoObj = {id: nanoid(), name: target.value, done: false}
    this.props.addToDoObj(toDoObj)
    target.value = ''
  }

  render() {
    return (
      <div className="todo-header">
        <input type="text" placeholder="输入新增任务，按回车键确认" 
            onKeyUp={this.handleKeyUp} />
      </div>
    )
  }
}
```

### 2.2.3 Item

```jsx
import React, {Component} from 'react'

import './index.css'

export default class Item extends Component {

  // mouse标识鼠标是否移入
  state = {mouse: false}

  // 将更改勾选的id和checked向外传
  handleCheck = id => {
    return event => {
      this.props.updateCheck(id, event.target.checked)
    }
  }

  // 改变鼠标是否移入
  handleMouse = flag => {
    return () => {
      this.setState({mouse: flag})
    }
  }

  // 将点击删除向外传
  handleDelete = id => {
    const {deleteObj} = this.props
    return () => {
      deleteObj(id)
    }
  } 
  render() {
    const {id, name, done} = this.props
    const {mouse} = this.state
    return (
      <li onMouseEnter={this.handleMouse(true)} onMouseLeave={this.handleMouse(false)}> 
        <label>
          <input type="checkbox" checked={done} onChange={this.handleCheck(id)}/>
          <span>{name}</span>
        </label>
        <button className="btn btn-danger" 
            style={{display: mouse ? 'block' : 'none'}}
            onClick={this.handleDelete(id)} >删除</button>
      </li>
    )
  }
}
```

### 2.2.4 List

```jsx
import React, {Component} from 'react'
import Item from '../Item'
import './index.css'

export default class List extends Component {
  render() {
    const {toDoList, updateCheck, deleteObj} = this.props
    return (
      <ul className="todo-main">  
        {
          toDoList.map(item => {
            return <Item {...item} key={item.id} updateCheck={updateCheck}
                      deleteObj={deleteObj} />
          })
        }
      </ul>
    )
  }
}

```

### 2.2.5 Footer

```jsx
import React, {Component} from 'react'
import './index.css'

export default class Footer extends Component {
  
  // 将全选与否向外传递
  handleCheckAll = event => {
    const {checkAll} = this.props
    checkAll(event.target.checked)
  }

  // 清除已完成任务
  handleDeleteChecked = () => {
    this.props.deleteAllChecked()
  }
  
  render() {
    const {toDoList} = this.props
    const checkedNumber = toDoList.filter(item => item.done === true).length
    const isCheckAll = checkedNumber === toDoList.length
    return (
      <div className="todo-footer">
				<label>
					<input type="checkbox" checked={isCheckAll} onChange={this.handleCheckAll} />
				</label>
				<span>已完成{checkedNumber}</span>/全部{toDoList.length}
				<button className="btn btn-danger" onClick={this.handleDeleteChecked}>清除已完成任务</button>
			</div>
    )
  }
}
```



# 4. 路由



# UI

# Redux

# yarn命令

```shell
yarn add 
yarn start
```









# 10. 问题

* React_dev_tools检测不了本地写的代码
  * 打开扩展，找到react_dev_tools，打开相应权限

* 通常，在 React 中，构造函数仅用于以下两种情况：

```js
// 通过给 this.state 赋值对象来初始化内部 state。
this.state = {isHot:false,wind:'微风'}
// 为事件处理函数绑定实例
this.changeWeather = this.changeWeather.bind(this)
```

二者都可以通过其他方式实现，直接给state赋值，使用箭头函数声明函数