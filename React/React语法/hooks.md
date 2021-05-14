# Hooks

## 1. hooks简介

### 1.1 为什么需要hooks

* Hook 是 React 16.8 的新增特性，它可以让我们在不编写class的情况下使用state以及其他的React特性（比如生命周期）。 
* 我们先来思考一下class组件相对于函数式组件有什么优势？比较常见的是下面的优势： 
* class组件可以定义自己的state，用来保存组件自己内部的状态； 
  * 函数式组件不可以，因为函数每次调用都会产生新的临时变量； 
* class组件有自己的生命周期，我们可以在对应的生命周期中完成自己的逻辑； 
  * 比如在componentDidMount中发送网络请求，并且该生命周期函数只会执行一次； 
  * 函数式组件在学习hooks之前，如果在函数中发送网络请求，意味着每次重新渲染都会重新发送一次网络请求； 
* class组件可以在状态改变时只会重新执行render函数以及我们希望重新调用的生命周期函数componentDidUpdate等； 
  * 函数式组件在重新渲染时，整个函数都会被执行，似乎没有什么地方可以只让它们调用一次； 
* 所以，在Hook出现之前，对于上面这些情况我们通常都会编写class组件。

### 1.2 class组件的问题

* 复杂组件变得难以理解： 
  * 我们在最初编写一个class组件时，往往逻辑比较简单，并不会非常复杂。但是随着业务的增多，我们的class组件会变得越来越复杂； 
  * 比如componentDidMount中，可能就会包含大量的逻辑代码：包括网络请求、一些事件的监听（还需要在 componentWillUnmount中移除）； 
  * 而对于这样的class实际上非常难以拆分：因为它们的逻辑往往混在一起，强行拆分反而会造成过度设计，增加代码的复杂度； 
* 难以理解的class： 
  * 很多人发现学习ES6的class是学习React的一个障碍。 
  * 比如在class中，我们必须搞清楚this的指向到底是谁，所以需要花很多的精力去学习this； p 虽然我认为前端开发人员必须掌握this，但是依然处理起来非常麻烦； 
* 组件复用状态很难： 
  * 在前面为了一些状态的复用我们需要通过高阶组件或render props； 
  * 像我们之前学习的redux中connect或者react-router中的withRouter，这些高阶组件设计的目的就是为了状态的复用； 
  * 或者类似于Provider、Consumer来共享一些状态，但是多次使用Consumer时，我们的代码就会存在很多嵌套； 
  * 这些代码让我们不管是编写和设计上来说，都变得非常困难；

### 1.3 hooks的出现

Hook的出现，可以解决上面提到的这些问题； 

* 简单总结一下hooks： 
  * 它可以让我们在不编写class的情况下使用state以及其他的React特性； 
  * 但是我们可以由此延伸出非常多的用法，来让我们前面所提到的问题得到解决； 
* Hook的使用场景： 
  * Hook的出现基本可以代替我们之前所有使用class组件的地方（除了一些非常不常用的场景）； 
  * 但是如果是一个旧的项目，你并不需要直接将所有的代码重构为Hooks，因为它完全向下兼容，你可以渐进式的来使用它； 
  * Hook只能在函数组件中使用，不能在类组件，或者函数组件之外的地方使用；

## 2. useState

### 2.1 useState初体验

useState被称为一个hook，表示钩入（hook into) state以及生命周期

useState:

* 参数是state的默认值，如果不设置是undefined
* 返回值是一个数组
  * 数组第一项是state，保存的状态
  * 数组第二项是修改这个state的函数，例如setCount()，调用setCount时传入的参数即为state的新值

* 功能：函数式组件添加了持久保存的、响应式的state



```jsx
import React, { useState } from 'react'

export default function CounterHook() {
  // 数组解构，相当于count = arr[0]，setCount = arr[1]
  const [count, setCount] = useState(0)
  return (
    <div>
      <h2>当前计数：{count}</h2>
      <button onClick={e => setCount(count + 1)}>点击+1</button>
      <button onClick={e => setCount(count - 1)}>点击-1</button>
    </div>
  )
}
```

