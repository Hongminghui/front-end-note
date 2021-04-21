## JavaScript

### 1. 事件循环

[JavaScript 运行机制详解：再谈Event Loop - 阮一峰的网络日志 (ruanyifeng.com)](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)

### 2. 宏任务 微任务

[微任务、宏任务与Event-Loop - 贾顺名 - 博客园 (cnblogs.com)](https://www.cnblogs.com/jiasm/p/9482443.html)

io 定时器是宏任务，promise.then catch是微任务，

执行完所有的微任务，再去执行下一个宏任务

```js
setTimeout(_ => console.log(4))

// promise实例化的执行器函数是同步执行的
new Promise(resolve => {
  resolve()
  console.log(1)
}).then(_ => {
  console.log(3)
})

console.log(2)

// 1 2 3 4
```

### 3. cookie session token

[常用的本地存储——cookie篇 - SegmentFault 思否](https://segmentfault.com/a/1190000004743454)

cookie 是存在客户端，而且它本身存储的尺寸大小也有限，最关键是用户可以是可见的，并可以随意的修改，很不安全。请求时要带上cookie数据，增加网络请求负担。

session session_id存储在客户端，用户数据存储在服务端，可能造成服务器负担，也可能有被截获的风险

token: 服务端加密后，返回一串字符串，解密

### 4. 获取页面中的所有标签及数量

元素节点 nodeType 1

文档节点 nodeType 9

文本节点 nodeType 3

document就是整个网页

```js
/* 获取所有标签及其数量 */
const map = {}

function fds(node) {
  if (node.nodeType === 1) {
    let tagName = node.nodeName
    map[tagName] = map[tagName] ? map[tagName] + 1 : 1
  }
  const children = node.childNodes
  for (let i = 0; i < children.length; i++) {
    fds(children[i])
  }
}

fds(document)
console.log(map)

/* 输出所有的节点和节点的总数量 */
let count = 0
function countNodes(node) {
  if(node.nodeType === 1) {
    count++
    console.log(node.nodeName)
  }
  let children = node.childNodes
  for(let i = 0; i < children.length; i++) {
    countNodes(children[i])
  }
  return count
}
console.log(countNodes(document))
```

### 4. sessionStorage localStorage

sessionStorage 对象只存储会话数据，这意味着数据只会存储到浏览器关闭。

localStorage作为在客户端持久存储 数据的机制。要访问同一个 localStorage 对象，页面必须来自同一个域（子域不可以）、在相同的端 口上使用相同的协议。

### 5. CRSF

跨站点请求伪造(Cross—Site Request Forgery)

1. 用户C打开浏览器，访问受信任网站A，输入用户名和密码请求登录网站A；

​    2.在用户信息通过验证后，网站A产生Cookie信息并返回给浏览器，此时用户登录网站A成功，可以正常发送请求到网站A；

​    3. 用户未退出网站A之前，在同一浏览器中，打开一个TAB页访问网站B；

​    4. 网站B接收到用户请求后，返回一些攻击性代码，并发出一个请求要求访问第三方站点A；

5. 浏览器在接收到这些攻击性代码后，根据网站B的请求，在用户不知情的情况下携带Cookie信息，向网站A发出请求。网站A并不知道该请求其实是由B发起的，所以会根据用户C的Cookie信息以C的权限处理该请求，导致来自网站B的恶意代码被执行。 

### 6. let const var

let const 有块级作用域，没有变量提升，不可以重复申明

const 定义的是常量，基本数据类型不可变，对象地址不可变

### 7. 判断一个对象是否是数组

```js
let arr = [1, 2, 3]
console.log(arr.__proto__.constructor === Array); // true
console.log(arr.constructor === Array); // true
console.log(arr.__proto__ === Array.prototype); // true
```

```js
const arr = [1, 2]
console.log(Array.isArray(arr))
console.log(arr.__proto__ === Array.prototype);
console.log(arr instanceof Array);
console.log(arr.constructor === Array); 

function isArray(arr) {
    if(typeof arr === 'object') {
        return Object.prototype.toString.call(arr) === '[object Array]'
    }
    return false
}
console.log(isArray(arr));
```

### 8. typeof

 typeof运算符的返回类型为字符串，值包括如下几种：

​    \1. 'undefined'       --未定义的变量或值

​    \2. 'boolean'         --布尔类型的变量或值

​    \3. 'string'           --字符串类型的变量或值

​    \4. 'number'         --数字类型的变量或值

​    \5. 'object'          --对象类型的变量或值，或者null(这个是js历史遗留问题，将null作为object类型处理)

​    \6. 'function'         --函数类型的变量或值

### 9. 防抖 节流

```js
function debounce(fn,delay){
    let timer = null //借助闭包
    return function() {
        if(timer){
            clearTimeout(timer) 
        }
        timer = setTimeout(fn,delay) // 简化写法
    }
}
// 然后是旧代码
function showTop  () {
    var scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
　　console.log('滚动条位置：' + scrollTop);
}
window.onscroll = debounce(showTop,1000) // 为了方便观察效果我们取个大点的间断值，实际使用根据需要来配置


// 思路：一段时间fn不会执行
function throttle(fn, delay) {
  let valid = true // valid标识fn能否执行
  return function() {
    if(!valid) {
      return
    }
    valid = false
    setTimeout(() => {
      fn()
      valid = true
    }, delay)
  }
}


function showTop  () {
    var scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
　　console.log('滚动条位置：' + scrollTop);
}
// onscroll绑定的是返回的函数
window.onscroll = throttle(showTop,1000) 
```

应用场景

防抖(debounce)

　　　　search搜索联想，用户在不断输入值时，用防抖来节约请求资源。

　　　　window触发resize的时候，不断的调整浏览器窗口大小会不断的触发这个事件，用防抖来让其只触			  发一次

节流(throttle)

　　　　鼠标不断点击触发，mousedown(单位时间内只触发一次)

　　　　监听滚动事件，比如是否滑到底部自动加载更多，用throttle来判断

### 10. TypeError

```js
console.log(a) // Uncaught ReferenceError: a is not defined

var a = 1
a() // Uncaught TypeError: a is not a function
console.log(a.info.name); // Uncaught TypeError: Cannot read property 'name' of undefined
```

### 11. 异步输出的顺序

```js
const { resolve } = require("path");

setTimeout(() => {
  console.log(1)
}, 0)

function fnA() {
  console.log(2);
}

new Promise((resolve) => {
  console.log(3)
  resolve(4)
}).then(value => {
  console.log(value)
    // 微任务中的宏任务
  setTimeout(() => {
    console.log(5);
  }, 0)
})

fnA()

// 同步代码中的宏任务，所以6 7会比5先输出
setTimeout(() => {
  console.log(6);
  new Promise((resolve) => {
    resolve()
  }).then(() => {
    console.log(7);
  })
})
```

```
3
2
4
1
6
7
5
```

### 12.  大量图片，优化加载

- 图片懒加载，在页面上的未可视区域可以添加一个滚动事件，判断图片位置与浏览器顶端的距离与页面的距离，如果前者小于后者，优先加载。
- 如果为幻灯片、相册等，可以使用图片预加载技术，将当前展示图片的前一张和后一张优先下载。
- 如果图片为css图片，可以使用`CSSsprite`，`SVGsprite`，`Iconfont`、`Base64`等技术。
- 如果图片过大，可以使用特殊编码的图片，加载时会先加载一张压缩的特别厉害的缩略图，以提高用户体验。
- 如果图片展示区域小于图片的真实大小，则因在服务器端根据业务需要先行进行图片压缩，图片压缩后大小与展示一致。

### 13. 月球围着太阳转

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <style>
    .wrapper {
      width: 500px;
      height: 500px;
      border-radius: 50%;
      border: 1px solid black;
      position: relative;
      margin: 100px;
      animation: rotate 5s linear infinite;
    }
    .round {
      width: 50px;
      height: 50px;
      border-radius: 50%;
      background-color: skyblue;
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      margin: auto
    }
    .square {
      width: 50px;
      height: 50px;
      background-color: slateblue;
      position: absolute;
      /* left: 50%;
      transform: translate(-50%, -50%);
      border-radius: 50%; */
    }
  </style>
</head>
<body>
  <div class="wrapper">
    <div class="round"></div>
    <div class="square"></div>
  </div>

  <script>
    window.onload = function() {
      const square = document.getElementsByClassName('square')[0]
      
      // 起始位置，先假设square没有宽高
      let initLeft = 250
      let initTop = 0
      let angle = 0

      // 半径
      let radius = 250
      
      setInterval(() => {
        angle++
        let currentLeft= initLeft + radius * Math.sin(angle/180*Math.PI)
        let currentTop = initTop + (radius - radius * Math.cos(angle/180*Math.PI))
        square.style.left = (currentLeft -25) + 'px'
        square.style.top = (currentTop - 25) + 'px'
        console.log(square.style.left, square.style.top);
      }, 10)

    }
  </script>
</body>
</html>
```

### 14. JavaScript原型，原型链 ? 有什么特点？

- 每个对象都会在其内部初始化一个属性，就是`prototype`(原型)，当我们访问一个对象的属性时
- 如果这个对象内部不存在这个属性，那么他就会去`prototype`里找这个属性，这个`prototype`又会有自己的`prototype`，于是就这样一直找下去，也就是我们平时所说的原型链的概念
- 关系：`instance.constructor.prototype = instance.__proto__`
- 特点：
  - `JavaScript`对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变
- 当我们需要一个属性的时，`Javascript`引擎会先看当前对象中是否有这个属性， 如果没有的
- 就会查找他的`Prototype`对象是否有这个属性，如此递推下去，一直检索到 `Object` 内建对象
- **原型：**
  - `JavaScript`的所有对象中都包含了一个 `[__proto__]` 内部属性，这个属性所对应的就是该对象的原型
  - JavaScript的函数对象，除了原型 `[__proto__]` 之外，还预置了 `prototype` 属性
  - 当函数对象作为构造函数创建实例时，该 prototype 属性值将被作为实例对象的原型 `[__proto__]`。
- **原型链：**
  - 当一个对象调用的属性/方法自身不存在时，就会去自己 `[__proto__]` 关联的前辈 `prototype` 对象上去找
  - 如果没找到，就会去该 `prototype` 原型 `[__proto__]` 关联的前辈 `prototype` 去找。依次类推，直到找到属性/方法或 `undefined` 为止。从而形成了所谓的“原型链”
- **原型特点：**
  - `JavaScript`对象是通过引用来传递的，当修改原型时，与之相关的对象也会继承这一改变

### 15. 谈谈你对webpack的看法

- `WebPack` 是一个模块打包工具，你可以使用`WebPack`管理你的模块依赖，并编绎输出模块们所需的静态文件。它能够很好地管理、打包`Web`开发中所用到的`HTML`、`Javascript`、`CSS`以及各种静态文件（图片、字体等），让开发过程更加高效。对于不同类型的资源，`webpack`有对应的模块加载器。`webpack`模块打包器会分析模块间的依赖关系，最后 生成了优化且合并后的静态资源

### 16. 说说严格模式的限制

- 变量必须声明后再使用
- 函数的参数不能有同名属性，否则报错
- 不能使用with语句
- 不能对只读属性赋值，否则报错
- 不能使用前缀0表示八进制数，否则报错
- 不能删除不可删除的属性，否则报错
- 不能删除变量`delete prop`，会报错，只能删除属性`delete global[prop]`
- `eval`不会在它的外层作用域引入变量
- `eval`和`arguments`不能被重新赋值
- `arguments`不会自动反映函数参数的变化
- 不能使用`arguments.callee`
- 不能使用`arguments.caller`
- 禁止`this`指向全局对象
- 不能使用`fn.caller`和`fn.arguments`获取函数调用的堆栈
- 增加了保留字（比如`protected`、`static`和`interface`）

### 17. 快速的让一个数组乱序

```javascript
var arr = [1,2,3,4,5,6,7,8,9,10];
arr.sort(function(){
    return Math.random() - 0.5;
})
console.log(arr);
```

### 18. 获取页面中所有的checkbox

```html
<div>
    <input type="text">
    <h1>hello world</h1>
    <input type="email">
    <input type="checkbox">
    <input type="checkbox">
</div>

<script>
    let inputList = Array.from(document.getElementsByTagName('input'))
    let checkboxList = inputList.filter(item => item.type === 'checkbox')
    console.log(checkboxList);
    console.log(inputList);
</script>
```

### 19. DOM操作

### 20. 怎样添加、移除、移动、复制、创建和查找节点

**创建新节点**

```js
createDocumentFragment()    //创建一个DOM片段
createElement()   //创建一个具体的元素
createTextNode()   //创建一个文本节点
```

**添加、移除、替换、插入**

```js
appendChild()      //添加
removeChild()      //移除
replaceChild()      //替换
insertBefore()      //插入
```

**查找**

```js
getElementsByTagName()    //通过标签名称
getElementsByName()     //通过元素的Name属性的值
getElementById()        //通过元素Id，唯一性
```

### 21. 数组去重

```js
function unique (arr) {
  return Array.from(new Set(arr))
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
 //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {}, {}]

[...new Set(arr)]
//代码就是这么少----（其实，严格来说并不算是一种，相对于第一种方法来说只是简化了代码）
```

### 22. 封装一个函数，参数是定时器的时间，.then执行回调函数

```js
function sleep (time) {
    return new Promise((resolve) => setTimeout(resolve, time));
}
```

### 23. 怎么判断两个对象相等？

```js
obj={
    a:1,
    b:2
}
obj2={
    a:1,
    b:2
}
obj3={
    a:1,
    b:'2'
}
```

> 可以转换为字符串来判断

```js
JSON.stringify(obj)==JSON.stringify(obj2);//true
JSON.stringify(obj)==JSON.stringify(obj3);//false
```

### 24. 项目做过哪些性能优化？

* 路由懒加载

  ```
  路由懒加载的主要作用就是将路由对应的组件打包成一个个的js代码块.只有在这个路由被访问到的时候, 才加载对应的组件，否则所有页面都被打包在一个js文件中，服务器一次性请求会花费一定的时间
  ```

* 图片懒加载

  使用VueLazyLoad插件

* 减少http请求

  使用雪碧图

* 防抖、节流

  避免回调函数频繁触发

* 长列表优化

  监听滚动事件，scollHeight - scollTop = clientHeight

* 减少dom操作

  使用虚拟dom

### 25. 拷贝

**浅拷贝**

- `Object.assign`
- 或者展开运算符

**深拷贝**

- 可以通过 `JSON.parse(JSON.stringify(object))` 来解决

```js
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
}
let b = JSON.parse(JSON.stringify(a))
a.jobs.first = 'native'
console.log(b.jobs.first) // FE
```

**该方法也是有局限性的**

- 会忽略 `undefined`
- 不能序列化函数
- 不能解决循环引用的对象

### 26. Event loop 

- 执行同步代码，这属于宏任务
- 执行栈为空，查询是否有微任务需要执行
- 执行所有微任务
- 必要的话渲染 UI
- 然后开始下一轮 `Event loop`，执行宏任务中的异步代码

### 27. 事件流

**事件流分为两种，捕获事件流和冒泡事件流**

- 捕获事件流从根节点开始执行，一直往子节点查找执行，直到查找执行到目标节点
- 冒泡事件流从目标节点开始执行，一直往父节点冒泡查找执行，直到查到到根节点

> 事件流分为三个阶段，一个是捕获节点，一个是处于目标节点阶段，一个是冒泡阶段

### 28. 继承

#### 1. 原型式继承

ECMAScript 5 通过增加 Object.create()方法将原型式继承的概念规范化了。这个方法接收两个 参数：作为新对象原型的对象，以及给新对象定义额外属性的对象（第二个可选）。

```js
let person = {
 name: "Nicholas",
 friends: ["Shelby", "Court", "Van"]
};
let anotherPerson = Object.create(person, {
 name: {
 value: "Greg"
 }
});
console.log(anotherPerson.name); // "Greg"
```

原型式继承非常适合不需要单独创建构造函数，但仍然需要在对象间共享信息的场合。但要记住， 属性中包含的引用值始终会在相关对象间共享，跟使用原型模式是一样的。

原型中包含的引用值会在所有实例间共享，不能给父类构造函数传参

#### 2. 盗用构造函数

在子类中，使用call，apply方法，调用父类中的构造函数，优点是可以传参，缺点是不能使用父类原型上的方法

#### 3. 组合继承

组合继承（有时候也叫伪经典继承）综合了原型链和盗用构造函数，将两者的优点集中了起来。基 本的思路是使用原型链继承原型上的属性和方法，而通过盗用构造函数继承实例属性。这样既可以把方 法定义在原型上以实现重用，又可以让每个实例都有自己的属性。来看下面的例子：

```js
function SuperType(name){
 this.name = name;
 this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function() {
 console.log(this.name);
};
function SubType(name, age){
 // 继承属性
 SuperType.call(this, name);
 this.age = age;
}
// 继承方法
SubType.prototype = new SuperType();
SubType.prototype.sayAge = function() {
 console.log(this.age);
};
let instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
console.log(instance1.colors); // "red,blue,green,black"
instance1.sayName(); // "Nicholas";
instance1.sayAge(); // 29
let instance2 = new SubType("Greg", 27);
console.log(instance2.colors); // "red,blue,green"
instance2.sayName(); // "Greg";
instance2.sayAge(); // 27
```

是父类构造函数始终会被调用两次

#### 4. 寄生式组合继承

```js
function inheritPrototype(subType, superType) {
 let prototype = Object.create(supertype.prototype); // 创建对象
 prototype.constructor = subType; // 增强对象
 subType.prototype = prototype; // 赋值对象
} 

function SuperType(name) {
 this.name = name;
 this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function() {
 console.log(this.name);
};
function SubType(name, age) {
 SuperType.call(this, name); 
 this.age = age;
}
inheritPrototype(SubType, SuperType);
SubType.prototype.sayAge = function() {
 console.log(this.age);
}; 
```

### 29. 排序

```js
function bubbleSort(arr) {
  for(let i = 0; i < arr.length - 1; i++) {
    for(let j = 0; j < arr.length - 1 - i; j++) {
      if(arr[j] > arr[j + 1]) {
        let m = arr[j]
        arr[j] = arr[j + 1]
        arr[j + 1] = arr[j]
      }
    }
  }
}

function quickSort(arr) {
  if(arr.length === 0) {
    return []
  }
  let midIndex = Math.floor(arr.length / 2)
  let midValue = arr[midIndex]
  let left = []
  let right = []

  for(let item of arr) {
    if(item < midValue) {
      left.push(item)
    }else {
      right.push(item)
    }
  }

  return quickSort(left).concat(midValue, quickSort(right))
}
```

### 30. 输出今天的日期

```js
function getDate() {
    const d = new Date()
    const year = d.getFullYear()
    let month = d.getMonth() + 1
    month = month < 10 ? '0' + month : month
    const day = d.getDate()
    let date = year + '-' + month + '-' + day
    return date
}
console.log(getDate());
```

### 31. 解析url

```js
let url = 'http://item.taobao.com/item.html?a=1&b=2&c=3&d=xxx'
function parseUrl(url) {
    let query = url.split('?')[1]
    let queryArr = query.split('&')
    const map = {}
    for(let item of queryArr) {
        map[item.split('=')[0]] = item.split('=')[1]
    }
    return map
}
console.log(parseUrl(url));
```

### 32. 立即执行函数作用域

```js
var bo = 10;
function foo() {
    console.log(bo);
}
(function() {
    var bo = 20;
    foo();
})();
(function (func) {
    var bo = 30;
    func();
})(foo)
// 10 10
// 函数执行的时候，变量查找从函数定义的位置开始找
```

### 33. 原型链

```js
Function.prototype.a = () => alert(1);
Object.prototype.b = () => alert(2);
function A() {};
var a = new A();
// a.a(); 
console.log(a.a); // undefined
console.log(a);
console.log(A.prototype); // Object
// a.b(); // 2
```

```js
function Person(name) {
   this.name = name;
}

let p = new Person("Tom");
console.log(p.__proto__); // Object
console.log(Person.__proto__); // f()
console.log(Function.prototype); // f() 和上面的一样
```



### 34. 不借助第三个数，完成两个数交换

[JavaScript不借助第三个变量交换a,b两个变量值_web_hwg的博客-CSDN博客](https://blog.csdn.net/web_hwg/article/details/75045689)

方式一：

```js
a = a + b
b = a - b
a = a - b
```

方式二：

```js
a = {a:a, b:b}
b = a.b
a = a.a
```

方式三：

```js
a = [b, b=a][0]
```

### 35. script标签defer和async的区别

**推迟执行脚本**

HTML 4.01 为script标签，增加了defer属性，这个属性表示脚本在执行的时候不会改变页面的结构。也就是说，脚本会被延迟到整个页面都解析完毕后再运行。因此，在`<script>`元素上 设置 defer 属性，相当于告诉浏览器立即下载，但延迟执行。

```html
<!DOCTYPE html>
<html>
 <head>
 <title>Example HTML Page</title>
 <script defer src="example1.js"></script>
 <script defer src="example2.js"></script>
 </head>
 <body>
 <!-- 这里是页面内容 -->
 </body>
</html>

```

虽然这个例子中的`<script>`元素包含在页面的`<head>`中，但它们会在浏览器解析到结束的
`</html>`标签后才会执行。HTML5 规范要求脚本应该按照它们出现的顺序执行，因此第一个推迟的脚
本会在第二个推迟的脚本之前执行，而且两者都会在 DOMContentLoaded 事件之前执行（关于事件，
请参考第 17 章）。不过在实际当中，推迟执行的脚本不一定总会按顺序执行或者在 DOMContentLoaded
事件之前执行，因此最好只包含一个这样的脚本。
如前所述，defer 属性只对外部脚本文件才有效。这是 HTML5 中明确规定的，因此支持 HTML5
的浏览器会忽略行内脚本的 defer 属性。IE4~7 展示出的都是旧的行为，IE8 及更高版本则支持 HTML5
定义的行为。
对 defer 属性的支持是从 IE4、Firefox 3.5、Safari 5 和 Chrome 7 开始的。其他所有浏览器则会忽略这
个属性，按照通常的做法来处理脚本。考虑到这一点，还是把要推迟执行的脚本放在页面底部比较好。

**异步执行脚本**
HTML5 为`<script>`元素定义了 async 属性。从改变脚本处理方式上看，async 属性与 defer 类
似。当然，它们两者也都只适用于外部脚本，都会告诉浏览器立即开始下载。不过，与 defer 不同的
是，标记为 async 的脚本并不保证能按照它们出现的次序执行，比如：

```html
 <!DOCTYPE html>
<html>
 <head>
 <title>Example HTML Page</title>
 <script async src="example1.js"></script>
 <script async src="example2.js"></script>
 </head>
 <body>
 <!-- 这里是页面内容 -->
 </body>
</html>
```

在这个例子中，第二个脚本可能先于第一个脚本执行。因此，重点在于它们之间没有依赖关系。给
脚本添加 async 属性的目的是告诉浏览器，不必等脚本下载和执行完后再加载页面，同样也不必等到
该异步脚本下载和执行后再加载其他脚本。正因为如此，异步脚本不应该在加载期间修改 DOM。
异步脚本保证会在页面的 load 事件前执行，但可能会在 DOMContentLoaded（参见第 17 章）之
前或之后。Firefox 3.6、Safari 5 和 Chrome 7 支持异步脚本。使用 async 也会告诉页面你不会使用
document.write，不过好的 Web 开发实践根本就不推荐使用这个方法。

### 36. 图片加载成功或失败

成功load，失败error

### 37. 函数调用模式

https://www.cnblogs.com/lsy0403/p/5862859.html

函数调用

对象方法调用

构造函数

函数上下文

### 38. this

默认，隐式，显式，new，箭头

```js
let obj = {
    a:20,
    b:this.a + 10, // NaN
    // b: this, // window
    say:function() {
        return(this.a + 10)
    }
}
console.log(obj.b);// NaN
console.log(obj.say(),obj.say)// 30, f() 
```

### 39. window.onload和DOMContentLoaded

一般情况下，DOMContentLoaded事件要在window.onload之前执行，当DOM树构建完成的时候就会执行 DOMContentLoaded事件，而window.onload是在页面载入完成的时候，才执行，这其中包括图片等元素。大多数时候我们只是想在 DOM树构建完成后，绑定事件到元素，我们并不需要图片元素，加上有时候加载外域图片的速度非常缓慢。

### 40. 阻止事件冒泡

当需要停止冒泡行为时，可以使用

```js
function stopBubble(e) { 
//如果提供了事件对象，则这是一个非IE浏览器 
if ( e && e.stopPropagation ) 
    //因此它支持W3C的stopPropagation()方法 
    e.stopPropagation(); 
else 
    //否则，我们需要使用IE的方式来取消事件冒泡 
    window.event.cancelBubble = true; 
}
```

当需要阻止默认行为时，可以使用

```js
//阻止浏览器的默认行为 
function stopDefault( e ) { 
    //阻止默认浏览器动作(W3C) 
    if ( e && e.preventDefault ) 
        e.preventDefault(); 
    //IE中阻止函数器默认动作的方式 
    else 
        window.event.returnValue = false; 
    return false; 
}
```

### 42. jsonp

https://www.cnblogs.com/cryRoom/p/8418961.html

### 43. es6新增内容

* let const
* 解构赋值
* 扩展运算符
* 函数参数允许设置默认值
* 模块化
* Set Map
* 数组新增了一些方法
* promise
* async await
* 箭头函数
* class

### 44. 路由模式

[（三十四）Vue路由原理及应用 · 语雀 (yuque.com)](https://www.yuque.com/litingwei/yt9psx/xw1cf7)

### 45. 表单事件

* select：在文本框（或 textarea）上当用户选择了一个或多个字符时触发。
* input 表单输入事件
* change 变化
* invalid  用户提交表单时，如果表单元素的值不满足校验条件，就会触发`invalid`事件。
* submit  提交
* blur 失去焦点
* focus 获取焦点

### 46. 模拟push的三种方式

* splice
* reverse unshift reverse
* toString + split

