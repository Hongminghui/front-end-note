[toc]

<div STYLE="page-break-after: always;"></div>

## Css3

文档：https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance





### 三十. Css技巧归纳



### 三十一. emmet语法

[更多查看](https://www.jianshu.com/p/9352a0411fcb)

```html
* 重复， $ 数字列表， {} 标签文本
ul>li{列表$}*10 在10后面按tab键，效果是生成十个列表
<ul>
      <li>列表1</li>
      <li>列表2</li>
      <li>列表3</li>
      <li>列表4</li>
      <li>列表5</li>
      <li>列表6</li>
      <li>列表7</li>
      <li>列表8</li>
      <li>列表9</li>
      <li>列表10</li>
</ul>
缩写：ul>li.item$@3*5
 <ul>
     <li class="item3"></li>
     <li class="item4"></li>
     <li class="item5"></li>
     <li class="item6"></li>
     <li class="item7"></li>
</ul>
缩写：ul>li.item$@-*5
<ul>
    <li class="item5"></li>
    <li class="item4"></li>
    <li class="item3"></li>
    <li class="item2"></li>
    <li class="item1"></li>
</ul>

.box$*3
<div class="box1">aaaa</div>
<div class="box2">aaaa</div>
<div class="box3">aaaa</div>
```



### 三十二.  less

[文档](https://less.bootcss.com/)

#### 1. less安装

```
less 安装
桌面 cmd > npm install -g less
webstorm file > settings > tools > file Watchers + less
```



![image-20200915172617917](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20200915172617917.png)

Program里填lessc.cmd的路径，但似乎下载之后会自动填好，这里的lessc就是安装的路径

![image-20200915173559961](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20200915173559961.png)

#### 2. 语法

##### **1) 变量**

```
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
```

编译为：

```
#header {
  width: 10px;
  height: 20px;
}
```

##### **2) 导入**

```
//import用来将其他的less引入到当前的less
//可以通过import来将其他的less引入到当前的less中
@import "syntax2.less";
```

##### **3) 计算**

```less
.box1{
    // 在less中所有的数值都可以直接进行运算
    // + - * /
    width: 100px + 100px;
    height: 100px/2;
    background-color: #bfa;
}
```

##### **4) 注释**

```
// less中的单行注释，注释中的内容不会被解析到css中

/* css中的注释，内容会被解析到css文件中*/
```

##### **5) 嵌套**

```css
.box1{
    .box2{
        color: red;
    }

    >.box3{
        color: red;

        &:hover{
            color: blue;
        }
    }

    //为box1设置一个hover
    //& 就表示外层的父元素
    &:hover{
        color: orange;
    }

    div &{
        width: 100px;
    }
}
```

编译为：

```css
.box1 .box2 {
  color: red;
}
.box1 > .box3 {
  color: red;
}
.box1 > .box3:hover {
  color: blue;
}
.box1:hover {
  color: orange;
}
div .box1 {
  width: 100px;
}
```

##### **6) 继承**

```css
.p1{
    width: 100px;
    height: 200px;
}

//:extend() 对当前选择器扩展指定选择器的样式（选择器分组）
.p2:extend(.p1){
    color: red;
}
```

编译为：

```css
.p1,
.p2 {
  width: 100px;
  height: 200px;
}
.p2 {
  color: red;
}
```

##### 7) 混入

```less
.p3{
    //直接对指定的样式进行引用，这里就相当于将p1的样式在这里进行了复制，比继承性能差一点
    //mixin 混合
    .p1();
}
```

编译为：

```css
.p3 {
  width: 100px;
  height: 200px;
}
```

```less
// 使用类选择器时可以在选择器后边添加一个括号，这时我们实际上就创建了一个mixins
// 不是选择器，不会出现在编译的css中，就是为了给别人用的
.p4(){
    width: 100px;
    height: 100px;
}

.p5{
    .p4;
}
```

```css
.p5 {
  width: 100px;
  height: 100px;
}
```

##### 8) 函数

```less
.test(@w:100px,@h:200px,@bg-color:red){
    width: @w;
    height: @h;
    border: 1px solid @bg-color;
}

div{
    //调用混合函数，按顺序传递参数
    // .test(200px,300px,#bfa);
    .test(300px);
    // .test(@bg-color:red, @h:100px, @w:300px);
}
```

编译为：

```
div {
  width: 300px;
  height: 200px;
  border: 1px solid red;
}
```

##### 9) 颜色

```less
span{
    color: average(red,blue);
}

body:hover{
    background-color: darken(#bfa,50%);
}
```

### 三十三. Css小项目

[css项目](https://zhuanlan.zhihu.com/p/55950771)

```html
<!--引入图标-->
<link rel="icon" href="favicon.ico">
```

#### 1. 复仇者联盟

```css
<style>
    html {
        perspective: 800px
    }

    .cube {
        width: 200px;
        height: 200px;
        /* background-color: #bfa; */
        margin: 100px auto;
        /* 设置3d变形效果 */
        transform-style: preserve-3d;
        /* transform: rotateX(45deg) rotateZ(45deg); */
        animation: rotate 20s infinite linear;
        /* transform:rotateY(45deg) scaleZ(2); */
    }

    .cube>div {
        width: 200px;
        height: 200px;
        /* 为元素设置透明效果 */
        opacity: 0.7;
        position: absolute;
    }

    img {
        vertical-align: top;
    }

    .box1 {
        transform: rotateY(90deg) translateZ(100px);
    }

    .box2 {
        transform: rotateY(-90deg) translateZ(100px);
    }

    .box3 {
        transform: rotateX(90deg) translateZ(100px);
    }

    .box4 {
        transform: rotateX(-90deg) translateZ(100px);
    }

    .box5 {
        transform:rotateY(180deg) translateZ(100px);
    }

    .box6 {
        transform: rotateY(0deg) translateZ(100px);
    }

    @keyframes rotate {
        form{
            transform:rotateX(0) rotateZ(0)
        }

        to{
            transform:rotateX(1turn) rotateZ(1turn)
        }
    }
</style>
```

```html
<!-- 创建一个外部的容器 -->
<div class="cube">
    <!-- 引入图片 -->
    <div class="box1">
        <img src="./img/14/1.jpg">
    </div>

    <div class="box2">
        <img src="./img/14/2.jpg">
    </div>

    <div class="box3">
        <img src="./img/14/3.jpg">
    </div>

    <div class="box4">
        <img src="./img/14/4.jpg">
    </div>

    <div class="box5">
        <img src="./img/14/5.jpg">
    </div>

    <div class="box6">
        <img src="./img/14/6.jpg">
    </div>
</div>
```

#### 2. 奔跑的少年

![bg2](C:\documents\编程\Html5+Css3\code\exercise\img\12\bg2.png)

```css
.box1{
    border: 1px black solid;
    width: 256px;
    height: 256px;
    /*处于父元素水平中间，即网页中间*/
    margin: 0 auto;
    /*大图放在小框里，默认只会显示一部分，所以正好显示雪碧图的第一张，图片*/
    background-image: url('./img/12/bg2.png');
    animation: run 1s steps(6) infinite;
}

/* 创建关键帧 */
@keyframes run {
    from{
        background-position: 0 0;
    }

    to{
        /*从初始位置，跑到最右边，然后开始重复*/
        background-position: -1536px 0;
    }
}
```

```html
<div class="box1"></div>
```

#### 3. 月球绕着地球转

```
问题：企图让月球自己转，但是旋转的中心一直控制不了，不知道是啥问题，所以选择让整个图形一起转
```

```css
<style>
    .outer {
      position: relative;
      width: 600px;
      height: 600px;
      border-radius: 50%;
      border: 2px solid silver;
      left: 200px;
      top: 30px;
      animation: rotate 2s  linear forwards infinite;
    }

    .moon {
      width: 50px;
      height: 50px;
      transform: rotateZ(0);
      border-radius: 50%;
      background-color: coral;
      position: absolute;
      left: 275px;
      top: -25px;
      transition: 10s;

    }
    .earth {
      width: 100px;
      height: 100px;
      border-radius: 100px;
      background-color: blueviolet;
      position: absolute;
      left: calc(50% - 50px);
      top: calc(50% - 50px);
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
```

```
<div class="outer">
  <div class="moon"></div>
  <div class="earth"></div>
</div>
```

## bootstrap

bootstrap4.5.0 基本模板，其他内容都替换h1标签

```html
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.0/dist/css/bootstrap.min.css" integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">

    <title>Hello, world!</title>
  </head>
  <body>
    <h1>Hello, world!</h1>

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.0/dist/js/bootstrap.min.js" integrity="sha384-OgVRvuATP1z7JjHLkuOU7Xw704+h835Lr+6QL9UvYjZE3Ipu6Tp75j7Bh/kR0JKI" crossorigin="anonymous"></script>
  </body>
</html>
```



