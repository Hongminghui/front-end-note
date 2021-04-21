

图标需要另外添加库

样式也得在index.js里引入

# Css编写方式

## 1. 内联样式

* 内联样式是官方推荐的一种css样式的写法： 
  * style 接受一个采用小驼峰命名属性的 JavaScript 对象，，而不是 CSS 字符串； 
  * 并且可以引用state中的状态来设置相关的样式； 
* 内联样式的优点: 
  * 1.内联样式, 样式之间不会有冲突 
  * 2.可以动态获取当前state中的状态 
* 内联样式的缺点： 
  * 1.写法上都需要使用驼峰标识 
  * 2.某些样式没有提示 
  * 3.大量的样式, 代码混乱 
  * 4.某些样式无法编写(比如伪类/伪元素) 
* 所以官方依然是希望内联合适和普通的css来结合编写；

```js
render() {
    const pStyle = {
      color: this.state.color,
      textDecoration: "underline"
    }

    return (
      <div>
        <h2 style={{fontSize: "50px", color: "red"}}>我是标题</h2>
        <p style={pStyle}>我是一段文字描述</p>
      </div>
    )
  }
```



## 2. 普通的Css

普通的css我们通常会编写到一个单独的文件，之后再进行引入。 

* 这样的编写方式和普通的网页开发中编写方式是一致的： 
  * 如果我们按照普通的网页标准去编写，那么也不会有太大的问题； 
  * 但是组件化开发中我们总是希望组件是一个独立的模块，即便是样式也只是在自己内部生效，不会相互影响； 
  * 但是普通的css都属于全局的css，样式之间会相互影响； 
  * 这种编写方式最大的问题是样式之间会相互层叠掉；

## 3. Css modules

* .css/.less/.scss 等样式文件都修改成 .module.css/.module.less/.module.scss 等

* 以模块的方式引入

  ```js
  import appStyle from './style.module.css'
  ```

* 所有的类名都以：appStyle.selector的方式编写

App.js

```js
import appStyle from './style.module.css'

function Cpn() {
  return (
    <div className={appStyle.cpn}>
      <p>子组件p元素</p>
    </div>
  )
}

function App() {
  return (
    <div className={appStyle.app}>
      <h2 className={appStyle.title}>类选择器</h2>
      <h2 id={appStyle.content}>id选择器</h2>
      <p>元素选择器</p>
      <Cpn></Cpn>
    </div>
    
  );
}

export default App;

```

style.css

```css
.title {
  color: red;
}

#content {
  color: blue;
}

.app > p {
  color: burlywood;
  font-size: 30px;
}

.cpn > p {
  color: chartreuse;
  font-size: 30px;
}
```

最终显示：

![image-20210419163121182](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20210419163121182.png)

优点：可以解决样式重叠的问题

缺点：

* 引用的类名，不能使用连接符(.home-title)，在JavaScript中是不识别的； 
* 所有的className都必须使用{style.className} 的形式来编写； 
* 不方便动态来修改某些样式，依然需要使用内联样式的方式；

## 4. Css in Js

非官方实现，使用第三方库实现

* styled-components 
* emotion 
* glamorous

以styled-components为例：

```
yarn add styled-components
```

### 4.1 基本用法

index.js

```js
import React, { Component } from 'react'

import {
  HYButton
} from './style'

export default class App extends Component {
  render() {
    return (
      <div>
        <HYButton>按钮</HYButton>
      </div>
    )
  }
}

```

style.js

创建一个js文件，用来写样式

```js
import styled from 'styled-components'

// 相当于创建了一个自带样式的button
export const HYButton = styled.button`
  padding: 10px 20px;
  background-color: red;
`
```

### 4.2 继承

```js
// 继承
export const HYPrimaryButton = styled(HYButton)`
  background-color: blue;
`
```

### 4.3 嵌套

Home > index.js

```js
import React, { PureComponent } from 'react'

import {HomeWrapper} from './style'

export default class Home extends PureComponent {
  render() {
    return (
      <HomeWrapper>
        <h2>Home标题</h2>
        <div>Home内容</div>
        <span>随便写的</span>
        <span className="active">轮播图</span>
      </HomeWrapper>
    )
  }
}

```

