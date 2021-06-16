## 一. Redux测试项目

流程图：

![image-20210422184646763](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20210422184646763.png)



官网图：![image-20210422184833376](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20210422184833376.png)

### 1. 初始化

创建redux-demo目录

```shell
npm init -y
yarn add redux
```

建立store目录，包含以下四个文件

* constant.js	负责存放常量
* actionCreators    负责存放action，决定执行哪一种事件
* reducer    负责联系state和action，负责事件的集中处理，具体执行
* index    根据reducer创建store，并导出

### 2. constant.js

```js
export const ADD_NUMBER = "ADD_NUMBER"
export const SUB_NUMBER = "SUB_NUMBER"
export const INCREMENT = "INCREMENT"
export const DECREMENT = "DECREMENT"
```

### 3. actionCreators.js

```js
// node使用import导入模块，需要在package.json中做相应修改
// 并且导入的文件需要加后缀名
import {
  ADD_NUMBER,
  SUB_NUMBER,
  DECREMENT,
  INCREMENT
} from './constant.js'

// 实际的action是对象形式的，使用的时候会使用调用后的返回值
// 写成函数的形式更灵活
export const addAction = num => ({
  type: ADD_NUMBER,
  num
})

export const subAction = num => ({
  type: SUB_NUMBER,
  num
})

export const incAction = () => ({
  type: INCREMENT
})

export const decAction = () => ({
  type: DECREMENT
})
```

### 4. reducer.js

```js
import {
  ADD_NUMBER,
  SUB_NUMBER,
  INCREMENT,
  DECREMENT
} from './constant.js'

const defaultState = {
  counter: 0
}

function reducer(state = defaultState, action) {
  switch(action.type) {
    case ADD_NUMBER: 
      return {...state, counter: state.counter + action.num}
    case SUB_NUMBER: 
      return {...state, counter: state.counter - action.num}
    case INCREMENT: 
      return {...state, counter: state.counter + 1}
    case DECREMENT: 
      return {...state, counter: state.counter - 1}
    default: 
      return state
  }
}

export default reducer
```

### 5. index.js

```js
// node中导入方式，webpack中使用{redux}导入
import redux from 'redux'

import reducer from './reducer.js'

const store = redux.createStore(reducer)

export default store
```

### 6. index.js(根目录)

```js
import store from './store/index.js'

import {
  addAction,
  subAction,
  incAction,
  decAction
} from './store/actionCreators.js'

// 在派发之前订阅，每次派发事件，就会执行传入的函数了
store.subscribe(() => {
  // 获取state
  console.log(store.getState());
})

// 派发事件
store.dispatch(addAction(5))
store.dispatch(subAction(3))
store.dispatch(incAction())
store.dispatch(decAction())
```

## 二. React-redux 手动联系不同组件

store目录和之前几乎完全相同，只是导入的内容有些许改变

store > index.js, 导入redux，变成导入createStore

```js
import { createStore } from 'redux'

import reducer from './reducer.js'

const store = createStore(reducer)

ue default store
```

实现功能：有两个组件About, Home，共同依赖store里的数据，当About发生改变时，Home也要同样改变。

思路：消息的发布与订阅，在两个组件的生命周期中都实现订阅与取消订阅，实现状态集中管理与同步更新。

### 1. about.js

```js
import React, { PureComponent } from 'react'

import store from '../store'

import {
  subAction
} from "../store/actionCreators";

export default class About extends PureComponent {
  state = {
    counter: store.getState().counter
  }

  // 分发任务只是改变了store中的state，还是得setState改变组件的state
  componentDidMount() {
    // subscribe会返回一个函数，用于取消订阅
    this.unSubscribe = store.subscribe(() => {
      this.setState({counter: store.getState().counter})
    })
  }

  componentWillUnmount() {
    this.unSubscribe()
  }

  render() {
    return (
      <div>
        <h1>About</h1>
        <h2>当前计数: {this.state.counter}</h2>
        <button onClick={e => this.decrement()} style={{margin: '5px'}}>-1</button>
        <button onClick={e => this.minusFive()}>-5</button>
      </div>
    )
  }

  decrement() {
    store.dispatch(subAction(1))
    console.log(this.state.counter);
  }

  minusFive() {
    store.dispatch(subAction(5))
    console.log(this.state.counter);
  }
}
```