### 2.2 使用多个状态

```jsx
import React, { useState } from 'react';

export default function MultiHookState() {

  const [count, setCount] = useState(0);
  const [age, setAge] = useState(18);
  const [friends, setFriends] = useState(["kobe", "lilei"]);

  return (
    <div>
      <h2>当前计数: {count}</h2>
      <h2>我的年龄: {age}</h2>
      <ul>
        {
          friends.map((item, index) => {
            return <li key={item}>{item}</li>
          })
        }
      </ul>
    </div>
  )
}

```

### 2.3 复杂状态的修改

```jsx
import React, { useState } from 'react'

export default function ComplexStateHook() {

  const [students, setStudents] = useState([
    {id: '01', name: 'test1', age: 18},
    {id: '02', name: 'test2', age: 18},
    {id: '03', name: 'test3', age: 18},
  ])

  // 错误的做法，students一直指向同一个地址，不会重新渲染
  // function addStudents() {
  //   console.log('addStudents');
  //   students.push({id: Math.random()*10, name: 'test4', age: 18})
  //   setStudents(students)
  // }

  function addStudents() {
    const newStudents = [...students]
    newStudents.push({id: Math.random()*10, name: 'test4', age: 18})
    setStudents(newStudents)
  }
  return (
    <div>
      <h2>学生列表</h2>
      <ul>
        {
          students.map((item, index) => (
            <li key={item.id}>
              <h3>姓名：{item.name}</h3>
              <h3>年龄：{item.age}</h3>
            </li>
          ))    
        }
      </ul>
      <button onClick={e => addStudents()}>增加一个学生</button>
    </div>
  )
}

```

### 2.4 setCount传参

```jsx
import React, { useState } from 'react'

export default function CounterHook() {
  // 数组解构，相当于count = arr[0]，setCount = arr[1]
  const [count, setCount] = useState(0)
  
  // setCount会合并，执行一次只会+1
  function add1() {
    setCount(count + 1)
    setCount(count + 1)
    setCount(count + 1)
  }

  // setCount不会合并，执行一次会+3
  function add2() {
    setCount(count => count + 1)
    setCount(count => count + 1)
    setCount(count => count + 1)
  }
  return (
    <div>
      <h2>当前计数：{count}</h2>
      <button onClick={e => add1()}>add1</button>
      <button onClick={e => add2()}>add2</button>
    </div>
  )
}

```



## 3. useEffect

功能：为函数式组件增加一些生命周期

useEffect

* 参数一是一个函数fn1，第一次渲染和组件更新后，fn1return语句之前的代码
  * fn1返回一个函数fn2，fn2在组件更新前或者销毁前执行
* 参数二是一个数组，表示执行的依赖，在下例中，只有count发生改变时，fn1才会执行
* fn1返回值，参数二都是可选参数，如果传空数组的话，表示不依赖任何数据，只会在第一次渲染的时候执行一次

### 3.1 基本使用

```jsx
import React, {useState, useEffect} from 'react'

export default function TitleEffect() {
  const [count, setCount] = useState(0)

  useEffect(() => {
    // 第一次渲染和组件更新后，执行这部分代码
    console.log('订阅事件');
    // 组件更新前或者销毁前，执行这部分代码
    return () => {
      console.log('取消订阅')
    }
  },[count])
  return (
    <div>
      <h2>当前计数：{count}</h2>
      <button onClick={e => setCount(count + 1)}>点击+1</button>
    </div>
  )
}
```

App.js

```jsx
<div>

    {/* 03_useEffect的使用 */}

    {/* jsx语法要包裹在一个标签里 */}
    {isShow && <TitleEffect/>}
    <button onClick={e => setShow(!isShow)}>按钮</button>
</div>
```

流程：

* 第一次渲染时，输出订阅事件
* 点击+1按钮时，依次输出取消订阅，订阅事件
* 点击隐藏按钮时，输出取消订阅
* 再次点击隐藏按钮时，输出订阅事件

