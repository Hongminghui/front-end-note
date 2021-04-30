[TOC]

<div STYLE="page-break-after: always;"></div>

跨浏览器 跨终端开发咋实现

## Vue学习

### 一. 邂逅vuejs

#### 1. 渐进式框架

渐进式意味着你可以将Vue作为你应用的一部分嵌入其中，带来更丰富的交互体验。或者如果你希望将更多的业务逻辑使用Vue实现，那么Vue的核心库以及其生态系统。比如Core+Vue-router+Vuex，也可以满足你各种各样的需求。（可以作为应用的一部分，也可以全部使用vue开发）

#### 2. MVVM

View层：视图层

​	在我们前端开发中，通常就是DOM层。主要的作用是给用户展示各种信息。

Model层：数据层

​	数据可能是我们固定的死数据，更多的是来自我们服务器，从网络上请求下来的数据。

ViewModel层：视图模型层

视图模型层是View和Model沟通的桥梁。一方面它实现了Data Binding，也就是数据绑定，将Model的改变实时的反应到View中，另一方面它实现了DOM Listener，也就是DOM监听，当DOM发生一些事件(点击、滚动、touch等)时，可以监听到，并在需要的情况下改变对应的Data。

![image-20200912115158203](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20200912115158203.png)



#### 3. vue的生命周期