### 2. home.js

```js
import React, { PureComponent } from 'react'

import store from "../store";

export default class Home extends PureComponent {

  state = {
    counter: store.getState().counter
  }

  componentDidMount() {
    this.unSubscribe = store.subscribe(() => {
      this.setState({counter: store.getState().counter})
    })
  }

  componentWillUnmount() {
    this.unSubscribe()
  }

  render() {
    return (
      <div>
        <h1>Home</h1>
        <h2>当前计数: {this.state.counter}</h2>
      </div>
    )
  }
}

```

### 3. App.js

```js
import React, { Component } from 'react'

import About from './pages/about1'
import Home from './pages/home1-手动实现联系redux'

export default class App extends Component {
  render() {
    return (
      <div>
        <About/>
        <hr/>
        <Home/>
      </div>
    )
  }
}
```

缺陷：每个需要使用store里数据的，都需要在生命周期里监听和取消，重复性代码太多。

## 三. React-redux自定义connect函数

将上述重复的代码都抽取到connect函数里

### 1. connect.js

src > utils > connect.js

```js
/* 把需要集中管理的state，订阅，取消订阅，封装派发任务的操作都交给connect */

import React, {PureComponent} from 'react'
import {StoreContext} from './context'

/* connect()(): 即可返回带有集中管理的数据，订阅，取消订阅，带有封装dispatch(action)函数的组件 */
// mapStateToProps: 接受state做参数，返回state中需要传递的属性
// this.context: 为了减少耦合度，store会通过context传递，而不是直接传递
// mapDispatchToProps: 接受dispatch(store.dispatch)做参数，
//                     返回几个调用dispatch派送action的函数
export default function connect(mapStateToProps, mapDispatchToProps) {
  return function EnhanceHOC(WrappedComponent) {
    class EnhanceComponent extends PureComponent {
      state = {
        storeState: mapStateToProps(this.context.getState())
      }

      // 添加订阅
      componentDidMount() {
        this.unSubscribe = this.context.subscribe(() => {
          this.setState({
            storeState: mapStateToProps(this.context.getState())
          })
        })
      }

      // 取消订阅
      componentWillUnmount() {
        this.unSubscribe()
      }

      render() {
        return <WrappedComponent {...this.props}
                                 {...mapStateToProps(this.context.getState())}
                                 {...mapDispatchToProps(this.context.dispatch)}
        
              />
      }
    }

    EnhanceComponent.contextType = StoreContext
    return EnhanceComponent
  }
}
```

### 2. context.js

src > utils > context.js

使用context来将store传给connect，避免store(业务代码,每次都不一样)与connect耦合度太高。这样每次传不同的store就行了，可以提高connect的通用性。

```js
import React from 'react'

const StoreContext = React.createContext()

export {
  StoreContext
}
```

### 3. index.js

src > index.js

在index.js里把store传入，不同的业务可以传入不同的store

```js
import React from 'react';
import ReactDOM from 'react-dom';

import store from './store'
import { StoreContext } from './utils/context'

import App from './App';

ReactDOM.render(
  <StoreContext.Provider value={store}>
    <App/>
  </StoreContext.Provider>,
  document.getElementById('root')
);
```

### 4. about.js

src > pages > about.js

```jsx
import React from 'react'

// import store from '../store'
import  connect from '../utils/connect'

import {
  subAction
} from "../store/actionCreators";

function About(props) {
  return (
    <div>
      <h1>About</h1>
      <h2>当前计数: {props.counter}</h2>
      <button onClick={e => props.decrement()} style={{ margin: '5px' }}>-1</button>
      <button onClick={e => props.minusFive()}>-5</button>
    </div>
  )
}

/* 这两个函数的返回值，都将作为props传入About组件 */
// state: connect中会将store.getState()传入
const mapStateToProps = state => {
  return {
    counter: state.counter
  }
}

// dispatch: connect中会将store.dispatch传入
const mapDispatchToProps = dispatch => {
  return {
    decrement: () => dispatch(subAction(1)),
    minusFive: () => dispatch(subAction(5))
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(About)

```