### 3.2 多个useEffect的使用

执行顺序和声明顺序相同，异步代码执行和js中执行规则相同，同步 > 微任务 > 宏任务

```jsx
import React, { useState, useEffect } from 'react'

export default function MultiEffectHook() {
  const [count ,setCount] = useState(0)
  const [isLogin, setLogin] = useState(true)

  useEffect(() => {
    console.log('count改变了', count);
    setTimeout(() => console.log('异步操作'), 0)
  }, [count])
  
  // 传空数组，表示不依赖任何数据，只会在第一次渲染时执行一次
  useEffect(() => {
    console.log('网络请求');
  }, [])
  return (
    <div>
      <h2>当前计数：{count}</h2>    
      <button onClick={e => setCount(count + 1)}>点击+1</button>
      <h2>{isLogin ? '用户名：hmh' : '未登录'}</h2>
      <button onClick={e => setLogin(!isLogin)}>登录/注销</button>
    </div>
  )
}
```

## 4. useContext

流程：

* createContext()创建context
* 包裹后代组件通过value属性传值，Provieder，是大写的
* 后代组件通过useContext(UserContext)可以使用传递的值了

直接通过useContext获取某一个Provider的值，就不会嵌套那么多层了

App.js

```jsx
import React, { useState, createContext } from 'react'

import ContextHook from './04_useContext的使用/01_useContext的使用'

export const UserContext = createContext()
export const ThemeContext = createContext()

function App() {
  return (
    <div>
      {/* 04_useContext的使用 */}
      <UserContext.Provider value={{name: 'hmh', age: 22}}>
        <ThemeContext.Provider value = {{fontSize: '20px', color: 'blue'}}>
          <ContextHook/>
        </ThemeContext.Provider>
      </UserContext.Provider>
    </div>
  )
}

export default App;

```

01_useContext的使用.js

```jsx
import React, { useContext } from 'react'
import { UserContext, ThemeContext } from '../App'

export default function ContextHook() {
  const user = useContext(UserContext)
  const theme = useContext(ThemeContext)
  console.log('user', user);
  console.log('theme', theme);
  return (
    <div>
      <h2 style={{backgroundColor: theme.color}}>useContext</h2>
    </div>
  )
}
```

## 5. useReducer

useReducer仅仅是useState的一种替代方案： 

* 在某些场景下，如果state的处理逻辑比较复杂，我们可以通过useReducer来对其进行拆分；
* 或者这次修改的state需要依赖之前的state时，也可以使用；
* 数据是不会共享的，只是共享了reducer函数，useReducer只是useState的一种替代品，并不能替代Redux。

reducer.js

```jsx
export default function reducer(state, action) {
  switch(action.type) {
    case 'increment': 
      return {...state, counter: state.counter + 1}
    case 'decrement': 
      return {...state, counter: state.counter - 1}
    default:
      return state
  }
}
```

home.js

```jsx
import React, { useReducer } from 'react'
import reducer from './reducer'

export default function Home() {
  // state的初始值就是传入的第二个参数
  // action就是dispatch的参数
  const [state, dispatch] = useReducer(reducer, {counter: 0})
  return (
    <div>
      <h2>Home当前计数：{state.counter}</h2>
      <button onClick={e => dispatch({type: 'increment'})}>点击+1</button>
      <button onClick={e => dispatch({type: 'decrement'})}>点击-1</button>
    </div>
  )
}
```

profile.js

```jsx
import React, { useReducer } from 'react'
import reducer from './reducer'

export default function Profile() {
  // state的初始值就是传入的第二个参数
  const [state, dispatch] = useReducer(reducer, {counter: 0})
  return (
    <div>
      <h2>Profile当前计数：{state.counter}</h2>
      <button onClick={e => dispatch({type: 'increment'})}>点击+1</button>
      <button onClick={e => dispatch({type: 'decrement'})}>点击-1</button>
    </div>
  )
}

```

