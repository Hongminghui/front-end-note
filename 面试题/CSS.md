CSS

### 1. 重流 重绘

[HTML页面的重绘（repaint）和重流（reflow） - Wayne-Zhu - 博客园 (cnblogs.com)](https://www.cnblogs.com/zhuzhenwei918/p/7441709.html)

重流：布局改变

重绘：颜色 背景改变

- 重绘：当渲染树中的元素外观（如：颜色）发生改变，不影响布局时，产生重绘
- 回流：当渲染树中的元素的布局（如：尺寸、位置、隐藏/状态状态）发生改变时，产生重绘回流
- 注意：JS获取Layout属性值（如：`offsetLeft`、`scrollTop`、`getComputedStyle`等）也会引起回流。因为浏览器需要通过回流计算最新值
- 回流必将引起重绘，而重绘不一定会引起回流

**如何最小化重绘(repaint)和回流(reflow)**：

- 需要要对元素进行复杂的操作时，可以先隐藏(`display:"none"`)，操作完成后再显示
- 需要创建多个`DOM`节点时，使用`DocumentFragment`创建完后一次性的加入`document`
- 缓存`Layout`属性值，如：`var left = elem.offsetLeft;` 这样，多次使用 `left` 只产生一次回流
- 尽量避免用`table`布局（`table`元素一旦触发回流就会导致table里所有的其它元素回流）
- 避免使用`css`表达式(`expression`)，因为每次调用都会重新计算值（包括加载页面）
- 尽量使用 `css` 属性简写，如：用 `border` 代替 `border-width`, `border-style`, `border-color`
- 批量修改元素样式：`elem.className` 和 `elem.style.cssText` 代替 `elem.style.xxx`

### 2. CSS选择器优先级

- `!important`
- 内联样式（`1000`）
- ID选择器（`0100`）
- 类选择器/属性选择器/伪类选择器（`0010`）
- 元素选择器/伪元素选择器（`0001`）
- 关系选择器/通配符选择器（`0000`）

### 3. 选择器解析方向

选择器从右往左解析，性能更高

[(2条消息) css面试点-CSS选择器为什么是从右往左解析_Miofly的博客-CSDN博客](https://blog.csdn.net/weixin_40013817/article/details/103120402?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.control)

### 4. css a标签的顺序

同时触发的时候，更重要的写后面

```js
a:link{text-decoration:none ; color:red ;}
a:visited {text-decoration:none ; color:black ;}
a:hover {text-decoration:underline ; color:yellow ;}
a:active {text-decoration:none ; color:pink ;}
```

### 5. css 居中方法

**水平居中**：

```
方式一：text-align: center，行内元素

方式二：margin: 0 auto，没有脱离文档流的块状元素

方式三：
	position: absolute
	left: 50%
	transform: translateX(-50%)
方式四：
	display: flex
	justify-content: center
```

**垂直居中**：

```
方式一：line-height 和行高一样

方式二：
	display:flex
	align-items: center

方式三：
    top: 50%
    transform: translateY(-50%)      
```

**水平垂直居中**：

```js
<div class="outer">
    <div class="inner"></div>
</div>
```

方法一：利用flex

```css
.outer {
    width: 300px;
    height: 300px;
    background-color: sandybrown;
    display: flex;
    justify-content: center;
    align-items: center;
}
.inner {
    width: 100px;
    height: 100px;
    background-color: seagreen;    
}
```

方式二：

```css
position: absolute
top: 50%
left: 50%
transform: translate(-50%, -50%s)
```





### 6. 文本溢出

单行文本溢出

```
.ellipsis {
   width: 300px;
   white-space: nowrap;
   text-overflow: ellipsis;
   overflow: hidden;
}
```

多行文本溢出 webkit内核浏览器

```css
.ellipsis {
    width: 300px;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 3; /* 自定义的行数 */
    overflow: hidden;
}
```

```css
p {
    position:relative;
    line-height:1.4em;
    /* 3 times the line-height to show 3 lines */
    height:4.2em;
    overflow:hidden;
}
p::after {
    content:"...";
    font-weight:bold;
    position:absolute;
    bottom:0;
    right:0;
    padding:0 20px 1px 45px;
    background:url(/newimg88/2014/09/ellipsis_bg.png) repeat-y;
}
```

### 7. 块级盒子、内联盒子

* 一个被定义成块级的（block）盒子会表现出以下行为:

```
盒子会在内联的方向上扩展并占据父容器在该方向上的所有可用空间，在绝大数情况下意味着盒子会和父容器一样宽
每个盒子都会换行
width 和 height 属性可以发挥作用
内边距（padding）, 外边距（margin） 和 边框（border） 会将其他元素从当前盒子周围“推开”
除非特殊指定，诸如标题(<h1>等)和段落(<p>)默认情况下都是块级的盒子。
```

* 如果一个盒子对外显示为 inline，那么他的行为如下:

```
盒子不会产生换行。
 width 和 height 属性将不起作用。
垂直方向的内边距、外边距以及边框会被应用但是不会把其他处于 inline 状态的盒子推开。
水平方向的内边距、外边距以及边框会被应用而且也会把其他处于 inline 状态的盒子推开。
用做链接的 <a> 元素、 <span>、 <em> 以及 <strong> 都是默认处于 inline 状态的。

我们通过对盒子display 属性的设置，比如 inline 或者 block ，来控制盒子的外部显示类型。
```

* inline-block

```
display有一个特殊的值，它在内联和块之间提供了一个中间状态。这对于以下情况非常有用:您不希望一个项切换到新行，但希望它可以设定宽度和高度，并避免上面看到的重叠。

一个元素使用 display: inline-block，实现我们需要的块级的部分效果：

设置width 和height 属性会生效。
padding, margin, 以及border 会推开其他元素。
但是，它不会跳转到新行，如果显式添加width 和height 属性，它只会变得比其内容更大。

在下一个示例中，我们将display: inline-block添加到<span>元素中。尝试将此更改为display: block 或完全删除行，以查看显示模型中的差异。

```

```
空元素
没有内容的标签 br hr等
```

### 8. 月球围着太阳转

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <style>
    .orbit {
      width: 300px;
      height: 300px;
      border-radius: 50%;
      border: 1px solid black;
      position: relative;
      animation: rotate 5s infinite linear;
    }
    .moon {
      width: 50px;
      height: 50px;
      border-radius: 50%;
      background-color: orange;
      position: absolute;
      top: -25px;
      left: 125px;
    }
    .earth {
      width: 70px;
      height: 70px;
      border-radius: 50%;
      background-color: blueviolet;
      position: absolute;
      left: 115px;
      top: 115px;
    }

    @keyframes rotate {
      from {
        transform: rotateZ(0);
      }
      to {
        transform: rotateZ(1turn);
      }
    }
  </style>
</head>
<body>
  <div class="orbit">
    <div class="moon"></div>
    <div class="earth"></div>
  </div>

</body>
</html>
```

### 9. css sprite是什么,有什么优缺点

- 概念：将多个小图片拼接到一个图片中。通过`background-position`和元素尺寸调节需要显示的背景图案。
- 优点：
  - 减少`HTTP`请求数，极大地提高页面加载速度
  - 增加图片信息重复度，提高压缩比，减少图片大小
  - 更换风格方便，只需在一张或几张图片上修改颜色或样式即可实现
- 缺点：
  - 图片合并麻烦
  - 维护麻烦，修改一个图片可能需要从新布局整个图片，样式

### 10. `display: none;`与`visibility: hidden;`的区别

- 联系：它们都能让元素不可见
- 区别：
  - `display:none`;会让元素完全从渲染树中消失，渲染的时候不占据任何空间；`visibility: hidden`;不会让元素从渲染树消失，渲染师元素继续占据空间，只是内容不可见
  - `display: none`;是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示`；visibility: hidden;`是继承属性，子孙节点消失由于继承了`hidden`，通过设置`visibility: visible;`可以让子孙节点显式
  - 修改常规流中元素的`display`通常会造成文档重排。修改`visibility`属性只会造成本元素的重绘。
  - 读屏器不会读取`display: none`;元素内容；会读取`visibility: hidden;`元素内容

### 11. `link`与`@import`的区别

1. `link`是`HTML`方式， `@import`是CSS方式
2. `link`最大限度支持并行下载，`@import`过多嵌套导致串行下载，出现`FOUC`(文档样式短暂失效)
3. `link`可以通过`rel="alternate stylesheet"`指定候选样式
4. 浏览器对`link`支持早于`@import`，可以使用`@import`对老浏览器隐藏样式
5. `@import`必须在样式规则之前，可以在css文件中引用其他文件
6. 总体来说：`link`优于`@import`

### 12. 介绍一下标准的CSS的盒子模型？低版本IE的盒子模型有什么不同的？

> - 有两种， `IE`盒子模型、`W3C`盒子模型；
> - 盒模型： 内容(content)、填充(`padding`)、边界(`margin`)、 边框(`border`)；
> - 区 别： `IE`的c`ontent`部分把 `border` 和 `padding`计算了进去;

- 盒子模型构成：内容(`content`)、内填充(`padding`)、 边框(`border`)、外边距(`margin`)
- `IE8`及其以下版本浏览器，未声明 `DOCTYPE`，内容宽高会包含内填充和边框，称为怪异盒模型(`IE`盒模型)
- 标准(`W3C`)盒模型：元素宽度 = `width + padding + border + margin`
- 怪异(`IE`)盒模型：元素宽度 = `width + margin`
- 标准浏览器通过设置 css3 的 `box-sizing: border-box` 属性，触发“怪异模式”解析计算宽高

**box-sizing 常用的属性有哪些？分别有什么作用**

- `box-sizing: content-box;` 默认的标准(W3C)盒模型元素效果
- `box-sizing: border-box;` 触发怪异(IE)盒模型元素的效果
- `box-sizing: inherit;` 继承父元素 `box-sizing` 属性的值

#### 13. position

设置position:sticky同时给一个(top,bottom,right,left)之一即可

使用条件：

1、父元素不能overflow:hidden或者overflow:auto属性。

2、必须指定top、bottom、left、right4个值之一，否则只会处于相对定位

3、父元素的高度不能低于sticky元素的高度

4、sticky元素仅在其父元素内生效

| 值       | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| absolute | 生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| fixed    | 生成绝对定位的元素，相对于浏览器窗口进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| relative | 生成相对定位的元素，相对于其正常位置进行定位。因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。 |
| static   | 默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。 |
| inherit  | 规定应该从父元素继承 position 属性的值。                     |

### 13. 如果需要手动写动画，你认为最小时间间隔是多久，为什么？（阿里）

- 多数显示器默认频率是`60Hz`，即`1`秒刷新`60`次，所以理论上最小间隔为`1/60*1000ms ＝ 16.7ms`

### 14. 左边宽度固定，右边自适应

**方法2：对右侧:div进行绝对定位，然后再设置right=0，即可以实现宽度自适应**

> 绝对定位元素的第一个高级特性就是其具有自动伸缩的功能，当我们将 `width`设置为 `auto` 的时候（或者不设置，默认为 `auto` ），绝对定位元素会根据其 `left` 和 `right` 自动伸缩其大小

```css
.outer {
    width: 100%;
    height: 500px;
    background-color: yellow;
    position: relative;
}
.left {
    width: 200px;
    height: 200px;
    background-color: red;
}
.right {
    height: 200px;
    background-color: blue;
    position: absolute;
    left: 200px;
    top:0;          
    right: 0;
}
```

**方法3：将左侧`div`进行绝对定位，然后右侧`div`设置`margin-left: 200px`**

```css
.outer {
    width: 100%;
    height: 500px;
    background-color: yellow;
    position: relative;
}
.left {
    width: 200px;
    height: 200px;
    background-color: red;
    position: absolute;
}
.right {
    height: 200px;
    background-color: blue;
    margin-left: 200px;
}
```

**方法4：使用flex布局**

```css
.outer {
    width: 100%;
    height: 500px;
    background-color: yellow;
    display: flex;
    flex-direction: row;
}
.left {
    width: 200px;
    height: 200px;
    background-color: red;
}
.right {
    height: 200px;
    background-color: blue;
    flex: 1;
}
```

### 15. 未知宽度的居中

```css
/** 2 **/
.wraper {
  position: relative;
  .box {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
}

/** 3 **/
.wraper {
  .box {
    display: flex;
    justify-content:center;
    align-items: center;
    height: 100px;
  }
}

/** 4 **/
.wraper {
  display: table;
  .box {
    display: table-cell;
    vertical-align: middle;
  }
}
```

### 16.  如何实现小于12px的字体效果

> `transform:scale()`这个属性只可以缩放可以定义宽高的元素，而行内元素是没有宽高的，我们可以加上一个`display:inline-block`;

```text
transform: scale(0.7);
```

`css`的属性，可以缩放大小

### 17. 用纯CSS创建一个三角形的原理是什么

```css
/* 把上、左、右三条边隐藏掉（颜色设为 transparent） */
#demo {
  width: 0;
  height: 0;
  border-width: 20px;
  border-style: solid;
  border-color: transparent transparent red transparent;
}
```

### 18. rgba() 和 opacity 的透明效果有什么不同

- `opacity` 作用于元素以及元素内的所有内容（包括文字）的透明度
- `rgba()` 只作用于元素自身的颜色或其背景色，子元素不会继承透明效果

### 19. 一个高度自适应的div，里面有两个div，一个高度100px，希望另一个填满剩下的高度

- 方案1：
  - `.sub { height: calc(100%-100px); }`
- 方案2：
  - `.container { position:relative; }`
  - `.sub { position: absolute; top: 100px; bottom: 0; }`
- 方案3：
  - `.container { display:flex; flex-direction:column; }`
  - `.sub { flex:1; }`

### 20. 常见兼容性问题？

- `png24`位的图片在iE6浏览器上出现背景，解决方案是做成`PNG8`
- 浏览器默认的`margin`和`padding`不同。解决方案是加一个全局的`*{margin:0;padding:0;}`来统一,，但是全局效率很低，一般是如下这样解决：

```css
body,ul,li,ol,dl,dt,dd,form,input,h1,h2,h3,h4,h5,h6,p{
margin:0;
padding:0;
}
```

### 21. 外边距重叠

* 类型一

  两个元素垂直堆叠时，外边距会取较大的外边距，而不是二者外边距之和

* 类型二

  元素嵌套的情况下，加入没有内边距或边框来分割二者的外边距，它们的上下外边距会重叠

* 类型三

  空元素在没有内边距和边框的情况下，上下外边距直接接触，也会重叠，如果折叠的外边距又遇到了其他元素的外边距，还会继续重叠，所以再多的空段落也只会占用一小块空间，因为它们的外边距会折叠

解决外边距重叠

* 父子元素

  ​	加上边框或者内边距

  ​	父元素加上after伪类

  ​	父元素开启BFC

### 22. BFC

块级格式化上下文

* 特点

  可以包含浮动元素

  不会被浮动元素覆盖

  父子元素外边距不会重叠

* 开启方法

  父元素设置浮动

  父元素设置绝对定位

  父元素overflow设置为不是visible的值

  display: inline-block或者table-cell

### 23. 图片格式

    图片的格式：
        jpeg(jpg)
        - 支持的颜色比较丰富，不支持透明效果，不支持动图
        - 一般用来显示照片
        gif
        - 支持的颜色比较少，支持简单透明，支持动图
        - 颜色单一的图片，动图
        png
        - 支持的颜色丰富，支持复杂透明，不支持动图
        - 颜色丰富，复杂透明图片（专为网页而生）
        webp
        - 这种格式是谷歌新推出的专门用来表示网页中的图片的一种格式
        - 它具备其他图片格式的所有优点，而且文件还特别的小
        - 缺点：兼容性不好
        base64
        - 将图片使用base64编码，这样可以将图片转换为字符，通过字符的形式来引入图片
        - 一般都是一些需要和网页一起加载的图片才会使用base64
    	svg
        - 矢量图，放大不失真
### 24. css画三角形

css画等边三角形

```css
.triangle {
    width: 0;
    height: 0;
    border: 100px solid transparent;
    border-bottom: 173px solid red;
}
```

border常态是等腰梯形，数值指边框的宽度，当内容为空时，边框就是四个等腰三角形

### 25. css3新增属性

* 边框属性：border-radius box-shawdow
* 伪类：nth-child(n)
* transform
* transition
* annimation
* text-shaodow