### 5. home.js

src > pages > home.js

```js
import React, { PureComponent } from 'react'

import connect from '../utils/connect'

class Home extends PureComponent {

  render() {
    return (
      <div>
        <h1>Home</h1>
        <h2>当前计数: {this.props.counter}</h2>
      </div>
    )
  }
}

const mapStateToProps = state => ({
  counter: state.counter
})

const mapDispatchToProps = dispatch => ({
  
})
export default connect(mapStateToProps, mapDispatchToProps)(Home)

```

## 四. redux-connect

### 1. 使用react-redux中的connect

流程：

* 安装react-redux
* 将组件使用connect函数封装
* 在src > index中用Provider包裹App组件

使用react-redux中提供的connect，完成上述的封装。

安装react-redux

```shell
yarn add react-redux
```

在about组件和home组件都使用react-redux中的connect函数

```js
// import connect from '../utils/connect'
import { connect } from 'react-redux'
```

在src > index.js中使用Provider代替StoreContext

```js
import React from 'react';
import ReactDOM from 'react-dom';

import store from './store'
// import { StoreContext } from './utils/context'

import {Provider} from 'react-redux'

import App from './App';

ReactDOM.render(
  <Provider store={store}>
    <App/>
  </Provider>,
  document.getElementById('root')
);
// ReactDOM.render(
//   <StoreContext.Provider value={store}>
//     <App/>
//   </StoreContext.Provider>,
//   document.getElementById('root')
// );

```

### 2. 进行网络请求

state > action > reducer > 请求数据，diapatch，修改state > 使用state 

Home组件请求数据，About组件使用数据

#### 2.1 constant.js

增加几个常量

```js
export const ADD_NUMBER = "ADD_NUMBER"
export const SUB_NUMBER = "SUB_NUMBER"
export const INCREMENT = "INCREMENT"
export const DECREMENT = "DECREMENT"

export const FETCH_HOME_MULTIDATA = 'FETCH_HOME_MULTIDATA'
export const CHANGE_BANNERS = 'CHANGE_BANNERS'
export const CHANGE_RECOMMENDS = 'CHANGE_RECOMMENDS'

```

#### 2.2 actionCreator.js

增加几个action

```js
// node使用import导入模块，需要在package.json中做相应修改
// 并且导入的文件需要加后缀名
import {
  ADD_NUMBER,
  SUB_NUMBER,
  DECREMENT,
  INCREMENT,
  FETCH_HOME_MULTIDATA,
  CHANGE_BANNERS,
  CHANGE_RECOMMENDS
} from './constant.js'

// 实际的action是对象形式的，使用的时候会使用调用后的返回值
// 写成函数的形式更灵活
export const addAction = num => ({
  type: ADD_NUMBER,
  num
})

export const subAction = num => ({
  type: SUB_NUMBER,
  num
})

export const incAction = () => ({
  type: INCREMENT
})

export const decAction = () => ({
  type: DECREMENT
})

export const changeBannersAction = banners => ({
  type: CHANGE_BANNERS,
  banners
})

export const changeRecommendsAction = recommends => ({
  type: CHANGE_RECOMMENDS,
  recommends
})

export const fetchHomeMultiData = {
  type: FETCH_HOME_MULTIDATA
}
```

#### 2.3 reducer

增加defalutState和case的类型