数据是不会共享的，只是共享了reducer这个函数，home和profile中的counter是相互独立的

## 6. useCallback

memo是函数式组件用来实现PureComponent功能的，即父组件因为改变某个数据重新渲染时，如果子组件不依赖这个数据就不必重新渲染。

如果父组件是函数式组件，那么父组件重新渲染时，内部定义的函数也会重新定义，如果这个函数会作为props传给子组件的话，memo会认为这两次传递的函数是不一样的，memo在这种情况下就没有起到性能优化的作用，因此需要useCallback。

useCallback：为性能优化而使用，传入的函数参数是有记忆的(memorized)的

* 参数一：函数，实现本来的功能
* 参数二：依赖的数组，如果父组件更新的数据，子组件并不依赖的话，在父组件更新过程中，这个函数会重复定义，但是依然是相同返回相同的值，子组件也就不会重新渲染
* 使用场景：子组件需要父组件传递的函数

下例中表现为isLogin的更新导致CallbackHook重新渲染，子组件btn1会重新渲染，但依赖函数用useCallback包裹的btn2不会重新渲染

```jsx
import React, {useState, useCallback, memo} from 'react'

const HYButton = memo(props => {
  console.log(`${props.title}重新渲染`);
  return <button onClick={props.increment}>{props.title}+1</button>
})

export default function CallbackHook() {
  const [count, setCount] = useState(0)
  const [isLogin, setLogin] = useState(true)

  // increment1会重新定义
  const increment1 = () => {
    console.log('increment1执行');
    setCount(count + 1)
  }
  const increment2 = useCallback(() => {
    console.log('increment2执行');
    setCount(count + 1)
  }, [count])
  return (
    <div>
      <h2>当前计数: {count}</h2>
      <HYButton title='btn1' increment={increment1}/>
      <HYButton title='btn2' increment={increment2}/>
      <h2>{isLogin ? '用户名：hmh' : '未登录'}</h2>
      <button onClick={e => setLogin(!isLogin)}>登录/注销</button>
    </div>
  )
}
```

## 7. useMemo

同样是用来性能优化的，

* 参数一：函数，返回值是有记忆的，当依赖的数据不发生变化，返回值就不会变
* 参数二：依赖的数组
* 返回值即为传入函数参数的返回值

### 7.1 使用复杂计算

复杂计算只依赖一部分数据，没必要每次组件重新渲染都计算一次

组件因isLogin变化而重新渲染时，total的值并不依赖isLogin，所以calculateSum函数也不会被调用

```jsx
import React, {useState, useMemo} from 'react'

function calculateSum(count) {
  console.log('CalculateSum调用');
  let total = 0
  for(let i = 0; i < count; i++) {
    total += i
  }
  return total
}

export default function CalMemoHook() {
  const [count, setCount] = useState(10)
  const [isLogin, setLogin] = useState(true)
  const total = useMemo(() => calculateSum(count), [count])
  return (
    <div>
      <h2>当前计数：{count}</h2>
      <h2>总和：{total}</h2>
      <button onClick={e => setCount(count + 1)}>+1</button>
      <h2>{isLogin ? 'hmh' : '未登录'}</h2>
      <button onClick={e => setLogin(!isLogin)}>登录/注销</button>
    </div>
  )
}
```

### 7.2 useCallback

useMemo实现useCallback的功能

```jsx
const increment2 = useCallback(() => {
    console.log("执行increment2函数");
    setCount(count + 1);
}, [count]);

// 用useMemo实现useCallback
const increment3 = useMemo(() => {
    return () => {
        console.log("执行increment2函数");
        setCount(count + 1);
    }
}, [count]);
```

### 7.3 将值传给子组件

如果传递的值没有变化，那么子组件就不应该重新渲染

函数式组件重新渲染，info会重新定义，memo会认为两次传递的值不一样。使用useMemo后这个值就是有记忆的，当依赖没有变化时，这个值就不会变化，子组件也就不会更新

