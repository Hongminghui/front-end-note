## Jsx语法

使用jsx，就要导入React

### 1. 注释

```jsx
render() {
    return (
        <div>
            {/* 我是一段注释 */}
            <h2>Hello World</h2>
        </div>
    )
}
```

### 2. 嵌入数据

```jsx
class App extends React.Component {
      constructor(props) {
        super(props);

        this.state = {
          // 1.在{}中可以正常显示显示的内容
          name: "why", // String
          age: 18, // Number
          names: ["abc", "cba", "nba"], // Array

          // 2.在{}中不能显示(忽略)
          test1: null, // null
          test2: undefined, // undefined
          test3: true, // Boolean
          flag: true,

          // 3.对象不能作为jsx的子类
          friend: {
            name: "kobe",
            age: 40
          }
        }
      }

      render() {
        return (
          <div>
            <h2>{this.state.name}</h2>
            <h2>{this.state.age}</h2>
            <h2>{this.state.names}</h2>

            <h2>{this.state.test1 + ""}</h2>
            <h2>{this.state.test2 + ""}</h2>
            <h2>{this.state.test3.toString()}</h2>

            <h2>{this.state.flag ? "你好啊": null}</h2>
            {/* 对象不能展示 */}
            {/*<h2>{this.state.friend}</h2>*/}
          </div>
        )
      }
    }
```

### 3. 嵌入表达式

```jsx
class App extends React.Component {
      constructor(props) {
        super(props);

        this.state = {
          firstname: "kobe",
          lastname: "bryant",
          isLogin: false
        }
      }

      render() {
        const { firstname, lastname, isLogin } = this.state;

        return (
          <div>
            {/*1.运算符表达式*/}
            <h2>{ firstname + " " + lastname }</h2>
            <h2>{20 * 50}</h2>

            {/*2.三元表达式*/}
            <h2>{ isLogin ? "欢迎回来~": "请先登录~" }</h2>

            {/*3.进行函数调用*/}
            <h2>{this.getFullName()}</h2>
          </div>
        )
      }

      getFullName() {
        return this.state.firstname + " " + this.state.lastname;
      }
    }

```

### 4. 绑定属性

```jsx
class App extends React.Component {
      constructor(props) {
        super(props);

        this.state = {
          title: "标题",
          imgUrl: "http://p2.music.126.net/L8IDEWMk_6vyT0asSkPgXw==/109951163990535633.jpg",
          link: "http://www.baidu.com",
          active: true
        }
      }

      render() {
        const { title, imgUrl, link, active } = this.state;
        return (
          <div>
            {/* 1.绑定普通属性 */}
            <h2 title={title}>我是标题</h2>
            <img src={getSizeImage(imgUrl, 140)} alt=""/>
            <a href={link} target="_blank">百度一下</a>

            {/* 2.绑定class */}
            <div className="box title">我是div元素</div>
            <div className={"box title " + (active ? "active": "")}>我也是div元素</div>
            <label htmlFor=""></label>

            {/* 3.绑定style */}
            <div style={{color: "red", fontSize: "50px"}}>我是div,绑定style属性</div>
          </div>
        )
      }
    }
```

### 5. this绑定

#### 5.1 原生与react比较

Html事件处理程序，原生绑定函数的调用，react绑定函数指针

```html
<!-- this指向节点，因为onclick是div节点的属性，相当于是div调用的 -->
<div id="test" onclick="test(this)">Hello World</div>
```

```js
let div = document.getElementById('test')
function test(a) {
    // 这里的this指向window，函数是全局调用的
    console.log(this);
    console.log(a) // 节点
}
```

react

```jsx
render() {
    const {isHot, wind} = this.state
    // render函数由实例调用，所以这里的this指向实例，this.changeWhether会指向组件中的方法
    // 这里的h2不是真正的节点，所以不会绑定给h2
    return <h2 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}, {wind}</h2>
}

// 回调函数由全局调用，严格模式下this绑定在undefined
changeWeather(){
	console.log(this)
}
```

#### 5.2 解决this丢失

```jsx
class App extends React.Component {
      constructor(props) {
        super(props);
        this.state = {
          message: "你好啊",
          counter: 100
        }

        this.btnClick = this.btnClick.bind(this);
      }

      render() {
        return (
          <div>
            {/* 1.方案一: bind绑定this(显示绑定) */}
            <button onClick={this.btnClick}>按钮1</button>
            <button onClick={this.btnClick}>按钮2</button>
            <button onClick={this.btnClick}>按钮3</button>

            {/* 2.方案二: 定义函数时, 使用箭头函数 */}
            <button onClick={this.increment}>+1</button>

            {/* 2.方案三(推荐): 直接传入一个箭头函数, 在箭头函数中调用需要执行的函数
            	相当于绑定了一个匿名函数，执行语句和this.decrement的语句相同，
            	但是this绑定在组件身上了 */}
            <button onClick={() => { this.decrement("why") }}>-1</button>
          </div>
        )
      }

      btnClick() {
        console.log(this.state.message);
      }

      // increment() {
      //   console.log(this.state.counter);
      // }
      // 箭头函数中永远不绑定this
      // ES6中给对象增加属性: class fields
      increment = () => {
        console.log(this.state.counter);
      }

      decrement(name) {
        console.log(this.state.counter, name);
      }
    }
```

