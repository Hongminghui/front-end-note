# react-router

文档：

[React Router: Declarative Routing for React.js](https://reactrouter.com/web/guides/quick-start)

[ReactTraining/react-router: Declarative routing for React (github.com)](https://github.com/ReactTraining/react-router)

## 1. 路由历史

### 1.1 后端路由阶段

* 早期的网站开发整个HTML页面是由服务器来渲染的. 
  * 服务器直接生产渲染好对应的HTML页面, 返回给客户端进行展示. 
* 但是, 一个网站, 这么多页面服务器如何处理呢? 
  *  一个页面有自己对应的网址, 也就是URL. 
  * URL会发送到服务器, 服务器会通过正则对该URL进行匹配, 并且最后交给一个Controller进行处理. 
  *  Controller进行各种处理, 最终生成HTML或者数据, 返回给前端. 
  * 这就完成了一个IO操作. 
* 上面的这种操作, 就是后端路由. 
  * 当我们页面中需要请求不同的路径内容时, 交给服务器来进行处理, 服务器渲染好整个页面, 并且将页面返回给客户顿. 
  * 这种情况下渲染好的页面, 不需要单独加载任何的js和css, 可以直接交给浏览器展示, 这样也有利于SEO的优化. 
* 后端路由的缺点: 
  * 一种情况是整个页面的模块由后端人员来编写和维护的. 
  * 另一种情况是前端开发人员如果要开发页面, 需要通过PHP和Java等语言来编写页面代码. 
  * 而且通常情况下HTML代码和数据以及对应的逻辑会混在一起, 编写和维护都是非常糟糕的事情

### 1.2 前后端分离阶段

* 前端渲染的理解： 
  * 每次请求涉及到的静态资源都会从静态资源服务器获取； 
  * 这些资源包括HTML+CSS+JS，然后在前端对这些请求回来的资源进行渲染； 
  * 需要注意的是，客户端的每一次请求，都会从静态资源服务器请求文件； 
  * 同时可以看到，和之前的后断路由不同，这时后端只是负责提供API了； 
* 前后端分离阶段： 
  * 随着Ajax的出现, 有了前后端分离的开发模式； 
  * 后端只提供API来返回数据，前端通过Ajax获取数据，并且可以通过JavaScript将数据渲染到页面中； 
  *  这样做最大的优点就是前后端责任的清晰，后端专注于数据上，前端专注于交互和可视化上；
  * 并且当移动端(iOS/Android)出现后，后端不需要进行任何处理，依然使用之前的一套API即可； 
  * 目前很多的网站依然采用这种模式开发（jQuery开发模式）

### 1.3 单页面富应用（SPA）

* 单页面富应用的理解： 
  * 单页面富应用的英文是single-page application，简称SPA； 
  * 整个Web应用只有实际上只有一个页面，当URL发生改变时，并不会从服务器请求新的静态资源； 
  * 而是通过JavaScript监听URL的改变，并且根据URL的不同去渲染新的页面； 
* 如何可以应用URL和渲染的页面呢？前端路由 
  * 前端路由维护着URL和渲染页面的映射关系； 
  * 路由可以根据不同的URL，最终让我们的框架（比如Vue、React、Angular）去渲染不同的组件； 
  * 最终我们在页面上看到的实际就是渲染的一个个组件页面

## 2. 路由原理

### 2.1 hash模式

```html
<div class="app">
    <a href="#/home">首页</a>
    <a href="#/about">关于</a>

    <div class="router-view"></div>
</div>
```

```js
let routerView = document.getElementsByClassName('router-view')[0]
window.addEventListener('hashchange', () => {
    switch(location.hash) {
        case '#/home': 
            routerView.innerHTML = '首页'
            break
        case '#/about': 
            routerView.innerHTML = '关于'
            break
        default: 
            routerView.innerHTML = ''
    }
})
```

### 2.2 history模式

```html
<div id="app">
    <a href="/home">首页</a>
    <a href="/about">关于</a>

    <div class="router-view"></div>
</div>
```

```js
const urlChange = () => {
    switch(location.pathname) {
        case '/home': 
            routerView.innerHTML = '首页'
            break
        case '/about': 
            routerView.innerHTML = '关于'
            break
        default: 
            routerView.innerHTML = ''
    }
}

let routerView = document.getElementsByClassName('router-view')[0]
let aTags = document.getElementsByTagName('a')

// 利用pushState自定义点击a标签后的行为
for(let tag of aTags) {
    tag.onclick = (event) => {
        event.preventDefault()
        const href = tag.href
        history.pushState({}, '', href)
        urlChange()
    }
}

// 监听popstate，浏览器返回按钮
window.addEventListener('popstate', urlChange)
```

## 3. react-router-dom

React Router的版本4开始，路由不再集中在一个包中进行管理了： 

* react-router是router的核心部分代码； 
* react-router-dom是用于浏览器的； 
* react-router-native是用于原生应用的；

安装react-router： 

* 安装react-router-dom会自动帮助我们安装react-router的依赖； 
* yarn add react-router-dom

### 3.1 基本使用

* BrowserRouter或HashRouter
  * Router中包含了对路径改变的监听，并且会将相应的路径传递给子组件；
  * BrowserRouter使用history模式；
  * HashRouter使用hash模式；

* Link和NavLink：
  * 通常路径的跳转是使用Link组件，最终会被渲染成a元素；
  * NavLink是在Link基础之上增加了一些样式属性（后续学习）；
  * to属性：Link中最重要的属性，用于设置跳转到的路径；

* Route： 
  * Route用于路径的匹配；
  * path属性：用于设置匹配到的路径；
  * component属性：设置匹配到路径后，渲染的组件；
  * exact：精准匹配，只有精准匹配到完全一致的路径，才会渲染对应的组件；
  * Route是组件展示的位置

```jsx
import {
  BrowserRouter,
  Link,
  Route
} from 'react-router-dom'

import Home from './pages/home'
import About from './pages/about'
import Profile from './pages/profile'

import './App.css';



function App() {
  return (
    <BrowserRouter>
      <Link to='/'>首页</Link>
      <Link to='/about'>关于</Link>
      <Link to='/profile'>我的</Link>

       {/* 默认是模糊匹配，不加exact，以/开头的路径都会渲染Home组件 */}
      <Route exact path='/' component={Home}></Route>
      <Route path='/about' component={About}></Route>
      <Route path='/profile' component={Profile}></Route>
    </BrowserRouter>
  );
}

export default App;
```

### 3.2 NavLink

将Link改成NavLink，会在路由处于活跃时，自动加上一个active的类

```jsx
<NavLink exact to='/'>首页</NavLink>
<NavLink to='/about'>关于</NavLink>
<NavLink to='/profile'>我的</NavLink>
```

App.css

```css
a.active {
  font-size: 20px;
  color: red;
}
```

* activeStyle：活跃时（匹配时）的样式； 
* activeClassName：活跃时添加的class； 
* exact：是否精准匹配；不加exact，根路径每次都会加上活跃的样式

```jsx
<NavLink exact to="/" activeStyle={{color: "red", fontSize: "30px"}}>首页</NavLink>
<NavLink to="/about" activeStyle={{color: "red", fontSize: "30px"}}>关于</NavLink>
<NavLink to="/profile" activeStyle={{color: "red", fontSize: "30px"}}>我的</NavLink>
```

```jsx
<NavLink exact to="/" activeClassName="link-active">首页</NavLink>
<NavLink to="/about" activeClassName="link-active">关于</NavLink>
<NavLink to="/profile" activeClassName="link-active">我的</NavLink>
```

App.css

```css
a.link-active {
  font-size: 20px;
  color: red;
}
```

### 3.3 Switch

NoMatch组件没加path属性，表示没有路径匹配上时显示这个组件，但是默认情况下，所有能匹配上路径的组件都会被渲染，因此在Route外面套一层Swtich，表示路由唯一匹配。一个路径就只会渲染一直Route了。

```jsx
<Switch>
    <Route exact path='/' component={Home} ></Route>
    <Route path='/about' component={About}></Route>
    <Route path='/profile' component={Profile}></Route>
    <Route component={NoMatch}></Route>
</Switch>
```

### 3.4 Redirect

pages > user.js

```jsx
import React, { PureComponent } from 'react'
import {Redirect} from 'react-router-dom'

export default class User extends PureComponent {
  state = {
    isLogin: false
  }
  render() {
    return this.state.isLogin ? (
      <div>
        <h2>用户</h2>
      </div>
    ) : <Redirect to='/login'/>
  }
}

```

### 3.5 路由嵌套

pages > about.js，在App.js只需要引入about的NavLink，和About组件即可

```jsx
import React, { PureComponent } from 'react'
import { NavLink, Switch, Route } from 'react-router-dom';

export function AboutHistory(props) {
  return <h2>企业成立于2000年, 拥有悠久的历史文化</h2>
}

export function AboutCulture(props) {
  return <h2>创新/发展/共赢</h2>
}

export function AboutContact(props) {
  return <h2>联系电话: 020-68888888</h2>
}

export function AboutJoin(props) {
  return <h2>投递简历到aaaa@123.com</h2>
}

export default class About extends PureComponent {
  render() {
    return (
      <div>
        <NavLink exact to="/about" activeClassName="about-active">企业历史</NavLink>
        <NavLink to="/about/culture" activeClassName="about-active">企业文化</NavLink>
        <NavLink to="/about/contact" activeClassName="about-active">联系我们</NavLink>

        <Switch>
          <Route exact path="/about" component={AboutHistory} />
          <Route path="/about/culture" component={AboutCulture} />
          <Route path="/about/contact" component={AboutContact} />
          <Route path="/about/join" component={AboutJoin} />
        </Switch>
      </div>
    )
  }
}
```

### 3.6 手动路由跳转

点击button，跳转到/join路径

通过Route渲染的组件，this.props上会有history, match, location等额外属性。

```js
export default class About extends PureComponent {
  render() {
    return (
      <div>
        <NavLink exact to="/about" activeClassName="about-active">企业历史</NavLink>
        <NavLink to="/about/culture" activeClassName="about-active">企业文化</NavLink>
        <NavLink to="/about/contact" activeClassName="about-active">联系我们</NavLink>
        <button onClick={e => this.jumpToJoin()}>加入我们</button>

        <Switch>
          <Route exact path="/about" component={AboutHistory} />
          <Route path="/about/culture" component={AboutCulture} />
          <Route path="/about/contact" component={AboutContact} />
          <Route path="/about/join" component={AboutJoin} />
        </Switch>
      </div>
    )
  }

  jumpToJoin() {
    console.log(this.props);
    this.props.history.push('/about/join')
  }
}
```

### 3.7 withRouter

想让普通的组件，也具有history,match,location等属性，可以使用withRouter包装

如果我们希望在App组件中获取到history对象，必须满足一下两个条件： 

* App组件必须包裹在Router(HashRouter或BroswerRouter)组件之内； 
* App组件使用withRouter高阶组件包裹；

App.js

```jsx
import {
  Route,
  NavLink,
  Switch,
  withRouter
} from 'react-router-dom'

import Home from './pages/home'
import About from './pages/about'
import Profile from './pages/profile'
import NoMatch from './pages/noMatch'
import User from './pages/user'
import Login from './pages/login'

import './App.css';

function App(props) {
  cosole.log(props)
  return (
    <div>
      <NavLink exact to='/' activeClassName='link-active'>首页</NavLink>
      <NavLink to='/about' activeClassName='link-active'>关于</NavLink>
      <NavLink to='/profile' activeClassName='link-active'>我的</NavLink>
      <NavLink to='/user' activeClassName='link-active'>用户</NavLink>
      <NavLink to='/login' activeClassName='link-active'>登录</NavLink>


      {/* 默认是模糊匹配，不加exact，以/开头的路径都会渲染Home组件 */}
      <Switch>
        <Route exact path='/' component={Home} ></Route>
        <Route path='/about' component={About}></Route>
        <Route path='/profile' component={Profile}></Route>
        <Route path='/user' component={User}></Route>
        <Route path='/login' component={Login}></Route>
        <Route component={NoMatch}></Route>
      </Switch>
    </div>
  );
}

export default withRouter(App);
```

index.js

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import {BrowserRouter} from 'react-router-dom'

import './index.css';
import App from './App';


ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
);
```

### 3.8 参数传递

#### 3.8.1 动态路由

NavLink > Route

```jsx
function App(props) {
  console.log(props);
  const id = '123'
  return (
    <div>
      <NavLink exact to='/' activeClassName='link-active'>首页</NavLink>
      <NavLink to='/about' activeClassName='link-active'>关于</NavLink>
      <NavLink to='/profile' activeClassName='link-active'>我的</NavLink>
      <NavLink to='/user' activeClassName='link-active'>用户</NavLink>
      <NavLink to='/login' activeClassName='link-active'>登录</NavLink>
      {/* 参数传递 */}
      <NavLink to={`/detail/${id}`} activeClassName='link-active'>详细信息</NavLink>

      {/* 默认是模糊匹配，不加exact，以/开头的路径都会渲染Home组件 */}
      <Switch>
        <Route exact path='/' component={Home} ></Route>
        <Route path='/about' component={About}></Route>
        <Route path='/profile' component={Profile}></Route>
        <Route path='/user' component={User}></Route>
        <Route path='/login' component={Login}></Route>
        {/* 参数传递 */}
        <Route path='/detail/:id' component={Detail}></Route>
        <Route component={NoMatch}></Route>
      </Switch>
    </div>
  );
}
```

使用参数：

detail.js组件

```jsx
import React, { Component } from 'react'

export default class Detail extends Component {
  render() {
    const {match} = this.props
    return (
      <div>
        <h2>{`Detail: ${match.params.id}`}</h2>
      </div>
    )
  }
}

```

#### 3.8.2 search参数

```jsx
<NavLink to={`/detail2?name=why&age=18`} activeClassName="link-active">详情2</NavLink>
...
<Route path="/detail2" component={Detail2} />
```

使用：

pages > detail2.js

```jsx
import React, { PureComponent } from 'react'

export default class Detail2 extends PureComponent {
  render() {
    console.log(this.props.location);

    return (
      <div>
        <h2>Detail2: {this.props.location.search}</h2>
      </div>
    )
  }
}
```

#### 3.8.3 to属性传对象

```jsx
<NavLink to={{
        pathname: '/detail2',
        search: 'name=abc',
        state: info
      }} activeClassName='link-active'>详细信息2</NavLink>
...
<Route path='/detail2' component={Detail2}></Route>
```

使用：

pages > detail2.js

```jsx
import React, { Component } from 'react'

export default class Detail2 extends Component {
  render() {
    const {location} = this.props
    return (
      <div>
        {/* state对象就是传入的info对象 */}
        <h2>{`Detail: ${location.state.age}`}</h2>
      </div>
    )
  }
}
```

### 3.9 react-router-config

安装：



#### 3.9.1 配置路由映射表

src > router > index.js

```jsx
import Home from '../pages/home'
import About, {
  AboutHistory,
  AboutContact,
  AboutCulture,
  AboutJoin
} from '../pages/about'
import Profile from '../pages/profile'
import User from '../pages/user'
import Detail from '../pages/detail'
import Detail2 from '../pages/detail2'

const routes = [
  {
    path: '/',
    exact: true,
    component: Home
  },
  {
    path: '/about',
    component: About,
    routes: [
      {
        path: '/about',
        component: AboutHistory
      },
      {
        path: '/about/culture',
        component: AboutCulture
      },
      {
        path: '/about/contact',
        component: AboutContact
      },
      {
        path: '/about/join',
        component: AboutJoin
      },
    ]
  },
  {
    path: '/profile',
    component: Profile
  },
  {
    path: '/user',
    component: User
  },
  {
    path: '/detail',
    component: Detail
  },
  {
    path: '/detail2',
    component: Detail2
  }
]

export default routes
```

#### 3.9.2 使用

使用renderRoutes(routes)代替原来的Swtich包裹组件

App.js

```jsx
import {
  Route,
  NavLink,
  Switch,
  withRouter
} from 'react-router-dom'

import {renderRoutes} from 'react-router-config'


import routes from './router/index'
import './App.css';

function App(props) {
  console.log(props);
  const id = '123'
  const info = {age: 13, sex: 'male'}
  return (
    <div>
      <NavLink exact to='/' activeClassName='link-active'>首页</NavLink>
      <NavLink to='/about' activeClassName='link-active'>关于</NavLink>
      <NavLink to='/profile' activeClassName='link-active'>我的</NavLink>
      <NavLink to='/user' activeClassName='link-active'>用户</NavLink>
      <NavLink to='/login' activeClassName='link-active'>登录</NavLink>
      {/* 参数传递 */}
      <NavLink to={`/detail/${id}`} activeClassName='link-active'>详细信息</NavLink>
      <NavLink to={{
        pathname: '/detail2',
        search: 'name=abc',
        state: info
      }} activeClassName='link-active'>详细信息2</NavLink>
      
      {/* 代替原来Switch包裹的组件 */}
      {renderRoutes(routes)}
    </div>
  );
}

export default withRouter(App);

```

#### 3.9.3 子路由使用

pages > about.js

还有另一个API matchRoutes()

```jsx
import React, { PureComponent } from 'react'
import { matchRoutes, renderRoutes } from 'react-router-config';
import { NavLink, Switch, Route } from 'react-router-dom';

export function AboutHistory(props) {
  return <h2>企业成立于2000年, 拥有悠久的历史文化</h2>
}

export function AboutCulture(props) {
  return <h2>创新/发展/共赢</h2>
}

export function AboutContact(props) {
  return <h2>联系电话: 020-68888888</h2>
}

export function AboutJoin(props) {
  return <h2>投递简历到aaaa@123.com</h2>
}

export default class About extends PureComponent {
  render() {
    const branch = matchRoutes(this.props.route.routes, '/about')
    console.log(branch);
    return (
      <div>
        <NavLink exact to="/about" activeClassName="about-active">企业历史</NavLink>
        <NavLink to="/about/culture" activeClassName="about-active">企业文化</NavLink>
        <NavLink to="/about/contact" activeClassName="about-active">联系我们</NavLink>
        <button onClick={e => this.jumpToJoin()}>加入我们</button>

        {/* <Switch>
          <Route exact path="/about" component={AboutHistory} />
          <Route path="/about/culture" component={AboutCulture} />
          <Route path="/about/contact" component={AboutContact} />
          <Route path="/about/join" component={AboutJoin} />
        </Switch> */}
        {renderRoutes(this.props.route.routes)}
      </div>
    )
  }

  jumpToJoin() {
    console.log(this.props);
    this.props.history.push('/about/join')
  }
}

```