```jsx
import React, {useState, useMemo, memo} from 'react'

const Text = memo(function (props) {
  console.log('Text重新渲染');
  return <h2>用户名：{props.info.name}</h2>
})

export default function PropsMemoHook() {
  const [count, setCount] = useState(0)
  // Text会重新渲染
  // const info = {name: 'hmh', age: 22}
  // Text不会重新渲染
  const info = useMemo(() => ({name: 'hmh', age: 22}), [])
  return (
    <div>
      <h2>当前计数：{count}</h2>
      <button onClick={e => setCount(count + 1)}>+1</button>
      <Text info={info}/>
    </div>
  )
}
```

## 8. useRef

### 8.1 引用dom或组件

使用useRef可以引用dom，或者类组件，函数式组件会报错

```jsx
import React, { useRef, PureComponent } from 'react'

class Cpn extends PureComponent {
  render() {
    return (
      <div>
        <h2>子组件Cpn</h2>
      </div>
    )
  }
}

export default function DomRefHook() {
  const titleRef = useRef()
  const cpnRef = useRef()

  return (
    <div>
      <h2 ref={titleRef}>标题</h2>
      <Cpn ref={cpnRef} />
      <button onClick={e => console.log(cpnRef, titleRef)}>按钮</button>
    </div>
  )
}

```

### 8.2 引用其他数据

useRef()传入一个值，countRef在组件整个生命周期中都保持不变

```jsx
import React, {useState, useRef} from 'react'

export default function DataRefHook() {
  const [count, setCount] = useState(0)
  // countRef在一直不变
  const countRef = useRef(count)
  const normalCount = count
  return (
    <div>
      <h2>当前计数：{count}</h2>
      <h2>countRef: {countRef.current}</h2>
      <h2>普通的变量：{normalCount}</h2>
      <button onClick={e => setCount(count + 1)}>+1</button>
    </div>
  )
}

```

### 8.3 展示上一次的值

useRef和useEffect结合使用，可以展示上一次的值

```jsx
import React, {useState, useRef, useEffect} from 'react'

export default function DataRefHook() {
  const [count, setCount] = useState(0)
  // countRef在一直不变
  const countRef = useRef(count)

  useEffect(() => {
    // 这部分代码在jsx更新后执行，会滞后一次
    countRef.current = count
  }, [count])
  return (
    <div>
      <h2>当前计数：{count}</h2>
      <h2>上一次计数：{countRef.current}</h2>
      <button onClick={e => setCount(count + 1)}>+1</button>
    </div>
  )
}
```

## 9. useImperetiveHandle

### 9.1 forwardRef使用

缺陷是将子组件的dom元素都暴露给父组件了，父组件可以对子组件的dom做任意操作，可能造成不可控的情况，在本例中只暴露focus即可。

```jsx
import React, { forwardRef, useRef } from 'react'

const HYInput = forwardRef((props, ref) => {
  return <input type='text' ref={ref}></input>
})
export default function ForwardRefDemo() {
  const HYInputRef = useRef()
  return (
    <div>
      <HYInput ref={HYInputRef}/>
      <button onClick={e => HYInputRef.current.focus()}>点击聚焦</button>
    </div>
  )
}

```

### 9.2 useImperativeHandle

因此要使用useImperativeHandle来控制父组件的权限

```jsx
import React, { useRef, forwardRef, useImperativeHandle } from 'react';

const HYInput = forwardRef((props, ref) => {
  // 父子组件的inputRef不一样
  const inputRef = useRef();

  // 现在父组件的inputRef.current，就只能访问此处箭头函数返回的对象了
  // inputRef.current变化时，重新执行此处的代码
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    }
  }), [inputRef.current])

  return <input ref={inputRef} type="text"/>
})

export default function UseImperativeHandleHookDemo() {
  const inputRef = useRef();

  return (
    <div>
      <HYInput ref={inputRef}/>
      <button onClick={e => inputRef.current.focus()}>聚焦</button>
    </div>
  )
}
```

