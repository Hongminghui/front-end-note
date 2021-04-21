## Vue

### 1. v-if v-show的区别

相同点：都用于决定一个元素是否渲染

不同点：v-if当条件为false时，压根不会有对应的元素在DOM中。

​			  v-show当条件为false时，仅仅是将元素的display属性设置为none而已。

选择：当需要在显示与隐藏之间切片很频繁时，使用v-show

​		   当只有一次切换时，通过使用v-if  

### 2. Vue中$nextTick的作用

将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 `Vue.nextTick` 一样，不同的是回调的 `this` 自动绑定到调用它的实例上。

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

### 3. Vue组件通信

父子组件通信

父组件向子组件通信：

父组件通过props向子组件传递数据

子组件通过$emit发射一个自定义事件，向父组件通信

非父子组件通信

可以通过Vuex 中央事件总线 localStorage

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

vuex localStorage

### 4. 简单的双向绑定

```js
const data = {};
const input = document.getElementById('input');
// data改变，input也会改变
Object.defineProperty(data, 'text', {
  set(value) {
    input.value = value;
    this.value = value;
  }
});
// input改变，data也会改变
input.onChange = function(e) {
  data.text = e.target.value;
}
```

```html
<input v-model="sth" />
//  等同于
<input :value="sth" @input="sth = $event.target.value" />
```

```
你可以用 v-model 指令在表单 <input>、<textarea> 及 <select> 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 v-model 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

v-model 会忽略所有表单元素的 value、checked、selected attribute 的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 data 选项中声明初始值。

v-model 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：

text 和 textarea 元素使用 value property 和 input 事件；
checkbox 和 radio 使用 checked property 和 change 事件；
select 字段将 value 作为 prop 并将 change 作为事件。
```

### 5. 生命周期

> 答：总共分为8个阶段创建前/后，载入前/后，更新前/后，销毁前/后

**生命周期是什么**

> Vue 实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模版、挂载Dom -> 渲染、更新 -> 渲染、卸载等一系列过程，我们称这是Vue的生命周期

**各个生命周期的作用**

| 生命周期      | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| beforeCreate  | 组件实例被创建之初，组件的属性生效之前                       |
| created       | 组件实例已经完全创建，属性也绑定，但真实dom还没有生成，$el还不可用 |
| beforeMount   | 在挂载开始之前被调用：相关的 render 函数首次被调用           |
| mounted       | el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子    |
| beforeUpdate  | 组件数据更新之前调用，发生在虚拟 DOM 打补丁之前              |
| updated       | 组件数据更新之后                                             |
| activited     | keep-alive专属，组件被激活时调用                             |
| deactivated   | keep-alive专属，组件被销毁时调用                             |
| beforeDestory | 组件销毁前调用                                               |
| destoryed     | 组件销毁后调用                                               |

### 6. nextTick

nextTick`可以让我们在下次 DOM 更新循环结束之后执行延迟回调，用于获得更新后的 `DOM

### 7. vue-router 有哪几种导航守卫?

- 全局守卫
- 路由独享守卫
- 路由组件内的守卫

**全局守卫**

> vue-router全局有三个守卫

- `router.beforeEach` 全局前置守卫 进入路由之前
- `router.beforeResolve` 全局解析守卫(2.5.0+) 在`beforeRouteEnter`调用之后调用
- `router.afterEach` 全局后置钩子 进入路由之后

```js
// main.js 入口文件
import router from './router'; // 引入路由
router.beforeEach((to, from, next) => { 
  next();
});
router.beforeResolve((to, from, next) => {
  next();
});
router.afterEach((to, from) => {
  console.log('afterEach 全局后置钩子');
});
```

**路由独享守卫**

> 如果你不想全局配置守卫的话，你可以为某些路由单独配置守卫

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => { 
        // 参数用法什么的都一样,调用顺序在全局前置守卫后面，所以不会被全局守卫覆盖
        // ...
      }
    }
  ]
})
```

**路由组件内的守卫**

- beforeRouteEnter 进入路由前, 在路由独享守卫后调用 不能 获取组件实例 this，组件实例还没被创建
- beforeRouteUpdate (2.2) 路由复用同一个组件时, 在当前路由改变，但是该组件被复用时调用 可以访问组件实例 this
- beforeRouteLeave 离开当前路由时, 导航离开该组件的对应路由时调用，可以访问组件实例 this

### 8. Vue2.x和Vue3.x渲染器的diff算法分别说一下

> 简单来说，`diff`算法有以下过程

- 同级比较，再比较子节点
- 先判断一方有子节点一方没有子节点的情况(如果新的`children`没有子节点，将旧的子节点移除)
- 比较都有子节点的情况(核心`diff`)
- 递归比较子节点
- 正常`Diff`两个树的时间复杂度是`O(n^3)`，但实际情况下我们很少会进行跨层级的移动`DOM`，所以`Vue`将`Diff`进行了优化，从`O(n^3) -> O(n)`，只有当新旧`children`都为多个子节点时才需要用核心的`Diff`算法进行同层级比较。
- `Vue2`的核心`Diff`算法采用了双端比较的算法，同时从新旧`children`的两端开始进行比较，借助`key`值找到可复用的节点，再进行相关操作。相比`React`的`Diff`算法，同样情况下可以减少移动节点次数，减少不必要的性能损耗，更加的优雅
- 在创建`VNode`时就确定其类型，以及在`mount/patch`的过程中采用位运算来判断一个`VNode`的类型，在这个基础之上再配合核心的`Diff`算法，使得性能上较`Vue2.x`有了提升

### 9. 虚拟Dom以及key属性的作用

- 由于在浏览器中操作`DOM`是很昂贵的。频繁的操作`DOM`，会产生一定的性能问题。这就是虚拟Dom的产生原因
- `Virtual DOM`本质就是用一个原生的JS对象去描述一个`DOM`节点。是对真实DOM的一层抽象
- `VirtualDOM`映射到真实DOM要经历`VNode`的`create`、`diff`、`patch`等阶段

**key的作用是尽可能的复用 DOM 元素**

- 新旧 `children` 中的节点只有顺序是不同的时候，最佳的操作应该是通过移动元素的位置来达到更新的目的
- 需要在新旧 `children` 的节点中保存映射关系，以便能够在旧 `children` 的节点中找到可复用的节点。`key`也就是`children`中节点的唯一标识

### 10.  Vue响应式原理

```js
Vue 采用数据劫持结合发布—订阅模式的方法，通过 Object.defineProperty() 来劫持各个属性的 setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。
```

https://mp.weixin.qq.com/s/FewKP2UTfbDzDxJMjoe62A

响应式的实现分为两步：

第一步：

Object.defineProperty()拦截数据的getter，setter

第二步：

观察者模式，当读取数据时，就将其加入观察者，当数据变化时，就通知每一个观察者

### 11. key的作用

[vue中:key的作用 - 简书 (jianshu.com)](https://www.jianshu.com/p/5d771cf57012)

### 12. vdom怎么转换成真实dom

[vue考点 —— Diff算法_zhanghuali-CSDN博客](https://blog.csdn.net/zhanghuali0210/article/details/82286579)