### 6. jsx事件绑定传递参数

方式一：使用箭头函数，实际绑定的是箭头函数调用后返回的函数

```jsx
render() {
        return (
          <div>
            <ul>
              {
                this.state.movies.map((item, index, arr) => {
                  return (
                    <li className="item" 
                        onClick={ e => { this.liClick(item, index, e) }}
                        title="li">
                      {item}
                    </li>
                  )
                })
              }
            </ul>
          </div>
        )
      }

      liClick(item, index, event) {
        console.log("li发生了点击", item, index, event);
      }
    }
```

方式二：函数柯里化

```jsx
render() {
        return (
          <div>
            <ul>
              {
                this.state.movies.map((item, index, arr) => {
                  return (
                    <li className="item" 
                        onClick={ this.liClick(item, index) }
                        title="li">
                      {item}
                    </li>
                  )
                })
              }
            </ul>
          </div>
        )
      }

      liClick = (item, index) => {
          return event => {
              console.log(item, index, event)
          }
      }
    }
```

### 7. 实现v-if

```jsx
class App extends React.Component {
      constructor(props) {
        super(props);

        this.state = {
          isLogin: true
        }
      }

      render() {
        const { isLogin } = this.state;

        // 1.方案一:通过if判断: 逻辑代码非常多的情况
        let welcome = null;
        let btnText = null;
        if (isLogin) {
          welcome = <h2>欢迎回来~</h2>
          btnText = "退出";
        } else {
          welcome = <h2>请先登录~</h2>
          btnText = "登录";
        }

        return (
          <div>
            <div>我是div元素</div>

            {welcome}
            {/* 2.方案二: 三元运算符 */}
            <button onClick={e => this.loginClick()}>{isLogin ? "退出" : "登录"}</button>

            <hr />

            <h2>{isLogin ? "你好啊, coderwhy": null}</h2>

            {/* 3.方案三: 逻辑与&& */}
            {/* isLogin为假，值为假，第二个值不渲染；isLogin为真，渲染第二个值，h2这里也可以放一个组件 */}
            <h2>{ isLogin && "你好啊, coderwhy" }</h2>
            { isLogin && <h2>你好啊, coderwhy</h2> }
          </div>
        )
      }

      loginClick() {
        this.setState({
          isLogin: !this.state.isLogin
        })
      }
    }
```

```js
{this.state.isShow && <Cpn/>}
```



### 8. 实现v-show

依靠三元运算符实现

```jsx
class App extends React.Component {
      constructor(props) {
        super(props);

        this.state = {
          isLogin: true
        }
      }

      render() {
        const { isLogin} = this.state;
        // 后续通过display设置是否展示
        const titleDisplayValue = isLogin ? "block": "none";
        return (
          <div>
            <button onClick={e => this.loginClick()}>{isLogin ? "退出": "登录"}</button>
            <h2 style={{display: titleDisplayValue}}>你好啊, coderwhy</h2>
          </div>
        )
      }

      loginClick() {
        this.setState({
          isLogin: !this.state.isLogin
        })
      }
    }
```

### 9. 实现v-for

```jsx
class App extends React.Component {
      constructor(props) {
        super(props);

        this.state = {
          names: ["abc", "cba", "nba", "mba", "dna"],
          numbers: [110, 123, 50, 32, 55, 10, 8, 333]
        }
      }

      render() {
        return (
          <div>
            <h2>名字列表</h2>
            <ul>
              {
                this.state.names.map(item => {
                  return <li>{item}</li>
                })
              }
            </ul>

            <h2>数字列表(过滤1)</h2>
            <ul>
              {
                this.state.numbers.filter(item => {
                  return item >= 50;
                }).map(item => {
                  return <li>{item}</li>
                })
              }
            </ul>

            <h2>数字列表(过滤2)</h2>
            <ul>
              {
                this.state.numbers.filter(item => item >= 50).map(item => <li>{item}</li>)
              }
            </ul>

            <h2>数字列表(截取)</h2>
            <ul>
              {
                this.state.numbers.slice(0, 4).map(item => {
                  return <li>{item}</li>
                })
              }
            </ul>
          </div>
        )
      }
    }
```

### 10. 实现动态绑定样式

```
<h2 className={ 'title ' + (isActive ? 'active' : '')}></h2>
```