```js
import {
  ADD_NUMBER,
  SUB_NUMBER,
  INCREMENT,
  DECREMENT,
  CHANGE_RECOMMENDS,
  CHANGE_BANNERS
} from './constant.js'

const defaultState = {
  counter: 0,
  recommends: [],
  banners: []
}

function reducer(state = defaultState, action) {
  switch(action.type) {
    case ADD_NUMBER: 
      return {...state, counter: state.counter + action.num}
    case SUB_NUMBER: 
      return {...state, counter: state.counter - action.num}
    case INCREMENT: 
      return {...state, counter: state.counter + 1}
    case DECREMENT: 
      return {...state, counter: state.counter - 1}
    case CHANGE_RECOMMENDS:
      return {...state, recommends: action.recommends}
    case CHANGE_BANNERS:
      return {...state, banners: action.banners}
    default: 
      return state
  }
}

export default reducer
```

#### 2.4 home.js

home组件在生命周期中请求数据，派发action

```js
import React, { PureComponent } from 'react'

// import connect from '../utils/connect'
import { connect } from 'react-redux'
import axios from 'axios'

import {
  changeBannersAction,
  changeRecommendsAction
} from '../store/actionCreators'

class Home extends PureComponent {
  componentDidMount() {
    axios({
      url: "http://123.207.32.32:8000/home/multidata",
    }).then(res => {
      const data = res.data.data
      this.props.changeBanners(data.banner.list)
      this.props.changeRecommends(data.recommend.list)
    })
  }

  render() {
    return (
      <div>
        <h1>Home</h1>
        <h2>当前计数: {this.props.counter}</h2>
      </div>
    )
  }
}

const mapStateToProps = state => ({
  counter: state.counter
})

const mapDispatchToProps = dispatch => ({
  changeBanners(banners) {
    dispatch(changeBannersAction(banners))
  },
  changeRecommends(recommends) {
    dispatch(changeRecommendsAction(recommends))
  }
})

export default connect(mapStateToProps, mapDispatchToProps)(Home)

```

#### 2.5 about.js

about组件使用state

```js
import React from 'react'

// import store from '../store'
// import  connect from '../utils/connect'
import { connect } from 'react-redux'

import {
  subAction
} from "../store/actionCreators";

function About(props) {
  return (
    <div>
      <h1>About</h1>
      <h2>当前计数: {props.counter}</h2>
      <button onClick={e => props.decrement()} style={{ margin: '5px' }}>-1</button>
      <button onClick={e => props.minusFive()}>-5</button>
      <h2>Banners</h2>
      <ul>
        {
          props.banners.map((item, index) => {
            return <li key={index}>{item.title}</li>
          })
        }
      </ul>
      <h2>Recommend</h2>
      <ul>
        {
          props.recommends.map((item, index) => {
            return <li key={index}>{item.title}</li>
          })
        }
      </ul>
    </div>
  )
}

/* 这两个函数的返回值，都将作为props传入About组件 */
// state: connect中会将store.getState()传入
const mapStateToProps = state => {
  return {
    counter: state.counter,
    banners: state.banners,
    recommends: state.recommends
  }
}

// dispatch: connect中会将store.dispatch传入
const mapDispatchToProps = dispatch => {
  return {
    decrement: () => dispatch(subAction(1)),
    minusFive: () => dispatch(subAction(5))
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(About)
```



## 五. redux-thunk

