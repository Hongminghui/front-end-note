# React基础

```
this指向
原生事件 this指向节点，window调用 div.onclick 由
React事件 render由实例调用，所以事件绑定this指向组件，事件绑定的回调函数由全局调用，严格模式绑定在undefined
```

```html
<!-- this指向节点，因为onclick是div节点的属性，相当于是div调用的 -->
<div id="test" onclick="test(this)">Hello World</div>
```

```js
let div = document.getElementById('test')
function test(a) {
    // 这里的this指向window，函数是全局调用的
    console.log(this);
}
```

```jsx
render() {
    const {isHot, wind} = this.state
    // render函数由实例调用，所以这里的this指向实例
    return <h2 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}, {wind}			</h2>
}

// 回调函数由全局调用，严格模式下this绑定在undefined
changeWeather(){
    const isHot = this.state.isHot
    // state的修改只能通过setState，是合并式修改，不是覆盖式修改
    this.setState({isHot: !isHot})
}
```

![image-20210409104548229](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20210409104548229.png)

## 二. 脚手架