## 10. useLayoutEffect

```jsx
import React, { useState, useEffect } from 'react'

export default function EffectCounterDemo() {
  const [count, setCount] = useState(10);

  // jsx重新渲染之后执行这部分代码
  useEffect(() => {
    if (count === 0) {
      setCount(Math.random() + 200)
    }
  }, [count]);

  // 数字变化是：10 > 0 > 随机数，数字变化有闪烁
  return (
    <div>
      <h2>数字: {count}</h2>
      <button onClick={e => setCount(0)}>修改数字</button>
    </div>
  )
}
```



```jsx
import React, { useState, useEffect, useLayoutEffect } from 'react'

export default function LayoutEffectCounterDemo() {
  const [count, setCount] = useState(10);

  // jsx重新渲染到屏幕之前，执行这部分代码
  useLayoutEffect(() => {
    if (count === 0) {
      setCount(Math.random() + 200)
    }
  }, [count]);


  return (
    <div>
      <h2>数字: {count}</h2>
      <button onClick={e => setCount(0)}>修改数字</button>
    </div>
  )
}

```

## 11. 自定义hook

### 11.1 共享context

App.jsx

```jsx
<UserContext.Provider value={{name: 'hmh', age: 22}}>
    <TokenContext.Provider value='faasdflasdfasdf'>
        <ContextShareHook/>
    </TokenContext.Provider>
</UserContext.Provider>
```

hook > userContext.js

把获取user相关的context操作都放在这个函数中，返回值是所有的user相关context，再将函数导出，需要使用user相关context的组件导入并调用这个函数，就能够获取所有的user相关context.

```jsx
import {useContext} from 'react'

import {UserContext, TokenContext} from '../App'  

function useUserContext() {
  const user = useContext(UserContext)
  const token = useContext(TokenContext)
  return [user, token]
}

export default useUserContext
```

example.js

使用自定义hook

```jsx
import React from 'react'
import useUserContext from '../hook/userContext'

export default function ContextShareHook() {
  const [user, token] = useUserContext()
  console.log();
  return (
    <div>
      <h2>用户名：{user.name}</h2>
      <h2>token： {token}</h2>
    </div>
  )
}
```

### 11.2 获取滚动位置

hook > scrollHook.js

```jsx
import { useState, useEffect } from 'react';

function useScrollPosition() {
  const [scrollPosition, setScrollPosition] = useState(0);

  useEffect(() => {
    const handleScroll = () => {
      setScrollPosition(window.scrollY);
    }
    document.addEventListener("scroll", handleScroll);

    // 销毁时，移除监听
    return () => {
      document.removeEventListener("scroll", handleScroll)
    }
  }, []);

  return scrollPosition;
}

export default useScrollPosition;
```

example.js

```jsx
import React, { useEffect, useState } from 'react'
import useScrollPosition from '../hooks/scroll-position-hook'

export default function CustomScrollPositionHook() {
  const position = useScrollPosition();

  return (
    <div style={{padding: "1000px 0"}}>
      <h2 style={{position: "fixed", left: 0, top: 0}}>CustomScrollPositionHook: {position}</h2>
    </div>
  )
}

```

### 11.3 localStorage存储

localStorageHook.js

```jsx
import {useState, useEffect} from 'react';

function useLocalStorage(key) {
  const [name, setName] = useState(() => {
    const name = JSON.parse(window.localStorage.getItem(key));
    return name;
  });

  useEffect(() => {
    window.localStorage.setItem(key, JSON.stringify(name));
  }, [name]);

  return [name, setName];
}

export default useLocalStorage;

```

example.js

```jsx
import React, { useState, useEffect } from 'react';

import useLocalStorage from '../hooks/local-store-hook';

export default function CustomDataStoreHook() {
  const [name, setName] = useLocalStorage("name");

  return (
    <div>
      <h2>CustomDataStoreHook: {name}</h2>
      <button onClick={e => setName("kobe")}>设置name</button>
    </div>
  )
}

```