Home > style.js

实现嵌套和伪类、伪元素

```css
import styled from 'styled-components'

export const HomeWrapper = styled.div`
  font-size: 24px;
  color: red;

  div {
    background-color: blue;
    color: white;
  }

  .active {
    color: black;
  }

  &:hover {
    background-color: blue;
  }

  &:after {
    content: '';
    display: inline-block;
    width: 100px;
    height: 100px;
    background-color: yellow;
  }

`
```

### 4.4 使用变量

```js
import React, { Component } from 'react'

import {
  HYInput
} from './style'

export default class Profile extends Component {

  state = {
    color: 'white'
  }

  render() {
    return (
      <div>
        <input type='password'></input>
        <HYInput color={this.state.color} type="password"></HYInput>
      </div>
    )
  }
}
```

```js
import styled from 'styled-components'

/* 
  attrs: 接收一个对象作为参数，给标签添加属性，或者定义变量
  props: 标签传入和attrs定义的值，
         具有穿透性，给HYInput添加的属性，最终会加到input上，如type属性
         把变化的值通过props传递，就可以动态的给标签加样式了
  ${}: 返回值会作为样式的值
*/
export const HYInput = styled.input.attrs({
  placeholder: 'hmh12345',
  bgColor: 'red'
})`
  color: ${props => props.color ? 'blue' : 'black'};
  background-color: ${props => props.bgColor}
`
```

### 4.5 共享主题

在app > index.js中导入ThemeProvider，然后将所有标签都包裹在ThemeProvider标签中，ThemeProvider标签中theme属性的值就可以在包裹的组件中共享了。

可以认为是传入公共变量

```js
import React, { Component } from 'react'

import {ThemeProvider} from 'styled-components'

import Home from "../home/index.js";
import Profile from "../profile/index.js"

import {
  HYButton,
  HYPrimaryButton
} from './style'

export default class App extends Component {
  render() {
    return (
      <ThemeProvider theme={{themeColor: 'red', fontSize: '30px'}}>
        <HYButton>按钮一</HYButton>
        <HYPrimaryButton>按钮二</HYPrimaryButton>
        <Home/>
        <Profile/>
      </ThemeProvider>
    )
  }
}

```

home > style.js

使用传入的主题

```js
export const TitleWrapper = styled.h2`
  color: ${props => props.theme.themeColor}
`
```

home > index.js

```js
import React, { PureComponent } from 'react'

import {HomeWrapper, TitleWrapper} from './style'

export default class Home extends PureComponent {
  render() {
    return (
      <HomeWrapper>
        <TitleWrapper>TitleWrapper</TitleWrapper>
        <h2>Home标题</h2>
        <div>Home内容</div>
        <span>随便写的</span>
        <span className="active">轮播图</span>
      </HomeWrapper>
    )
  }
}

```

## 5. react中添加class

```shell
yarn add classnames
```



```js
export default class App extends PureComponent {
  constructor(props) {
    super(props);

    this.state = {
      isActive: true
    }
  }

  render() {
    const {isActive} = this.state;
    const isBar = false;
    const errClass = "error";
    const warnClass = 10;

    return (
      <div>
        {/* 原生React中添加class方法 */}
        <h2 className={"foo bar active title"}>我是标题1</h2>
        <h2 className={"title" + (isActive ? " active": "")}>我是标题2</h2>
        <h2 className={["title", (isActive ? "active": "")].join(" ")}>我是标题3</h2>

        {/* classnames库添加class */}
        <h2 className="foo bar active title">我是标题4</h2>
        <h2 className={classNames("foo", "bar", "active", "title")}>我是标题5</h2>
        <h2 className={classNames({"active": isActive, "bar": isBar}, "title")}>我是标题6</h2>
        <h2 className={classNames("foo", errClass, warnClass, {"active": isActive})}>我是标题7</h2>
        <h2 className={classNames(["active", "title"])}>我是标题8</h2>
        <h2 className={classNames(["active", "title", {"bar": isBar}])}>我是标题9</h2>
      </div>
    )
  }
}

```

# AntDesign

## 自定义主题

在create-react-app中高级配置一节