[vue的生命周期详解](https://segmentfault.com/a/1190000019743049)

[简书](https://www.jianshu.com/p/9a20caf470bf)

### 二. vue的基本语法

#### 0. 初体验

```html

<div id="app">
  <h1>{{message}}</h1>
</div>

<script src="js/vue.js"></script> <--这个文件在vue官网下></-->
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    }
  })
</script>
```



#### 1. mustache语法

```javascript
const app = new Vue({
  //el 此vue实例将要管理的组件
  el: '#app',
  data: {
    message: '你好啊',
    name: '小马哥',
    url: '<a href="http://www.baidu.com">百度一下</a>'
  }
})
//双大括号，并可以做一些简单的操作
<h2>{{message}} + ' ' + {{name}}</h2>
```

#### 2.vue的简单指令

```html
<!--只渲染一次-->
<h1 v-once>{{message}} {{name}}</h1>

<!--当成html语法来解析-->
<h2 v-html="url"></h2>

<!--文本覆盖-->
<h2 v-text="message">, 李银河</h2>

<!--不渲染,就显示{{message}}-->
<h2 v-pre>{{message}}</h2>

<!--渲染完成后再显示，渲染完成前有v-cloak属性，渲染后自动消失-->
<div id="app" v-cloak>
  <h2>{{message}}</h2>
</div>
<style>
    div[v-cloak]{
        display: none;
    }
</style>
```

#### 3.v-bind

**作用**：动态绑定属性，比如动态绑定a元素的href属性，比如动态绑定img元素的src属性

**缩写**：:

**动态绑定class**

对象语法（数组语法略）

和普通的类同时存在，并不冲突，如果isActive和isLine都为true，那么会有title/active/line三个类

```html
<h2 class="title" :class="{'active': isActive, 'line': isLine}">Hello World</h2>
```

**动态绑定style**

我们可以利用v-bind:style来绑定一些CSS内联样式。

在写CSS属性名的时候，比如font-size，我们可以使用驼峰式 (camelCase) fontSize，或短横线分隔 (kebab-case，记得用单引号括起来) ‘font-size’

```html
<!--currentSize, currentColor来自data-->
<h2 :style="{fontSize: currentSize, color: currentColor}">{{message}}</h2>
```

#### 4. 计算属性

对数据进行一些转化后再显示，或者需要将多个数据结合起来进行显示

```javascript
<h2>{{fullName}}</h2>

computed: {
  //计算属性写成函数，可以认为是语法糖，实际上是计算属性的getter，提供读取操作
  fullName() {
    return this.firstName + ' ' + this.lastName
  }
}
```

methods和computed看起来都可以实现我们的功能，那么为什么还要多一个计算属性这个东西呢？

原因：计算属性会进行缓存，如果多次使用时，计算属性只会调用一次。

#### 5. 事件监听 **v-on**

 **作用**：绑定事件监听器 **缩写**：@

* v-on 的参数问题

  情况一：如果该方法不需要额外参数，那么方法后的()可以不添加。

  但是注意：如果方法本身中有一个参数，那么会默认将原生事件event参数传递进去

  情况二：如果需要同时传入某个参数，同时需要event时，可以通过$event传入事件。

```html
methods: {
  btnClick1() {
    console.log(this.message)
  },
  btnClick2(event) {
    console.log(event)
  },
  btnClick3(a, event) {
    console.log('++++++', a, event)
  }
}

<!--当事件调用的方法无形参时，加不加括号无区别-->
  <button @click="btnClick1">按钮一</button>
  <button @click="btnClick1()">按钮二</button>

  <!--当事件调用的方法有一个参数时，如果不写括号，会将事件浏览器产生的event事件（点击，滚动等）作为参数传入
                              如果写了括号，但没传值，则为参数为undefined
                              如果写入一个未定义的变量，则会报错-->
  <button @click="btnClick2">按钮三</button>
  <button @click="btnClick2()">按钮四</button>
  <!--<button @click="btnClick2(s)">按钮五</button-->

  <button @click="btnClick3(message, $event)">按钮六</button>
  <button @click="btnClick3(message)">按钮七</button>
  <button @click="btnClick3()">按钮八</button>
  <!--不写括号，将浏览器产生的事件作为第一个参数，第二个参数为undefined-->
  <button @click="btnClick3">按钮九</button>
```

* v-on的修饰符 .once .stop .native(监听组件根元素的原生事件) .prevent(阻止默认行为)

```html
<!--只触发一次-->
<button @click.once="btnClick1">按钮一</button>

<!--阻止事件冒泡-->
<div id="app" @click="divClick">aaa
  <button @click.stop="buttonClick">按钮</button>
</div>

<!--阻止表单默认提交，变成点击后触发btnClick1事件-->
<form action="http:\\www.baidu.com">
  <input type="submit" value="提交" @click.prevent="btnClick1">
</form>

<!--enter松开时，btnClick1被触发-->
<input type="text" @keyup.enter="btnClick1">

```

#### 6.条件渲染 v-if v-show

```html
<h2 v-if="score > 90">优秀</h2>
<h2 v-else-if="score > 80">良好</h2>
<h2 v-else>及格</h2>

<h2 v-show="score > 90">优秀</h2>
```

v-if v-show对比

相同点：都用于决定一个元素是否渲染

不同点：v-if当条件为false时，压根不会有对应的元素在DOM中。

​			  v-show当条件为false时，仅仅是将元素的display属性设置为none而已。

选择：当需要在显示与隐藏之间切片很频繁时，使用v-show

​		   当只有一次切换时，通过使用v-if

#### 7. 循环遍历 v-for

* 遍历数组

```html
<!--item为数组中的值-->
<li v-for="item in movies">{{item}</li>
<!--item值，index索引-->
<li v-for="(item, index) in movies">{{index}}-{{item}</li>
```

* 遍历对象

```html
<!--依次为值，键，索引-->
<li v-for="(value, key, index) in info">{{index}}.{{key}}:{{value}}</li>
```

* :key属性

![image-20200912182257453](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20200912182257453.png)

#### 8. 检测数组更新

因为Vue是响应式的，所以当数据发生变化时，Vue会自动检测数据变化，视图会发生对应的更新。

Vue中包含了一组观察数组编译的方法，使用它们改变数组也会触发视图的更新。

push() pop() shift() unshift() splice() sort() reverse()

#### 9. 过滤器

```js
<h2>总价格为: {{totalPrice | showPrice}}</h2>

filters: {
  showPrice(price) { // 会自动把把totalPrice传入
    //toFixed(2) 保留两位小数
    return '￥' + price.toFixed(2)
  }
},
```

过滤器与计算属性对比

| 计算属性                                                 | 过滤器                                                  |
| -------------------------------------------------------- | ------------------------------------------------------- |
| 依赖于一个固定的vue实例 ，在某一个实例中使用             | 不依赖于实例。可以 定义一个全局过滤器，在多个实例中使用 |
| 不接受额外参数，依赖于data属性中的变量                   | 不要求是data中的变量，可以是临时变量。可接受额外参数。  |
| 有缓存管理机制，可减少页面调用次数                       | 无缓存机制，调用次数，取决于页面中有所多少过滤器        |
| 计算属性虽默认为只读，但可以定义为对象，开启可读可写模式 | 只能读取操作                                            |
| 计算属性被作为一个类属性调用                             | 过滤器被作为一个特殊方法处理                            |

#### 10.双向绑定 v-model

 表单元素和数据的双向绑定

* 原理

```html
<!--本质-->
<!--1.文本框的值等于message的值，并且随message变化而变化，通过动态绑定属性完成-->
<!--2.message的值等于文本框的值，并随着文本框值的变化而变化，通过监听输入事件完成-->

<!--$event是指当前触发的是什么事件（鼠标事件，键盘事件等）-->
<!--$event.target则指的是事件触发的目标，即哪一个元素触发了事件，这将直接获取该dom元素-->
<input type="text" :value="message" @input="message = $event.target.value" >
{{message}}
```

* 基本使用

```html
<!--v-model 数据与界面双向绑定， 文本框时绑定的时输入内容-->
<!--修饰符lazy，等失去焦点时再更新-->
<input type="text" v-model.lazy="message">
{{message}}
```

* v-model结合radio(单选框)使用

```html
<!--1. label中for的值与input中id值相同，可实现点击label中的字，也能选中，每一个input都应该搭配一个label-->
<!--2. 数据sex与界面单选框的value双向绑定, 并且v-model可以起name的作用-->
<label for="male">
  <input type="radio" id="male" value="男" v-model="sex">男
</label>
<label for="female">
  <input type="radio" id="female" value="女" v-model="sex">女
</label>
<h2>你选择的性别是: {{sex}}</h2>
```

```javascript
data: {
  message: '你好啊',
  sex: '女'
}
```

* v-model结合select(下拉菜单)使用

```html
<!--单选框，绑定一个值(fruit声明在data中)，值为选中的value-->
<select name="abc" v-model="fruit">
  <option value="Apple">Apple</option>
  <option value="Banana">Banana</option>
  <option value="Orange">Orange</option>
</select>
<h2>您的选择为: {{fruit}}</h2>

<!--多选框，绑定一个数组(fruits声明在data中)，里面的数据是选中的value，multiple多选-->
<select name="abc" v-model="fruits" multiple>
  <option value="Pear">Pear</option>
  <option value="Apple">Apple</option>
  <option value="Banana">Banana</option>
  <option value="Orange">Orange</option>
</select>
<h2>您的选择为：{{fruits}}</h2>
```

```javascript
data: {
  message: '你好啊',
  fruit: 'Banana',
  fruits: []
}
```

* v-model结合checkbox使用

```php+HTML
<div id="app">
  <!--checkbox作为单选框，单选框的value 选中为true， 与数据isAgree双向绑定-->
  <label for="Agree">
    <input type="checkbox" id="Agree" v-model="isAgree">同意协议
  </label>
  <h2>你选择的是：{{isAgree}}</h2>
  <button :disabled="!isAgree">下一步</button>
  <br><br>
   
  <!--checkbox作为多选框，绑定一个数组-->
  <input type="checkbox" value="篮球" v-model="hobbies">篮球
  <input type="checkbox" value="足球" v-model="hobbies">足球
  <input type="checkbox" value="羽毛球" v-model="hobbies">羽毛球
  <input type="checkbox" value="台球" v-model="hobbies">台球
  <h2>你选择的是：{{hobbies}}</h2>

  <!--值绑定，数据来源于data-->
  <label v-for="item in originalHobbies" :for="item">
    <input type="checkbox" :id="item" :value="item" v-model="hobbies">{{item}}
  </label>
</div>
```

```javascript
data: {
  message: '你好啊',
  /*false不需要写单引号*/
  isAgree: 'agree',
  hobbies: [],
  originalHobbies: ['basketball', 'football', 'volleyball', 'tennis']
}
```

* 修饰符

![image-20200912190941998](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20200912190941998.png)

#### 11. watch

- **类型**：`{ [key: string]: string | Function | Object | Array }`

- **详细**：

  一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。Vue 实例将会在实例化时调用 `$watch()`，遍历 watch 对象的每一个 property。

- **示例**：

  ```js
  var vm = new Vue({
    data: {
      a: 1,
      b: 2,
      c: 3,
      d: 4,
      e: {
        f: {
          g: 5
        }
      }
    },
    watch: {
      a: function (val, oldVal) {
        console.log('new: %s, old: %s', val, oldVal)
      },
      // 方法名
      b: 'someMethod',
      // 该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
      c: {
        handler: function (val, oldVal) { /* ... */ },
        deep: true
      },
      // 该回调将会在侦听开始之后被立即调用
      d: {
        handler: 'someMethod',
        immediate: true
      },
      // 你可以传入回调数组，它们会被逐一调用
      e: [
        'handle1',
        function handle2 (val, oldVal) { /* ... */ },
        {
          handler: function handle3 (val, oldVal) { /* ... */ },
          /* ... */
        }
      ],
      // watch vm.e.f's value: {g: 5}
      'e.f': function (val, oldVal) { /* ... */ }
    }
  })
  vm.a = 2 // => new: 2, old: 1
  ```

  注意，**不应该使用箭头函数来定义 watcher 函数** (例如 `searchQuery: newValue => this.updateAutocomplete(newValue)`)。理由是箭头函数绑定了父级作用域的上下文，所以 `this` 将不会按照期望指向 Vue 实例，`this.updateAutocomplete` 将是 undefined。



### 三. 组件化

#### 1. 邂逅组件化

![image-20200912191214573](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20200912191214573.png)

![image-20200912191345347](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20200912191345347.png)

```html
<template id="cpn">
 <!--模板中需要一个根div包裹所有内容-->
  <div>
    <h2>组件一</h2>
    <p>内容一</p>
  </div>
</template>
```

```js
// 全局组件
Vue.component('cpn', {
    template: '#cpn'
  })
const cpn1 = {
    template: '#cpn1',
    data() {}
}
const app = new Vue({
  el: '#app',
  data: {

  },
  // 局部组件
  components: {
  	cpn1
  }
})
```

全局组件可以在所有vue实例管理的块中使用，局部组件只能在相应vue实例管理的块中中使用

组件属性名别用驼峰，可用连词符

#### 2. 组件中data

首先，如果不是一个函数，Vue直接就会报错。

其次，原因是在于Vue让每个组件对象都返回一个新的对象，因为如果是同一个对象的，组件在多次使用后会相互影响。

```js
const cpn = {
    template: '#cpn',
    data() {
      return {
        categories: [
          {id: 'aaa', name: '热门推荐'},
          {id: 'bbb', name: '手机数码'},
          {id: 'ccc', name: '家用家电'},
          {id: 'ddd', name: '电脑办公'},
        ]
      }
    }
  }

  const app = new Vue({
    el: '#app',
    components: {
      cpn
    }
  })
```

#### 3. 父子组件通信1

[props](https://www.cnblogs.com/360minitao/p/11850269.html)

父传子

```
1. 在子组件的props中定义需要传入的数据，可以规定类型，默认值等
2. 在子组件的模板中使用待传的值
3. 在使用子组件时，通过动态绑定属性(传入的值的变量才需要动态)，传入需要的值
4. props中定义的数据在当前组件使用方法和data中的数据一样
```

```javascript
const cpn = {
    template: '#cpn',

    props: {
      // 只规定传入类型
      cMessage: String,
        
	 // 规定传入类型，默认值，必传
      cBooks: {
        type: Array,
        default() {
          return ['book1']
        },
        required: true
      }
    }
  }

  const app = new Vue({
    el: '#app',
    data: {
      books: ['book1', 'book2', 'book3', 'book4'],
      message: 'hello world'
    },
    components: {
      cpn
    }
  })

```

```html
<!--父组件传数据给子组件-->
<div id="app">
  <!--注意：props定义的驼峰属性自动变成连词符属性-->
  <cpn :c-books="books" :c-message="message"></cpn>
</div>

<!--子组件的模板中，使用props定义的值，待传入-->
<template id="cpn">
  <div>
    <ul>
      <li v-for="item in cBooks">{{item}}</li>
    </ul>
    <h2>{{cMessage}}</h2>
  </div>

</template>
```

#### 4. 父子组件通信2

子传父

方法：自定义事件

```
目的：子组件的点击事件传递给父组件
1. 子组件模板内监听点击事件，触发子组件方法，发送自定义事件和数据
2. 子组件使用时，监听自定义事件，触发父组件的方法
```

```javascript
  const cpn = {
    template: '#cpn',
    data() {
      return {
        categories: [
          {id: 'aaa', name: '热门推荐'},
          {id: 'bbb', name: '手机数码'},
          {id: 'ccc', name: '家用家电'},
          {id: 'ddd', name: '电脑办公'},
        ]
      }
    },
    methods: {
      btnClick(item) {
          // 这里的item会自动变成父组件对应事件的参数
        this.$emit('cpn-click', item);
      }
    }
  }

  const app = new Vue({
    el: '#app',
    components: {
      cpn
    },
    methods: {
        // item自动传入，不需要在html里面写
      cpnClick(item) {
        console.log(item.name);
      }
    }
  })
```

```html
<div id="app">
  <cpn @cpn-click="cpnClick"></cpn>
</div>

<!--子组件模板-->
<template id="cpn">
  <div>
    <button v-for="item in categories"
            @click="btnClick(item)">{{item.name}}</button>
  </div>
</template>

```

#### 5. 父子组件访问

```html
<div id="app">
  <cpn ref="cpn"></cpn>
  <button @click="showMessage">按钮</button>
</div>

<template id="cpn">
  <h1>hello world</h1>
</template>

```

```js
const cpn = {
    template: '#cpn',
    data() {
      return {
        message: 'hello world'
      }
    }
  }
  const app = new Vue({
    el: '#app',
    data: {

    },
    components: {
      cpn
    },
    // 方法里能用，计算属性不能，不知道为啥
    computed: {
      pMessage() {
        return this.$refs.cpn.message;
      }
    },
    methods: {
      showMessage() {
        console.log(this.$refs.cpn.message);
      }
    }
  })
```



#### 6. 非父子组件通信

通过中央事件总线完成

第一步：在main.js中为原型添加$bus属性

```javascript
Vue.prototype.$bus = new Vue()
```

第二步：一个组件（goodsListItem.vue）发射事件

```javascript
imageLoad() {
    /*事件总线*/
    this.$bus.$emit('itemImageLoad')
},
```

第三步：另一个非父子关系组件(home.vue)接收事件

```javascript
mounted() {
  // 1.图片加载完成的事件监听
  const refresh = debounce(this.$refs.scroll.refresh, 50)
  this.$bus.$on('itemImageLoad', () => {
    refresh()
  })
},
```

goodsListItem.vue发射，Home.vue > mounted接收

#### 7. 编译作用域

**父组件模板的所有东西都会在父级作用域内编译；子组件模板的所有东西都会在子级作用域内编译。**

```
//isShow来源于父组件的data
<my-cpn v-show="isShow"></my-cpn>
```

#### 8. 插槽

组件的插槽也是为了让我们封装的组件更加具有扩展性。让使用者可以决定组件内部的一些内容到底展示什么。

最好的封装方式就是将共性抽取到组件中，将不同暴露为插槽。一旦我们预留了插槽，就可以让使用者根据自己的需求，决定插槽中插入什么内容。是搜索框，还是文字，还是菜单。由调用者自己来决定。

* 基本使用

```html
<!--子组件模板-->
<template id="cpn">
  <div>
    <h2>我是cpn</h2>
    <p>我也是cpn</p>
    <slot><button>按钮</button></slot>
  </div>
</template>

<!--父组件内使用子组件，渲染时h2标签会替代插槽，如果cpn中间没有内容，则会使用原插槽内部内容-->
<!--cpn内的内容会替换每一个插槽，如果有多个，就重复多次-->
<cpn><h2>我是插槽的替换</h2></cpn>
```

* 具名插槽

```html
<template id="cpn">
  <div>
    <slot name="left"><h2>我是左边的插槽</h2></slot>
    <slot name="center"></slot>
    <slot name="right"><h2>我是右边的插槽</h2></slot>
  </div>
</template>

<!--第二个插槽不会显示，第三个插槽显示原内容-->
<cpn><h2 slot="left">我来替换左边的插槽</h2></cpn>
```

* 作用域插槽

父组件替换插槽的标签，但是内容由子组件来提供。（与子组件相同的内容，父组件用不同形式来展示）

似乎完全可以用ref来解决

```html
<body>
<div id="app">
  <cpn></cpn>

  <cpn>
    <template slot-scope="slot">
      <span v-for="item in slot.data"> {{item}}  </span>
    </template>
  </cpn>

  <cpn>
    <template slot-scope="slot">
      {{slot.data.join('-')}}
    </template>
  </cpn>
</div>

<template id="cpn">
  <div>
    <slot :data="pLanguage">
      <ul>
        <li v-for="item in pLanguage">{{item}}</li>
      </ul>
    </slot>
  </div>
</template>
<script src="../js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: '你好啊'
    },
    components: {
      cpn: {
        template: '#cpn',
        data() {
          return {
            pLanguage: ['JavaScript', 'Java', 'Python', 'Html']
          }
        }
      },

    }
  })
</script>
</body>
```

#### 9. 实例属性

https://www.jianshu.com/p/f6f3f00cd923

```
vm.$el
获取Vue实例关联的DOM元素；

vm.$data
获取Vue实例的data选项（对象）

vm.$options
获取Vue实例的自定义属性（如vm.$options.methods,获取Vue实例的自定义属性methods）

vm.$refs
获取页面中所有含有ref属性的DOM元素（如vm.$refs.hello，获取页面中含有属性ref = “hello”的DOM元素，如果有多个元素，那么只返回最后一个）

Js代码
var app   = new Vue({    
        el:"#container",    
        data:{    
                msg:"hello,2018!"    
        },    
        address:"长安西路"    
})    
console.log(app.$el);
返回Vue实例的关联DOM元素，在这里是#container

console.log(app.$data);
返回Vue实例的数据对象data，在这里就是对象{msg：”hello，2018“}

console.log(app.$options.address);
返回Vue实例的自定义属性address，在这里是自定义属性address

console.log(app.$refs.hello)
返回含有属性ref = hello的DOM元素（如果多个元素都含有这样的属性，只返回最后一个）<h3 ref = "hello">呵呵 1{{msg}}</h3>

```

#### 10. nextTick

[Vue.nextTick 的原理和用途 - SegmentFault 思否](https://segmentfault.com/a/1190000012861862)

- **参数**：

  - `{Function} [callback]`

- **用法**：

  将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 `Vue.nextTick` 一样，不同的是回调的 `this` 自动绑定到调用它的实例上。

  > 2.1.0 起新增：如果没有提供回调且在支持 Promise 的环境中，则返回一个 Promise。请注意 Vue 不自带 Promise 的 polyfill，所以如果你的目标浏览器不是原生支持 Promise (IE：你们都看我干嘛)，你得自行 polyfill。

- **示例**：

  ```js
  new Vue({
    // ...
    methods: {
      // ...
      example: function () {
        // 修改数据
        this.message = 'changed'
        // DOM 还没有更新
        this.$nextTick(function () {
          // DOM 现在更新了
          // `this` 绑定到当前实例
          this.doSomethingElse()
        })
      }
    }
  })
  ```

#### 11. component

动态组件

[Vue组件component标签的使用 - 小玲慕斯 - 博客园 (cnblogs.com)](https://www.cnblogs.com/yjiangling/p/12794933.html)

### 四. Vue cli

[介绍 | Vue CLI (vuejs.org)](https://cli.vuejs.org/zh/guide/)

CLI是Command-Line Interface, 翻译为命令行界面, 但是俗称脚手架。Vue CLI是一个官方发布 vue.js 项目脚手架，使用 vue-cli 可以快速搭建Vue开发环境以及对应的webpack配置.

NPM的全称是Node Package Manager，是一个NodeJS包管理和分发工具，已经成为了非官方的发布Node模块（包）的标准。

#### 1. 使用前提

* 安装NodeJs	[下载地址](http://nodejs.cn/download/)

* Webpack的全局安装：npm install webpack -g

* npm install -g @vue/cli （这是cli3的版本）

  Vue CLI3初始化项目：vue create my-project（my-project是项目名字）

* npm install -g @vue/cli-init （这是cli2的版本，在安装cli3命令的基础上）

  Vue CLI2初始化项目：vue init webpack my-project

#### 2. Vue cli3

##### 2.1 初始化

```js
创建新项目：vue create my-project //（my-project是项目名字）
打包：npm run build
运行：npm run serve 
可以在package.json里看，cli2是 npm run dev
```

* 邂逅Vue cli3

  vue-cli 3 与 2 版本有很大区别

  vue-cli 3 是基于 webpack 4 打造，vue-cli 2 还是 webapck 3

  vue-cli 3 的设计原则是“0配置”，移除的配置文件根目录下的，build和config等目录

  vue-cli 3 提供了 vue ui 命令，提供了可视化配置，更加人性化

  移除了static文件夹，新增了public文件夹，并且index.html移动到public中

* 安装选项：Linter千万别选

![image-20200913015924075](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20200913015924075.png)

删除保存的preset：在c > users > admin > .vuerc

.gitconfig也在这

cli3默认会创建.git文件，并跟踪自动生成的目录

##### 2.2 目录结构

package.json：配置相关信息，版本的一个范围

package-lock.json：具体的版本

![image-20200913020100430](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20200913020100430.png)

##### 2.3 自定义配置

查看配置：

```
node_modules > vue > package.json
node_modules > vue > dist > vue.js
```

自定义配置：

* windows命令行 vue ui

* 在根目录下创建vue.config.js文件

给配置起别名

```javascript
module.exports = {
  configureWebpack: {
    resolve: {
      alias: {
        'components': '@/components',
        'content': 'components/content',
        'common': 'components/common',
        'assets': '@/assets',
        'network': '@/network',
        'views': '@/views',
      }
    }
  }
}
```

##### 2.4 项目信息

```js
new Vue({
  // 用render的结果替换#app的模板，和el: '#app'一样
  render: h => h(App),
}).$mount('#app')
```



### 五. vue-router

#### 1. vue-router的使用

0. 安装，npm install vue-router --save；在src下创建router目录

1. 创建路由组件，Home.vue, About.vue等, components目录下

2. 创建路由实例，路由表 ，router目录下的index.js(没有就自己创建)
   
   ```javascript
// 2.1 导入Vue, VueRouter, 导入要用的路由组件
   import VueRouter from "vue-router";
   import Vue from "vue";
   
    路由懒加载写法
    const Home = () => import('../components/Home')
    const HomeMessage = () => import('../components/HomeMessage')
    const HomeNews = () => import('../components/HomeNews')
    const About = () => import('../components/About')
    const User = () => import('../components/User')
    const Profile = () => import('../components/Profile')
   
    // 普通导入，和懒加载二选一
    // import Home from "./components/Home"
   
    // 2.2 使用VueRouter插件
    Vue.use(VueRouter)
   
   // 2.3 配置路由表，数组routes, 对象{path, component, } , {path, redirect}  
   // 一个路径对应一个组件，组件在components目录下创建，格式是.vue文件
   const routes = [
       {
           path: '',
           redirect: '/home',
           // name属性可以标识一个组件，不能重复
           // <router-view  name="Hello"></router-view> //将渲染Home组件
           name: 'home'
       },
       {
           path: '/about',
           component: About,
           meta: {
               title: '关于'
           }
       },
   ]
   
   // 2.4 创建vueRouter实例router，挂载routes，将mode改为history
   const router = new VueRouter({
       routes,
       mode: 'history',
       // 点击时自动加上的类，改名为active
       linkActiveClass: 'active'
   })
   
   // 2.5 导出vueRouter实例
   export default router
   ```
   
   3. 挂载到Vue实例中，即main.js
      3.1    导入router
      3.2    将router挂载到Vue实例中

   ```js
import Vue from 'vue'
import App from './App'
import router from "./router"
   
   Vue.config.productionTip = false
   
   new Vue({
     el: '#app',
     router,
     render: h => h(App)
   })
   ```


   ```

4. 使用路由（在App.vue中，App.vue是显示的页面）

   router-view是组件显示的位置，router-link最终被渲染成a标签

   

​```html
方式一： 
	<router-view></router-view>	
    <router-link to="/home" tag="button" replace>首页</router-link>
    <router-link to="/about" tag="button" replace>关于</router-link>
方式二：
	<router-view></router-view>
	<button @click="homeClick">首页</button>
    <button @click="aboutClick">关于</button>
	然后将方法定义在script中
   ```

```javascript
methods: {
    homeClick() {
        console.log('homeClick')
        // vue-router往所有的组件中都加了$router属性，有push/replace方法
        // 所有路由跳转都通过vue-router，不要直接用history跳转
        this.$router.replace('/home')
    },
    aboutClick() {
        console.log('aboutClick')
        this.$router.replace('/about')
    }
}
```

router-link的其他属性

tag: tag可以指定`<router-link>`之后渲染成什么组件, 比如上面的代码会被渲染成一个<li>元素, 而不是`<a>`

replace: replace不会留下history记录, 所以指定replace的情况下, 后退键返回不能返回到上一个页面中

active-class: 当`<router-link>`对应的路由匹配成功时, 会自动给当前元素设置一个router-link-active的class, 设置active-class可以修改默认的名称.(**当首页这个按钮被点击时，会自动加上一个router-link-active的类，可以通过这个设置点击时的样式**，例如高亮等，也可以将更改active-class的名称，如下，更改为active)

```html
<router-link to="/home" tag="button" replace active-class="active">首页</router-link>
<router-link to="/about" tag="button" replace active-class="active">关于</router-link>
<router-link to="/home" tag="button" replace>首页</router-link>
<router-link to="/about" tag="button" replace>关于</router-link>
```

#### 2. 动态路由

 1. 创建需要有动态路由(/user/zhangsan, /user/Tom)的路由组件User.vue

 2. 在路由表（router -> index.js ）里配置一个动态路由的映射

    ```javascript
    {
        path: '/user/:id',
        component: User
    }
    ```

3. 使用动态路由，在App.vue中 

   ```html
   <router-view/>
   <router-link :to="'/user/' + userId" tag="button">用户</router-link>
   useId来源于App.vue中的data
   data() {
       return {
         userId: 123
       }
   },
   ```

4. 动态路由（User.vue）里展示数据，.id与路由表中/user/:id呼应

   ```javascript
   <h2>{{$route.params.id}}</h2>
   ```
   
   或者在User.vue的js中，使用计算属性获取
   
   ```js
   computed: {
       userId() {
           //this.#route获取的是当前活跃的路由（路由表中的某一个）
           //.id 与index.js中路由表中的 path: '/user/:id' 对应
           return this.$route.params.id
   	}
   }
   
   ```
   
   

#### 3. 路由懒加载

路由懒加载的主要作用就是将路由对应的组件打包成一个个的js代码块.只有在这个路由被访问到的时候, 才加载对应的组件，否则所有页面都被打包在一个js文件中，服务器一次性请求会花费一定的时间

![image-20201213203627942](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201213203627942.png)

路由懒加载打包后的目录结果如上，

app. ---- 是App.vue组件，  vendor.----- 是第三方框架，   manifest.-----底层支撑的代码， 其他的是懒加载的路由组件

```javascript
在router -> index.js 中通过如下代码引入相应的组件，即可实现懒加载，es6写法
const Home = () => import('../components/Home')
const HomeNews = () => import('../components/HomeNews')
const HomeMessage = () => import('../components/HomeMessage')
```

#### 4. 嵌套路由

 1. 创建子组件，如HomeNews.vue, HomeMessage.vue

 2. 在路由表（router -> index.js）中导入相应组件，并配置映射关系

    ```javascript
    {
        path: '/home',
        component: Home,
        children: [
          {
            path: '',
            redirect: 'news'
          },
          {
            //子组件不加斜杠
            path: 'news',
            component: HomeNews
          },
          {
            path: 'message',
            component: HomeMessage
          }
        ]
    },
    ```

    

 3. 使用嵌套路由，在嵌套路由的父组件（Home.vue）中

    ```html
    <h2>我是首页</h2>
    <p>我是首页内容, 哈哈哈</p>
    <router-view></router-view>
    <!--此处home前记得加/-->
    <router-link to="/home/message" tag="button" replace>HomeMessage</router-link>
    <router-link to="/home/news" tag="button" replace>HomeNews</router-link>
    
    ```


#### 5. 传递参数

  1. params：见动态路由如何展示数据 path -> router-link -> $route.params.id

     方式一：使用router-link

```js
path: '/user/:id' // router > index.js
```

```html
<router-link :to="'/users/' + userId" tag="button">用户</router-link> // App.vue
```

```html
<p>{{$route.params.id}}</p> // users.vue
```

方式二：编程跳转

```
APP.VUE中

this.$router.push({name:'login',params: {openid:toro}});
然后就在login.vue里面取用toro

 this.user.openid=this.$route.params.openid
```



1. query：（1） 创建组件（profile.vue), 配置路由

   ​			 （2） 在App.vue中使用组件	

```javascript
方式一：
<router-link :to="{path: '/profile', query: {name: 'hmh', age: 22}}" tag="button">个人资料</router-link>
方式二：
<button @click="profileClick">个人资料</button>

profileClick() {
    console.log('profileClick'),
        this.$router.replace({
        path: '/profile',
        query: {
            name: 'hmh',
            age: 22
        }
    })
}
```

​				（3）使用参数 在profile.vue中

```javascript
//$route为当前活跃的路由，可以获取name, age, query, params等属性
//$router为vueRouter实例，有push,replace等方法，可以进行跳转
//是router > index.js中的router实例
<p>{{$route.query.name}}</p>
<p>{{$route.query.age}}</p>
```

3. 利用props传参

   router > index.js

```
{path: '/categories/edit/:id', component: CategoryEdit, props: true},
```

​		使用参数，CategoryEdit.vue中

```js
props: {
    id: {}
  },
```

```html
<h1>新建分类</h1>{{id}}
```





#### 6. 导航守卫	

[更多学习](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html)

需求：在一个SPA应用中, 改变多个网页的标题

vue-router提供的导航守卫主要用来监听监听路由的进入和离开的.

 1. 为路由表（router -> index.js）中每个路径添加meta

    ```javascript
    {
        path: '/profile',
            component: Profile,
                meta: {
                    title: '个人资料'
                }
    }
    ```

	2. 在router（router -> index.js）被创建后，使用前置钩子（全局前置守卫）

    ```javascript
    //to: 即将要进入的目标的路由对象.route类型
    //from: 当前导航即将要离开的路由对象.
    //next: 调用该方法后, 才能进入下一个钩子.
    router.beforeEach((to, from, next) => {
      //to拿到的可能是嵌套组件，一个数组，to.matched[0]表示嵌套的第一个组件,即自己
      document.title = to.matched[0].meta.title
      next()
    })
    ```

    每个守卫方法接收三个参数：
    
    - **`to: Route`**: 即将要进入的目标 [路由对象](https://router.vuejs.org/zh/api/#路由对象)
    - **`from: Route`**: 当前导航正要离开的路由
    - **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。
      - **`next()`**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** (确认的)。
      - **`next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
      - **`next('/')` 或者 `next({ path: '/' })`**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 [`router-link` 的 `to` prop](https://router.vuejs.org/zh/api/#to) 或 [`router.push`](https://router.vuejs.org/zh/api/#router-push) 中的选项。
      - **`next(error)`**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](https://router.vuejs.org/zh/api/#router-onerror) 注册过的回调。

#### 7. keep-alive

keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。

需求如果从home/news跳转到其他路由，跳转回home时，也要回到home/news

```
它有两个非常重要的属性:
include - 字符串或正则表达，只有匹配的组件会被缓存
exclude - 字符串或正则表达式，任何匹配的组件都不会被缓存
```

1. 将需要保留状态的组件，包裹在keep-alive中, App.vue中

   ```javascript
   //排除了Profile和User组件
   <keep-alive exclude="Profile,User">
         <router-view/>
    </keep-alive>
   ```

2. 在需要保留状态的组件（Home.vue）中，记录回到的是哪个子路由

   ```javascript
   data() {
         return {
           path: '/home/news'
         }
       },
       /*生命周期函数，创建时，销毁时执行的函数，此处用来记录该路由是否被生成和销毁*/
       created() {
         console.log('Home created')
       },
       destroyed() {
         console.log('Home destroyed')
       },
       //此函数只在keep-alive中的组件中才有效
       // 处于活跃状态时，就回到离开前的子路由
       activated() {
         //跳转到记录的路径
         this.$router.push(this.path)
       },
       // 组件内守卫，this指组件实例
       beforeRouteLeave(to, from, next){
         //记录下跳转前的路径
         this.path = this.$route.path
         next()
   }
   ```

#### 8. 路由历史

* 后端路由

```
早期的网站开发整个HTML页面是由服务器来渲染的.服务器直接生产渲染好对应的HTML页面, 返回给客户端进行展示.

但是, 一个网站, 这么多页面服务器如何处理呢?一个页面有自己对应的网址, 也就是URL.

URL会发送到服务器, 服务器会通过正则对该URL进行匹配, 并且最后交给一个Controller进行处理.

Controller进行各种处理, 最终生成HTML或者数据, 返回给前端.这就完成了一个IO操作.

上面的这种操作, 就是后端路由.

当我们页面中需要请求不同的路径内容时, 交给服务器来进行处理, 服务器渲染好整个页面, 并且将页面返回给客户顿.

这种情况下渲染好的页面, 不需要单独加载任何的js和css, 可以直接交给浏览器展示, 这样也有利于SEO的优化.

后端路由的缺点:

一种情况是整个页面的模块由后端人员来编写和维护的.

另一种情况是前端开发人员如果要开发页面, 需要通过PHP和Java等语言来编写页面代码.

而且通常情况下HTML代码和数据以及对应的逻辑会混在一起, 编写和维护都是非常糟糕的事情.
```

* 前后端分离

```
随着Ajax的出现, 有了前后端分离的开发模式.后端只提供API来返回数据, 前端通过Ajax获取数据, 并且可以通过JavaScript

将数据渲染到页面中.这样做最大的优点就是前后端责任的清晰, 后端专注于数据上, 前端专注于交互和可视化上.

并且当移动端(iOS/Android)出现后, 后端不需要进行任何处理, 依然使用之前的一套API即可.目前很多的网站依然采用这种模

式开发.
```

* 单页面富应用阶段

```
其实SPA最主要的特点就是在前后端分离的基础上加了一层前端路由.也就是前端来维护一套路由规则.前端路由的核心是什么呢？

改变URL，但是页面不进行整体的刷新。
```

#### 9. 不刷新改变url

![image-20201213171411565](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201213171411565.png)



history接口是HTML5新增的, 它有五种模式改变URL而不刷新页面.

![image-20201213171624105](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201213171624105.png)

![image-20201213171641692](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201213171641692.png)

![image-20201213171703830](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201213171703830.png)

#### 10. 路由对象

一个**路由对象 (route object)** 表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息，还有 URL 匹配到的**路由记录 (route records)**。

路由对象是不可变 (immutable) 的，每次成功的导航后都会产生一个新的对象。

路由对象出现在多个地方:

- 在组件内，即 `this.$route`

- 在 `$route` 观察者回调内

- `router.match(location)` 的返回值

- 导航守卫的参数：

  ```js
  router.beforeEach((to, from, next) => {
    // `to` 和 `from` 都是路由对象
  })
  ```

- `scrollBehavior` 方法的参数:

  ```js
  const router = new VueRouter({
    scrollBehavior(to, from, savedPosition) {
      // `to` 和 `from` 都是路由对象
    }
  })
  ```

##### [#](https://router.vuejs.org/zh/api/#路由对象属性)路由对象属性

- **$route.path**

  - 类型: `string`

    字符串，对应当前路由的路径，总是解析为绝对路径，如 `"/foo/bar"`。

- **$route.params**

  - 类型: `Object`

    一个 key/value 对象，包含了动态片段和全匹配片段，如果没有路由参数，就是一个空对象。

- **$route.query**

  - 类型: `Object`

    一个 key/value 对象，表示 URL 查询参数。例如，对于路径 `/foo?user=1`，则有 `$route.query.user == 1`，如果没有查询参数，则是个空对象。

- **$route.hash**

  - 类型: `string`

    当前路由的 hash 值 (带 `#`) ，如果没有 hash 值，则为空字符串。

- **$route.fullPath**

  - 类型: `string`

    完成解析后的 URL，包含查询参数和 hash 的完整路径。

- **$route.matched**

  - 类型: `Array<RouteRecord>`

  一个数组，包含当前路由的所有嵌套路径片段的**路由记录** 。路由记录就是 `routes` 配置数组中的对象副本 (还有在 `children` 数组)。

  ```js
  const router = new VueRouter({
    routes: [
      // 下面的对象就是路由记录
      {
        path: '/foo',
        component: Foo,
        children: [
          // 这也是个路由记录
          { path: 'bar', component: Bar }
        ]
      }
    ]
  })
  ```

  当 URL 为 `/foo/bar`，`$route.matched` 将会是一个包含从上到下的所有对象 (副本)。

- **$route.name**

  当前路由的名称，如果有的话。(查看[命名路由](https://router.vuejs.org/zh/guide/essentials/named-routes.html))

- **$route.redirectedFrom**

  如果存在重定向，即为重定向来源的路由的名字。(参阅[重定向和别名](https://router.vuejs.org/zh/guide/essentials/redirect-and-alias.html))

### 六. tabbar案例

问题一：v-if放在了slot里，导致页面不渲染

##### 1. 创建项目

```js
vue create tabbar // 项目名称不能有大写字母
```

主要目录结构：![image-20201214213135512](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201214213135512.png)

```
assets里放css,img等资源；views里放路由显示的组件，
```

##### 2. 搭建静态的页面

TabBar.vue：整个底部导航栏

```html
<div id="tabBar">
	<slot></slot>
</div>
```

```css
#tabBar {
  display: flex;
  height: 49px;
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgb(240, 240, 240);
  box-shadow: 0 -1px 1px rgba(200, 200, 200, 0.9);
}
```

TabBarItem.vue

```html
<div class="tab-bar-item" @click="itemClick">
    <!--用一个div包裹插槽，用外层的div来控制样式-->
    <!--isActive初始值为false，显示常态的图片-->
    <div v-if="isActive" class="img"><slot name="item-icon-active" ></slot></div>
    <div v-else class="img"><slot name="item-icon"></slot></div>
    <div><slot name="item-text"></slot></div>
</div>
```

```css
.tab-bar-item {
  flex-grow: 1;
  text-align: center;
  font-size: 14px;
  position: relative;
  /* 设置垂直居中 */
  top: 0;
  bottom: 0;
  margin: auto 0;
}

.img {
  height: 22px;
}
```

MainTabBar.vue

负责显示具体的图片，文字

```html
  <div id="main-tab-bar">
    <tab-bar>
      <tab-bar-item class="home" path="/home">
        <img src="../../assets/img/home-active.svg" slot="item-icon-active" alt="">
        <img slot="item-icon" src="../../assets/img/home.svg"  alt="">
        <span slot="item-text">首页</span>
      </tab-bar-item>
      <tab-bar-item path="/category">
        <img src="../../assets/img/category-active.svg" slot="item-icon-active" alt="">
        <img slot="item-icon" src="../../assets/img/category.svg"  alt="">
        <span slot="item-text">分类</span>
      </tab-bar-item>
      <tab-bar-item path="/shop-cart">
        <img src="../../assets/img/shop-cart-active.svg" slot="item-icon-active" alt="">
        <img slot="item-icon" src="../../assets/img/shop-cart.svg"  alt="">
        <span slot="item-text">购物车</span>
      </tab-bar-item>
      <tab-bar-item path="/profile">
        <img src="../../assets/img/profile-active.svg" slot="item-icon-active" alt="">
        <img slot="item-icon" src="../../assets/img/profile.svg"  alt="">
        <span slot="item-text">我的</span>
      </tab-bar-item>
    </tab-bar>

  </div>

```

```js
import TabBar from "../tabBar/TabBar";
import TabBarItem from "../tabBar/TabBarItem";
export default {
  name: "MainTabBar",
  components: {
    TabBarItem,
    TabBar
  }
}

```

```css
img {
  height: 100%;
  vertical-align: middle;
}
```

##### 3. 路由跳转

3.1 创建相应的组件，Home.vue，Category.vue等(在views目录下)

3.2 配置路由映射表

router > index.js

```js
const routes = [
  {
    path: '/',
    component: Home
  },
  {
    path: '/home',
    name: 'Home',
    component: Home
  },
  {
    path: '/category',
    name: 'Category',
    component: Category
  },
  {
    path: '/shop-cart',
    component: ShopCart
  },
  {
    path: '/profile',
    component: Profile
  }
]
```

3.3 路由跳转

TabBarItem.vue

这样就给每一个item都设置了点击跳转功能

```html
<div class="tab-bar-item" @click="itemClick">
    <!--用一个div包裹插槽，以免插槽被替换掉，样式失效-->
    <div v-if="isActive" class="img"><slot name="item-icon-active" ></slot></div>
    <div v-else class="img"><slot name="item-icon"></slot></div>
    <div :class="{active: isActive}"><slot name="item-text"></slot></div>
</div>
```

```js
// path由父组件MainTabBar.vue传入
// 例如：<tab-bar-item class="home" path="/home">
// 具体使用时，才能确定是要跳转到哪个路径
props: {
    path: {
        type: String,
        require: true
    }
},
methods: {
   itemClick() {
       // 成功解决重复点击报错的问题
       // 如果当前活跃的路径不等于此item的路径，才进行跳转
       if(this.$route.path !== this.path) {
          this.$router.replace(this.path);
       }else {
        console.log('redundant click');
      }
  }
}
```

路由渲染

App.vue

```html
<router-view></router-view>
```

##### 4. 点击变色

思路：如果活跃的路径等于此item的路径，就让他变红

用includes方法的原因是可能有子路由，this.$route.path可能比this.path更长

```js
computed: {
    isActive() {
    	// 当路由活跃时，才变红
    	return this.$route.path.includes(this.path)
    }
},
```

isActive为真时，换一张红色的图片，文字应用active这个类

```html
<div class="tab-bar-item" @click="itemClick">
    <!--用一个div包裹插槽，以免插槽被替换掉，样式失效-->
    <div v-if="isActive" class="img"><slot name="item-icon-active" ></slot></div>
    <div v-else class="img"><slot name="item-icon"></slot></div>
    <div :class="{active: isActive}"><slot name="item-text"></slot></div>
</div>
```

```css
.active {
  color: red;
}
```

图像下面有三个像素，可以通过vertical-align:center去除

### 七. vuex

#### 1.初识vuex

官方解释：Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。

它采用 **集中式存储管理** 应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

官方调试工具 **devtools**(浏览器插件)：可以记录是哪个组件修改了状态

使用方法：打开检查，在console那一行找Vue

可以简单的将其看成把需要多个组件共享的变量全部存储在一个对象里面，并且做到**响应式**

应用：如果你做过大型开放，你一定遇到过多个状态，在多个界面间的共享问题，比如用户的登录状态、用户名称、头像、地理位置信息等等。比如商品的收藏、购物车中的物品等等。这些状态信息，我们都可以放在统一的地方，对它进行保存和管理，而且它们还是响应式的。

**单一状态树**：状态信息是保存到一个Store对象中的。方便管理和维护



#### 2. 流程理解

第一步：在根目录下使用npm install vuex --save 在生产环境下面安装包

第二步：src创建store目录，store目录下创建index.js

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

import mutations from "./mutations";
import actions from "./actions";
import getters from "./getters";
import moduleA from "./module/moduleA";

//Vue.use()会执行install 
Vue.use(Vuex)

const state = {
  counter: 1000,
  students: [
    {name: 'hmh', age: 22, height: 170},
    {name: 'ml', age: 18, height: 160},
    {name: 'myr', age: 22, height: 160},
    {name: 'lq', age: 22, height: 160},
  ],
  info: {
    name: 'hmh',
    height: 170,
    age: 22
  }
}
export default new Vuex.Store({
  state,
  getters,
  mutations,
  actions,
  //a最终会被添加到state中
  modules: {
    a: moduleA
  }
})
```

第三步：挂载到main.js中

```javascript
import Vue from 'vue'
import App from './App.vue'
import store from './store'

Vue.config.productionTip = false

//main.js挂载store, 会在对象对象原型中添加这个属性$store Vue.prototype.$store = store
new Vue({
  store,
  render: h => h(App)
}).$mount('#app')
```



#### 2. 核心概念

**state** 

公共管理的数据

##### getters

从store中获取一些state变异后的状态，拿到state中的数据，并呈现改变后的数据，例如呈现平方，相加后的数据。

```javascript
getters.js文件
export default {
  /*得到state中counter的平方*/
  square(state) {
    return state.counter * state.counter
  },
  /*筛选出大于二十岁的学生数组*/
  greaterAge(state) {
    return state.students.filter( student => student.age > 20)
  },
  /*得到筛选后的数组长度*/
  //可以传两个参数，第一个参数是index.js里state对象，第二个参数是导出的getters对象，即本文件
  greaterAgeLength(state, getters) {
    return getters.greaterAge.length
  },
  /*得到小于某年龄的学生数组，age待传入*/
  //getters默认不能传自定义的参数，如果需要传递参数，只能让这个函数返回一个函数
  greaterAgeStudents(state) {
    return age => {
      return state.students.filter(student => student.age < age)
    }
  }
}
```

使用getters：

```html
App.vue
<h2>------------------App内容，getter相关测试----------------------</h2>
<p>{{$store.getters.square}}</p>
<p>{{$store.getters.greaterAge}}</p>
<p>{{$store.getters.greaterAgeLength}}</p>
<!--调用greatAgeStudents后，返回一个函数，该函数有一个参数age-->
<p>{{$store.getters.greaterAgeStudents(20)}}</p>
```

##### mutations

Vuex的store状态的更新唯一方式：**提交Mutation**，只能通过这种方式改变state里的变量

**响应式**：Vuex的store中的state是响应式的, 当state中的数据发生改变时, Vue组件会自动更新.这就要求我们必须遵守一些Vuex对应的规则:

​	1. 提前在store中初始化好所需的属性.

​	2. 当给改变state中对象属性时, 使用下面的方式:

​		方式一: 使用Vue.set(obj, 'newProp', 123)

​		方式二: 用新对象给旧对象重新赋值

```javascript
mutation.js
updateInfo(state) {
    //方式一
    state.info = {name: 'hmh', age: 23, height: 170}

    // state.info['address'] = '洛杉矶'
    // 该方式做不到响应式
	//方式二
    Vue.set(state.info, 'address', '洛杉矶')
    // 该方式是响应式

    // delete state.info.age 否
    Vue.delete(state.info, 'age') //是
}
```



**常量类型**： 当我们的项目增大时, Vuex管理的状态越来越多, 需要更新状态的情况越来越多, 那么意味着Mutation中的方法越来越多，方法过多, 使用者需要花费大量的经历去记住这些方法, 甚至是多个文件间来回切换, 查看方法名称, 甚至如果不是复制的时候, 可能还会出现写错的情况.在各种Flux实现中, 一种很常见的方案就是使用**常量替代Mutation事件的类型**，我们可以**将这些常量放在一个单独的文件中**, 方便管理以及让整个app所有的事件类型一目了然.

```javascript
mutation-type.js
const INCREMENT = 'increment'
const DECREMENT = 'decrement'
const ADDSTUDENT = 'addStudent'
const INCREMENTCOUNT = 'incrementCount'
const UPDATEINFO = 'updateInfo'
const DECREMENTCOUNT = 'decrementCount'

export {INCREMENT, DECREMENT, ADDSTUDENT, INCREMENTCOUNT, UPDATEINFO, DECREMENTCOUNT}
```

```javascript
mutation.js
//mutation负责改变state里的数据
import {INCREMENTCOUNT, DECREMENTCOUNT, UPDATEINFO, ADDSTUDENT, DECREMENT, INCREMENT}
from "./mutations-type";

export default {
  [INCREMENT](state) {
    state.counter++
  },
  [DECREMENT](state) {
    state.counter--
  },
  //待传的参数是一个值
  [INCREMENTCOUNT](state, count){
    state.counter += count
  },
  //待传的参数是一个对象
  [DECREMENTCOUNT](state, payload){
    state.counter -= payload.count
  },
  [ADDSTUDENT](state, payload){
    state.students.push(payload.student)
  }
}
```

```javascript
App.vue
//方法不实现具体功能，只负责提交到mutation对应的函数，由mutation中的函数来处理
import {INCREMENTCOUNT, DECREMENTCOUNT, DECREMENT, INCREMENT, UPDATEINFO, ADDSTUDENT}
from "./store/mutations-type";

methods: {
      increment() {
        this.$store.commit(INCREMENT)
      },
      decrement() {
        this.$store.commit(DECREMENT)
      },
      //提交的额外参数是一个值
      incrementCount(count) {
        this.$store.commit(INCREMENTCOUNT, count)
      },
      //提交的参数是一个对象
      // decrementCount(count) {
      //   this.$store.commit(DECREMENTCOUNT, {count: count})
      // },
      //以上都是普通的提交风格，下面是另一种提交风格
      decrementCount(count) {
        this.$store.commit({
          type: DECREMENTCOUNT,
          //对象字面量的增强写法
          count
        })
      },
      addStudent() {
        this.$store.commit({
          type: ADDSTUDENT,
          student: {name: 'ghs', age: 19, height: 175}
        })
      }
    }
```

以上三步才算实现了vuex中的函数功能

使用：

```html
App.vue
<h2>----------------mutation相关测试--------------------</h2>
<p>counter: {{$store.state.counter}}</p>
<p>students: {{$store.state.students}}</p>
<button @click="increment">+</button>&nbsp;
<button @click="decrement">-</button>&nbsp;
<button @click="incrementCount(5)">+5</button>&nbsp;
<button @click="decrementCount(5)">-5</button>&nbsp;
<button @click="addStudent">添加学生</button>&nbsp;
```

四部曲：定义常量类型 -> mutation.js 中的函数负责实现功能 -> App.vue中的函数负责提交到对应的函数 -> App.vue中使用函数

##### actions

> 通常情况下, Vuex要求我们Mutation中的方法必须是同步方法.主要的原因是当我们使用devtools时, 可以devtools可以帮助我们捕捉mutation的快照.但是如果是异步操作, 那么devtools将不能很好的追踪这个操作什么时候会被完成.

使用actions来处理异步操作

```javascript
App.vue 
//分发
aIncrementStudent() {
    this.$store
    	//.dispatch后返回一个Promise对象，所以后面可以跟.then
        .dispatch('incrementStudent', {name: 'lao', age: 25, height: 174})
        .then(res => {
        console.log(res)
    })
}
```

```js
// 或者是这种写法
pushStudent() {
    this.$store.dispatch({
        type: 'incrementStudent',
        student: {name: 'ml', age: 22, height: 160}
    })
}
```

```javascript
actions.js
//提交	
import {ADDSTUDENT} from "./mutations-type";
export default {
  //此处context指代$store
  incrementStudent(context, payload) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        console.log('分发了')
        context.commit({
          type: ADDSTUDENT,
          student: payload
        })
      }, 1000)
    })
  }
}
```

```javascript
mutation.js
//处理
[ADDSTUDENT](state, payload){
    state.students.push(payload.student)
}
```

```javascript
App.vue
//使用
<h2>----------------actions相关测试--------------------</h2>
<p>students: {{$store.state.students}}</p>
<button @click="aIncrementStudent">异步增加学生</button>
```

##### modules

> Vue使用单一状态树,那么也意味着很多状态都会交给Vuex来管理.当应用变得非常复杂时,store对象就有可能变得相当臃肿.

为了解决这个问题, Vuex允许我们将store分割成模块(Module), 而每个模块拥有自己的state、mutation、action、getters等

```javascript
moduleA.js
export default {
  state: {
    name: 'zhangSan',
    age: 15
  },
  getters: {
    fullInfo(state){
      return state.name + state.age
    },
    //模块中的getters函数可以传第三个参数
    fullInfo1(state, getters, rootState) {
      return getters.fullInfo + rootState.counter
    }
  }
}
```

index.js

```js
const store = new Vuex.Store({
  state,
  mutations,
  actions,
  getters,

  //最后会将a放进state里
  modules: {
    a: moduleA
  }
})
```

```html
App.vue
<!--模块中的getters与其他普通的getters调用方法一样，不要命名同名方法-->
<h2>----------------modules相关测试--------------------</h2>
<p>{{$store.state.a.name}}</p>
<p>{{$store.getters.fullInfo}}</p>
<p>{{$store.getters.fullInfo1}}</p>
```







### 八. 调试

#### devtools

查看组件是否被加入，再看element 

### 九. 优化

#### 1. 图片懒加载

一个页面图片比较多的时候，需要对界面的图片进行懒加载处理，今天遇到了，做个懒加载的笔记。

1，需要安装vue的懒加载插件。

```
npm install vue-lazyload --save-dev
```

2，需要在main.js里面进行引用。

```js
import VueLazyload from "vue-lazyload";

Vue.use(VueLazyload);

// 或者自定义

Vue.use(VueLazyload, {
preLoad: 1.3,
error: 'dist/error.png',
loading: 'dist/loading.gif',
attempt: 1
})
```

3,修改图片的路径，设置为懒加载的形式，将src改成v-lazy

```
 <img v-lazy="psdimg" class="psd" />
```

 

   今天踩过的坑总结。

   当遇到是v-for循环的时候，或者用div包裹着img的时候，需要在div上面添加v-lazy-container="{ selector: 'img' }"

```html
<div v-lazy-container="{ selector: 'img' }">
  <img data-src="//domain.com/img1.jpg">
  <img data-src="//domain.com/img2.jpg">
  <img data-src="//domain.com/img3.jpg">  
</div>
 
或者这种：
 <div>
    v-lazy-container="{ selector: 'img' }"
    class="contentDiv construction"
    v-html="content">
</div>
```



以及我将html里面的图片路径拿到后，转换成懒加载的时候，

![img](https://img2020.cnblogs.com/blog/1728151/202008/1728151-20200814170436872-1939563126.png)

 

 一定是 data-src，而不是v-lazy，要不然在接口获取的时候，拿到了图片地址，但是会一直显示不出来的。

### 十. 原理

#### 1. 响应式原理

现在是时候深入一下了！Vue 最独特的特性之一，是其非侵入性的响应式系统。数据模型仅仅是普通的 JavaScript 对象。而当你修改它们时，视图会进行更新。这使得状态管理非常简单直接，不过理解其工作原理同样重要，这样你可以避开一些常见的问题。在这个章节，我们将研究一下 Vue 响应式系统的底层的细节。

##### [如何追踪变化](https://cn.vuejs.org/v2/guide/reactivity.html#如何追踪变化)

当你把一个普通的 JavaScript 对象传入 Vue 实例作为 `data` 选项，Vue 将遍历此对象所有的 property，并使用 [`Object.defineProperty`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 把这些 property 全部转为 [getter/setter](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Working_with_Objects#定义_getters_与_setters)。`Object.defineProperty` 是 ES5 中一个无法 shim 的特性，这也就是 Vue 不支持 IE8 以及更低版本浏览器的原因。

这些 getter/setter 对用户来说是不可见的，但是在内部它们让 Vue 能够追踪依赖，在 property 被访问和修改时通知变更。这里需要注意的是不同浏览器在控制台打印数据对象时对 getter/setter 的格式化并不同，所以建议安装 [vue-devtools](https://github.com/vuejs/vue-devtools) 来获取对检查数据更加友好的用户界面。

每个组件实例都对应一个 **watcher** 实例，它会在组件渲染的过程中把“接触”过的数据 property 记录为依赖。之后当依赖项的 setter 触发时，会通知 watcher，从而使它关联的组件重新渲染。

![data](https://cn.vuejs.org/images/data.png)

##### [检测变化的注意事项](https://cn.vuejs.org/v2/guide/reactivity.html#检测变化的注意事项)

由于 JavaScript 的限制，Vue **不能检测**数组和对象的变化。尽管如此我们还是有一些办法来回避这些限制并保证它们的响应性。

##### [对于对象](https://cn.vuejs.org/v2/guide/reactivity.html#对于对象)

Vue 无法检测 property 的添加或移除。由于 Vue 会在初始化实例时对 property 执行 getter/setter 转化，所以 property 必须在 `data` 对象上存在才能让 Vue 将它转换为响应式的。例如：

```
var vm = new Vue({
  data:{
    a:1
  }
})

// `vm.a` 是响应式的

vm.b = 2
// `vm.b` 是非响应式的
```

对于已经创建的实例，Vue 不允许动态添加根级别的响应式 property。但是，可以使用 `Vue.set(object, propertyName, value)` 方法向嵌套对象添加响应式 property。例如，对于：

```
Vue.set(vm.someObject, 'b', 2)
```

您还可以使用 `vm.$set` 实例方法，这也是全局 `Vue.set` 方法的别名：

```
this.$set(this.someObject,'b',2)
```

有时你可能需要为已有对象赋值多个新 property，比如使用 `Object.assign()` 或 `_.extend()`。但是，这样添加到对象上的新 property 不会触发更新。在这种情况下，你应该用原对象与要混合进去的对象的 property 一起创建一个新的对象。

```
// 代替 `Object.assign(this.someObject, { a: 1, b: 2 })`
this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })
```

##### [对于数组](https://cn.vuejs.org/v2/guide/reactivity.html#对于数组)

Vue 不能检测以下数组的变动：

1. 当你利用索引直接设置一个数组项时，例如：`vm.items[indexOfItem] = newValue`
2. 当你修改数组的长度时，例如：`vm.items.length = newLength`

举个例子：

```
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```

为了解决第一类问题，以下两种方式都可以实现和 `vm.items[indexOfItem] = newValue` 相同的效果，同时也将在响应式系统内触发状态更新：

```
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
```

你也可以使用 [`vm.$set`](https://cn.vuejs.org/v2/api/#vm-set) 实例方法，该方法是全局方法 `Vue.set` 的一个别名：

```
vm.$set(vm.items, indexOfItem, newValue)
```

为了解决第二类问题，你可以使用 `splice`：

```
vm.items.splice(newLength)
```

##### [声明响应式 property](https://cn.vuejs.org/v2/guide/reactivity.html#声明响应式-property)

由于 Vue 不允许动态添加根级响应式 property，所以你必须在初始化实例前声明所有根级响应式 property，哪怕只是一个空值：

```
var vm = new Vue({
  data: {
    // 声明 message 为一个空值字符串
    message: ''
  },
  template: '<div>{{ message }}</div>'
})
// 之后设置 `message`
vm.message = 'Hello!'
```

如果你未在 `data` 选项中声明 `message`，Vue 将警告你渲染函数正在试图访问不存在的 property。

这样的限制在背后是有其技术原因的，它消除了在依赖项跟踪系统中的一类边界情况，也使 Vue 实例能更好地配合类型检查系统工作。但与此同时在代码可维护性方面也有一点重要的考虑：`data` 对象就像组件状态的结构 (schema)。提前声明所有的响应式 property，可以让组件代码在未来修改或给其他开发人员阅读时更易于理解。

##### [异步更新队列](https://cn.vuejs.org/v2/guide/reactivity.html#异步更新队列)

可能你还没有注意到，Vue 在更新 DOM 时是**异步**执行的。只要侦听到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作是非常重要的。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。Vue 在内部对异步队列尝试使用原生的 `Promise.then`、`MutationObserver` 和 `setImmediate`，如果执行环境不支持，则会采用 `setTimeout(fn, 0)` 代替。

例如，当你设置 `vm.someData = 'new value'`，该组件不会立即重新渲染。当刷新队列时，组件会在下一个事件循环“tick”中更新。多数情况我们不需要关心这个过程，但是如果你想基于更新后的 DOM 状态来做点什么，这就可能会有些棘手。虽然 Vue.js 通常鼓励开发人员使用“数据驱动”的方式思考，避免直接接触 DOM，但是有时我们必须要这么做。为了在数据变化之后等待 Vue 完成更新 DOM，可以在数据变化之后立即使用 `Vue.nextTick(callback)`。这样回调函数将在 DOM 更新完成后被调用。例如：

```js
<div id="example">{{message}}</div>
var vm = new Vue({
  el: '#example',
  data: {
    message: '123'
  }
})
vm.message = 'new message' // 更改数据
vm.$el.textContent === 'new message' // false
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
})
```

在组件内使用 `vm.$nextTick()` 实例方法特别方便，因为它不需要全局 `Vue`，并且回调函数中的 `this` 将自动绑定到当前的 Vue 实例上：

```js
Vue.component('example', {
  template: '<span>{{ message }}</span>',
  data: function () {
    return {
      message: '未更新'
    }
  },
  methods: {
    updateMessage: function () {
      this.message = '已更新'
      console.log(this.$el.textContent) // => '未更新'
      this.$nextTick(function () {
        console.log(this.$el.textContent) // => '已更新'
      })
    }
  }
})
```

因为 `$nextTick()` 返回一个 `Promise` 对象，所以你可以使用新的 [ES2017 async/await](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function) 语法完成相同的事情：

```js
methods: {
  updateMessage: async function () {
    this.message = '已更新'
    console.log(this.$el.textContent) // => '未更新'
    await this.$nextTick()
    console.log(this.$el.textContent) // => '已更新'
  }
}
```

#### 2. diff算法

[详解vue的diff算法 - _wind - 博客园 (cnblogs.com)](https://www.cnblogs.com/wind-lanyan/p/9061684.html)

[详解vue的diff算法原理_vue.js_脚本之家 (jb51.net)](https://www.jb51.net/article/140471.htm)

当数据发生改变时，set方法会让调用`Dep.notify`通知所有订阅者Watcher，订阅者就会调用`patch`给真实的DOM打补丁，更新相应的视图。

patch函数接收两个参数`oldVnode`和`Vnode`分别代表新的节点和之前的旧节点

- 判断两节点是否值得比较，值得比较则执行`patchVnode`
- 不值得比较则用`Vnode`替换`oldVnode`

如果两个节点都是一样的，那么就深入检查他们的子节点。如果两个节点不一样那就说明`Vnode`完全被改变了，就可以直接替换`oldVnode`。

虽然这两个节点不一样但是他们的子节点一样怎么办？别忘了，diff可是逐层比较的，如果第一层不一样那么就不会继续深入比较第二层了。（我在想这算是一个缺点吗？相同子节点不能重复利用了...）

这个函数做了以下事情：

- 找到对应的真实dom，称为`el`
- 判断`Vnode`和`oldVnode`是否指向同一个对象，如果是，那么直接`return`
- 如果他们都有文本节点并且不相等，那么将`el`的文本节点设置为`Vnode`的文本节点。
- 如果`oldVnode`有子节点而`Vnode`没有，则删除`el`的子节点
- 如果`oldVnode`没有子节点而`Vnode`有，则将`Vnode`的子节点真实化之后添加到`el`
- 如果两者都有子节点，则执行`updateChildren`函数比较子节点，这一步很重要

其他几个点都很好理解，我们详细来讲一下updateChildren

##### updateChildren

- 将`Vnode`的子节点`Vch`和`oldVnode`的子节点`oldCh`提取出来
- `oldCh`和`vCh`各有两个头尾的变量`StartIdx`和`EndIdx`，它们的2个变量相互比较，一共有4种比较方式。如果4种比较都没匹配，如果设置了`key`，就会用`key`进行比较，在比较的过程中，变量会往中间靠，一旦`StartIdx>EndIdx`表明`oldCh`和`vCh`至少有一个已经遍历完了，就会结束比较。

![image-20210215233715621](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20210215233715621.png)

## 问题

	1. vue-router   router-link中replace属性使用，未成功实现无历史记录的效果，重复点击时可能会报错，路径后面加时间戳或者加一个判断：是否是跳转相同路径，或者.catch捕捉错误
	
	2. vuex使用中 $store会有波浪线，但可以正常使用
	
	3. Cannot read property '$createElement' of undefined
	   原因：配置路由映射表时，component写成了components
	4. Duplicate named routes definition: { name: "About", path: "/users/:id" }
	   原因：配置路由映射表时，name写了相同的值