文档：[reduxjs/redux-thunk: Thunk middleware for Redux (github.com)](https://github.com/reduxjs/redux-thunk)

异步请求的数据也应该由redux管理，因此要在redux部分增加发送网络请求的步骤，因此要使用中间件redux-thunk.

![image-20210425152645295](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20210425152645295.png)

redux-thunk是如何做到让我们可以发送异步的请求呢？

* 我们知道，默认情况下的dispatch(action)，action需要是一个JavaScript的对象；
* redux-thunk可以让dispatch(action函数)，action可以是一个函数；
  * 该函数会被调用，并且会传给这个函数一个dispatch函数和getState函数；
  * dispatch函数用于我们之后再次派发action；
  * getState函数考虑到我们之后的一些操作需要依赖原来的状态，用于让我们可以获取之前的一些状态；

```bash
yarn add redux-thunk
```

### 1. index.js

流程：

* store.index.js做相应配置
* store > actionCreator.js中定义函数式的action，在里面做网络请求等异步操作，然后再派发另一个action，对应reducer里面改变state的action
* 组件中派发函数式的action

store > index.js

引入redux-thunk，并完成相应配置

```js
// 导入applyMiddleware, thunkMiddleware
import { createStore, applyMiddleware } from 'redux'
import thunkMiddleware from 'redux-thunk'

import reducer from './reducer.js'

// 应用中间件，将返回值传入store的第二个参数
const storeEnhancer = applyMiddleware(thunkMiddleware)
const store = createStore(reducer, storeEnhancer)

export default store
```

### 2. actionCreator.js

store > actionCreator.js

可以把原先写在组件生命周期里的操作，写在action里

```js
// redux-thunk中定义的action，可以是一个函数
export const getHomeMultiDataAction = (dispatch, getState) => {
  axios({
    url: 'http://123.207.32.32:8000/home/multidata',
  }).then(res => {
    const data = res.data.data
    dispatch(changeBannersAction(data.banner.list))
    dispatch(changeRecommendsAction(data.recommend.list))
  })
}
```

### 3. home.js

通过mapDispatchToProps传给props，在生命周期中调用封装的函数

```js
import React, { PureComponent } from 'react'

import { connect } from 'react-redux'

import {
  changeBannersAction,
  changeRecommendsAction,
  getHomeMultiDataAction
} from '../store/actionCreators'

class Home extends PureComponent {

  componentDidMount() {
    this.props.getHomeMultiData()
  }

  render() {
    return (
      <div>
        <h1>Home</h1>
        <h2>当前计数: {this.props.counter}</h2>
      </div>
    )
  }
}

const mapStateToProps = state => ({
  counter: state.counter
})

const mapDispatchToProps = dispatch => ({
  changeBanners(banners) {
    dispatch(changeBannersAction(banners))
  },
  changeRecommends(recommends) {
    dispatch(changeRecommendsAction(recommends))
  },
  getHomeMultiData() {
    // 使用redux-thunk，dispatch的action可以是一个函数，会自动调用
    dispatch(getHomeMultiDataAction)
  }
})

export default connect(mapStateToProps, mapDispatchToProps)(Home)

```



## 六. redux devtools

在github上搜索，redux devtools/redux devtools extensions

文档：[zalmoxisus/redux-devtools-extension: Redux DevTools extension. (github.com)](https://github.com/zalmoxisus/redux-devtools-extension#installation)

store > index.js

```js
// 导入applyMiddleware, thunkMiddleware
import { createStore, applyMiddleware, compose } from 'redux'
import thunkMiddleware from 'redux-thunk'

import reducer from './reducer.js'

// composeEnhancers函数，为了react_devtools配置的
// const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
// 增加react devtools中trace的查看，跟踪变化的路径
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({trace: true}) || compose;

// 应用中间件，将返回值传入store的第二个参数
const storeEnhancer = applyMiddleware(thunkMiddleware)
// storeEnhancer使用composeEnhancer包裹
const store = createStore(reducer, composeEnhancers(storeEnhancer))

export default store
```

## 七. redux saga

文档：[redux-saga/redux-saga: An alternative side effect model for Redux apps (github.com)](https://github.com/redux-saga/redux-saga)

中文文档：[自述 · Redux-Saga (redux-saga-in-chinese.js.org)](https://redux-saga-in-chinese.js.org/)

### 1. generator

[gajus.com-blog/index.md at master · gajus/gajus.com-blog (github.com)](https://github.com/gajus/gajus.com-blog/blob/master/posts/the-definitive-guide-to-the-javascript-generators/index.md)

依旧没怎么看懂

```js
function* bar() {
    const result = yield new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('hello world')
        }, 3000)
    })
    console.log(result);
}

// 函数调用但不会执行内部语句，返回迭代器
const iterator = bar() 
// bar()函数内部语句执行，在第一个yield处停下,
// 输出：before, inner, ---, {value: promise, done: false}
// console.log('---', iterator.next())

// 三秒之后输出undefined
iterator.next().value.then(res => iterator.next())
// 三秒之后输出hello world,
// 把res传入，相当于yield res，所以输出语句之前会再中断一次
// iterator.next().value.then(res => iterator.next(res))
```

### 2. index.js

流程：

* store.js中做相应配置
* saga.js写拦截的action及函数，拦截函数中会有发起网络请求等异步操作，然后再派送另一个action，对应reducer中改变state的action
* 组件中派发拦截的action

saga的配置：store > index.js

* 导入createSageMiddleware
* 导入saga
* 调用createSageMiddleware，生成saga中间件
* 应用中间件
* 中间件调用run方法

```js
// 导入applyMiddleware, thunkMiddleware
import { createStore, applyMiddleware, compose } from 'redux'
import thunkMiddleware from 'redux-thunk'
import createSagaMiddleware from 'redux-saga'

import reducer from './reducer.js'
import saga from './saga'

// redux devtools
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({trace: true}) || compose;

const sagaMiddleware = createSagaMiddleware()

// 应用中间件，将返回值传入store的第二个参数
const storeEnhancer = applyMiddleware(thunkMiddleware, sagaMiddleware)
// storeEnhancer使用composeEnhancer包裹
const store = createStore(reducer, composeEnhancers(storeEnhancer))

sagaMiddleware.run(saga)

export default store
```

### 3. saga.js

store > saga.js

```js
import { call, put, takeEvery, takeLatest, all } from 'redux-saga/effects'
import axios from 'axios'

import {
  changeBannersAction,
  changeRecommendsAction
} from './actionCreators'
import {FETCH_HOME_MULTIDATA} from './constant'

function* fetchHomeMultiData(action) {
  const res = yield axios.get("http://123.207.32.32:8000/home/multidata")
  const banners = res.data.data.banner.list
  const recommends = res.data.data.recommends.list
  // put: saga封装的dispatch
  // yield put(changeBannersAction(banners))
  // yield put(changeRecommendsAction(recommends))
  yield all([
    yield put(changeBannersAction(banners)),
    yield put(changeRecommendsAction(recommends))
  ])
}

function* mySaga() {
  // dispatch这个action的时候，就会拦截，执行fetchHomeMultiData这个函数
  yield takeEvery(FETCH_HOME_MULTIDATA, fetchHomeMultiData)
}
export default mySaga
```

### 4. home.js

组件中使用，派发FetchHomeMultiData这个action

```js
import React, { PureComponent } from 'react';

// import {connect} from '../utils/connect';
import { connect } from 'react-redux';


import {
  incAction,
  addAction,
  fetchHomeMultidataAction
} from '../store/actionCreators'

class Home extends PureComponent {
  componentDidMount() {
    this.props.getHomeMultidata();
  }

  render() {
    return (
      <div>
        <h1>Home</h1>
        <h2>当前计数: {this.props.counter}</h2>
        <button onClick={e => this.props.increment()}>+1</button>
        <button onClick={e => this.props.addNumber(5)}>+5</button>
      </div>
    )
  }
}

const mapStateToProps = state => ({
  counter: state.counter
})

const mapDispatchToProps = dispatch => ({
  increment() {
    dispatch(incAction());
  },
  addNumber(num) {
    dispatch(addAction(num));
  },
  getHomeMultidata() {
    dispatch(fetchHomeMultidataAction);
  }
})

export default connect(mapStateToProps, mapDispatchToProps)(Home);

```

## 八. 中间件

需求：在dispatch前后各做一些事情。

```js
// 封装patchLogging的代码，调用patchLogging就会修改dispatch
function patchLogging(store) {
  const next = store.dispatch;
  function dispatchAndLogging(action) {
    console.log("dispatch前---dispatching action:", action);
    next(action);
    console.log("dispatch后---new state:", store.getState());
  }
  // store.dispatch = dispatchAndLogging;

  return dispatchAndLogging;
}

// 封装patchThunk的功能
function patchThunk(store) {
  const next = store.dispatch;

  function dispatchAndThunk(action) {
    if (typeof action === "function") {
      action(store.dispatch, store.getState)
    } else {
      next(action);
    }
  }

  // store.dispatch = dispatchAndThunk;
  return dispatchAndThunk;
}

patchLogging(store);
patchThunk(store);

// store.dispatch(addAction(10));
// store.dispatch(addAction(5));

// function foo(dispatch, getState) {
//   dispatch(subAction(10));
// }

// store.dispatch(foo);

// 5.封装applyMiddleware
function applyMiddlewares(...middlewares) {
  // const newMiddleware = [...middlewares];
  middlewares.forEach(middleware => {
    store.dispatch = middleware(store);
  })
}

applyMiddlewares(patchLogging, patchThunk);
```

## 九. 拆分reducer

将负责不同功能的reducer拆分在不同的文件中，便于管理。

之前的例子中可以将负责counter功能的reducer和负责home页面的reducer分离。

例如：

### 1. 拆分

store > home > constant.js

```js
export const FETCH_HOME_MULTIDATA = "FETCH_HOME_MULTIDATA";
export const CHANGE_BANNERS = "CHANGE_BANNERS";
export const CHANGE_RECOMMEND = "CHANGE_RECOMMEND";
```



store > home > actionCreator.js

```js
import axios from 'axios';

import {
  CHANGE_BANNERS,
  CHANGE_RECOMMEND,
  FETCH_HOME_MULTIDATA
} from './constants.js';


// 轮播图和推荐的action
export const changeBannersAction = (banners) => ({
  type: CHANGE_BANNERS,
  banners
});

export const changeRecommendAction = (recommends) => ({
  type: CHANGE_RECOMMEND,
  recommends
});


// redux-thunk中定义的action函数
export const getHomeMultidataAction = (dispatch, getState) => {
  axios({
    url: "http://123.207.32.32:8000/home/multidata",
  }).then(res => {
    const data = res.data.data;
    dispatch(changeBannersAction(data.banner.list));
    dispatch(changeRecommendAction(data.recommend.list));
  })
}


// redux-saga拦截的action
export const fetchHomeMultidataAction = {
  type: FETCH_HOME_MULTIDATA
}


```

store > home > reducer.js

```js
import {
  CHANGE_BANNERS,
  CHANGE_RECOMMEND
} from './constants.js';

// 拆分homeReducer
const initialHomeState = {
  banners: [],
  recommends: []
}
function homeReducer(state = initialHomeState, action) {
  switch (action.type) {
    case CHANGE_BANNERS:
      return { ...state, banners: action.banners };
    case CHANGE_RECOMMEND:
      return { ...state, recommends: action.recommends };
    default:
      return state;
  }
}

export default homeReducer;
```

store > home > reducer.js

```js
import reducer from './reducer';

export {
  reducer
}

```

### 2. 合并

store > reducer.js

```js
import { reducer as counterReducer } from './counter';
import { reducer as homeReducer } from './home';

import { combineReducers } from 'redux';

// function reducer(state = {}, action) {
//   return {
//     counterInfo: counterReducer(state.counterInfo, action),
//     homeInfo: homeReducer(state.homeInfo, action)
//   }
// }

// counter就放在了state.counterInfo中了
//reducer应该是一个什么类型? function
const reducer = combineReducers({
  counterInfo: counterReducer,
  homeInfo: homeReducer
});

export default reducer;

```

事实上，它也是讲我们传入的reducers合并到一个对象中，最终返回一个combination的函数（相当于我们之前的reducer函数了）；

* 在执行combination函数的过程中，它会通过判断前后返回的数据是否相同来决定返回之前的state还是新的state；
* 新的state会触发订阅者发生对应的刷新，而旧的state可以有效的组织订阅者发生刷新；



### 3. 使用

pages > home.js

```js
const mapStateToProps = state => ({
  counter: state.counterInfo.counter
})

const mapDispatchToProps = dispatch => ({
  increment() {
    dispatch(incAction());
  },
  addNumber(num) {
    dispatch(addAction(num));
  },
  getHomeMultidata() {
    dispatch(fetchHomeMultidataAction);
  }
})
```

