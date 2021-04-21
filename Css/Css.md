[toc]

<div STYLE="page-break-after: always;"></div>

## Css3

文档：https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance

### 一. 邂逅Css3

```
1. 网页分成三个部分：结构(HTML)  表现(CSS)  行为(JavaScript)

2. CSS 层叠样式表
    网页实际上是一个多层的结构，通过CSS可以分别为网页的每一个层来设置样式,而最终我们能看到只是网页的最上边一层
    CSS用来设置网页中元素的样式  
    
3. 使用CSS来修改元素样式的三种方式
    1）内联样式，行内样式
         在标签内部通过style属性来设置元素的样式
         问题：
             使用内联样式，样式只能对一个标签生效，
             如果希望影响到多个元素必须在每一个元素中都复制一遍
             并且当样式发生变化时，我们必须要一个一个的修改，非常的不方便
             
	2）内部样式表
          将样式编写到head中的style标签里，然后通过CSS的选择器来选中元素并为其设置各种样式
          可以同时为多个标签设置样式，并且修改时只需要修改一处即可全部应用
          内部样式表更加方便对样式进行复用
          问题：
              内部样式表只能对一个网页起作用，它里边的样式不能跨页面进行复用
     3) 外部样式表
    	  可以将CSS样式编写到一个外部的CSS文件中,然后通过link标签来引入外部的CSS文件
            - 外部样式表需要通过link标签进行引入，意味着只要想使用这些样式的网页都可以对其进行
            引用，或者使用@import url('')的方式引入
              使样式可以在不同页面之间进行复用
            - 将样式编写到外部的CSS文件中，可以使用到浏览器的缓存机制，
              从而加快网页的加载速度，提高用户的体验。
```

```css
<style>
	@import url('test.css');
</style>
```



### 二. 选择器

#### 1. 普通选择器

```
元素选择器
作用：选中相同标签的所有元素
例子：p{}  h1{}  div{}

id选择器
作用：选中一个元素
例子：#box{} #red{}  

类选择器
作用：选中一组元素
例子：.box1 {}	.item1 {}	

通配符选择器
作用选中所有元素
例子：* {}
```

#### 2. 复合选择器

```
交集选择器
    作用：选中同时复合多个条件的元素
    语法：选择器1选择器2选择器3选择器n{}
    注意点：交集选择器中如果有元素选择器，必须使用元素选择器开头
    例子： div.red{
                font-size: 30px;
            }
选择器分组（并集选择器）
    作用：同时选择多个选择器对应的元素
    语法：选择器1,选择器2,选择器3,选择器n{}
	例子：#b1,.p1,h1,span,div.red{}     
 
```

#### 3. 关系选择器

似乎不怎么常用，可以用指定id，类的方式替代

```
子元素选择器
    作用：选中指定父元素的指定子元素
    语法：父元素 > 子元素
    例子：div.box > span{
            color: orange;
            }
后代元素选择器：
     作用：选中指定元素内的指定后代元素
     语法：祖先 后代
     例子：div span{
             color: skyblue
         	}
         	
选择下一个兄弟
      语法：前一个 + 下一个
      例子：p + span{
              color: red;
          	}
选择下边所有的兄弟
      语法：兄 ~ 弟
      例子： p ~ span{
              color: red;
         	 }
```

#### 4. 属性选择器

怎么会有这么麻烦的选择器

```html
<style>
    /* 
        [属性名] 选择含有指定属性的元素
        [属性名=属性值] 选择含有指定属性和属性值的元素
        [属性名^=属性值] 选择属性值以指定值开头的元素
        [属性名$=属性值] 选择属性值以指定值结尾的元素
        [属性名*=属性值] 选择属性值中含有某值的元素的元素
     */
    /* p[title]{ */
    /* p[title=abc]{ */
    /* p[title^=abc]{ */
    /* p[title$=abc]{ */
    p[title*=e]{
        color: orange;
    }
</style>
<body>
        <p title="abc">少小离家老大回</p>
        <p title="abcdef">乡音无改鬓毛衰</p>
        <p title="helloabc">儿童相见不相识</p>
        <p>笑问客从何处来</p>
        <p>秋水共长天一色</p>
        <p>落霞与孤鹜齐飞</p>
</body>
```

#### 5. 伪类选择器

```
伪类（不存在的类，特殊的类）
    - 伪类用来描述一个元素的特殊状态
        比如：第一个子元素、被点击的元素、鼠标移入的元素...
    - 伪类一般情况下都是使用:开头
        :first-child 第一个子元素
        :last-child 最后一个子元素
        :nth-child() 选中第n个子元素
            特殊值：
                n 第n个 n的范围0到正无穷
                2n 或 even 表示选中偶数位的元素
                2n+1 或 odd 表示选中奇数位的元素

            - 以上这些伪类都是根据所有的子元素进行排序

        :first-of-type
        :last-of-type
        :nth-of-type()
            - 这几个伪类的功能和上述的类似，不通点是他们是在同类型元素中进行排序

    - :not() 否定伪类
        - 将符合条件的元素从选择器中去除
```

```css
/*选中奇数个*/ 
ul > li:nth-child(2n+1){
    color: red;
 }

/*选中偶数个*/ 
 ul > li:nth-child(even){
 	color: red;
 } 

/*选中除了第三个的其他li*/ 
ul > li:not(:nth-of-type(3)){
            color: yellowgreen;
}

/*选中第一个子元素且是span的元素，如果第一个元素不是span就无效*/ 
ul > span:first-child{
    color: green;
}

/*选中第一个ul中的第一个li*/ 
ul > li:first-of-type{
    color: red;
}
```

#### 6. a标签的伪类

```css
/*  :link 用来表示没访问过的链接（正常的链接） */
a:link{
    color: red;
    
}

/* 
    :visited 用来表示访问过的链接
    由于隐私的原因，所以visited这个伪类只能修改链接的颜色
*/
a:visited{
    color: orange; 
    /* font-size: 50px;   */
}

/* :hover 用来表示鼠标移入的状态 */
 a:hover{
     color: aqua;
     font-size: 50px;
 }

 /*:active 用来表示鼠标点击 */
 a:active{
     color: yellowgreen;  
 }
```

#### 7. 伪元素选择器

```css
/* 
     伪元素，表示页面中一些特殊的并不真实的存在的元素（特殊的位置）
         伪元素使用 :: 开头

         ::first-letter 表示第一个字母
         ::first-line 表示第一行
         ::selection 表示选中的内容
         ::before 元素的开始 
         ::after 元素的最后
             - before 和 after 必须结合content属性来使用
 */
 p::first-letter{
     font-size: 50px;
 }

 p::first-line{
     background-color: yellow; 
 }

 p::selection{
     background-color: greenyellow;
 }

 /* div::before{
     content: 'abc';
     color: red;
 }

 div::after{
     content: 'haha';
     color: blue;
 } */

 div::before{
     content: '『';
  }

 div::after{
     content: '』';
 }
```

#### 8. 样式的继承

```
样式的继承，我们为一个元素设置的样式同时也会应用到它的后代元素上

继承是发生在祖先后后代之间的

继承的设计是为了方便我们的开发，利用继承我们可以将一些通用的样式统一设置到共同的祖先元素上，
这样只需设置一次即可让所有的元素都具有该样式

注意：并不是所有的样式都会被继承：比如 背景相关的，布局相关等的这些样式都不会被继承。
```

#### 9. 选择器的权重

```
样式的冲突
    - 当我们通过不同的选择器，选中相同的元素，并且为相同的样式设置不同的值时，此时就发生了样式的冲突。

发生样式冲突时，应用哪个样式由选择器的权重（优先级）决定

选择器的权重
    内联样式        1,0,0,0
    id选择器        0,1,0,0
    类和伪类选择器   0,0,1,0
    元素选择器       0,0,0,1
    通配选择器       0,0,0,0
    继承的样式       没有优先级

比较优先级时，需要将所有的选择器的优先级进行相加计算，最后优先级越高，则越优先显示
（分组选择器是单独计算的）,
    选择器的累加不会超过其最大的数量级，类选择器在高也不会超过id选择器
    如果优先级计算后相同，此时则优先使用靠下的样式

可以在某一个样式的后边添加 !important ，则此时该样式会获取到最高的优先级，甚至超过内联样式，
    注意：在开发中这个玩意一定要慎用！
```

### 三. 盒模型

#### 1. 块级盒子、内联盒子

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



* 内外部显示类型

```

 css的box模型有一个外部显示类型，来决定盒子是块级还是内联。

同样盒模型还有内部显示类型，它决定了盒子内部元素是如何布局的。默认情况下是按照 正常文档流 布局，也意味着它们和其他块元素以及内联元素一样(如上所述).

但是，我们可以通过使用类似  flex 的 display 属性值来更改内部显示类型。 

块级和内联布局是web上默认的行为 —— 正如上面所述， 它有时候被称为 正常文档流， 因为如果没有其他说明，我们的盒子布局默认是块级或者内联。
```

#### 2. 盒子模型的各个部分

CSS中组成一个块级盒子需要:

- **Content box**: 这个区域是用来显示内容，大小可以通过设置 [`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width) 和 [`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height).
- **Padding box**: 包围在内容区域外部的空白区域； 大小通过 [`padding`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding) 相关属性设置。
- **Border box**: 边框盒包裹内容和内边距。大小通过 [`border`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border) 相关属性设置。
- **Margin box**: 这是最外面的区域，是盒子和其他元素之间的空白区域。大小通过 [`margin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin) 相关属性设置。

如下图：

![image-20200916205316692](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20200916205316692.png)

##### 1）标准盒模型

默认使用

在标准模型中，如果你给盒设置 `width` 和 `height`，实际设置的是 *content box*。 padding 和 border 再加上设置的宽高一起决定整个盒子的大小。margin 不计入实际大小 —— 当然，它会影响盒子在页面所占空间，但是影响的是盒子外部空间。盒子的范围到边框为止 —— 不会延伸到margin。

##### 2）替代（IE）盒模型

盒子的`width`和`height`设置的是 border box。是实际盒子的宽高，包括内容区，内边距和边框

* 将一个盒子设置为替代盒模型

```css
.box { 
  box-sizing: border-box; 
}
```

* 将所有盒子设置为替代盒模型

```css
html {
  box-sizing: border-box;
}
*, *::before, *::after {
  box-sizing: inherit;
}
```

##### 3）边框

可以使用`border`属性一次设置所有四个边框的宽度、颜色和样式。必须设置三个属性

```
border-width	border-style	border-color
```

如果每边各不相同的话，可通过具体的边来设置

```
border-top	border-right	border-bottom	border-left
```

```
border-radius: 50% 边框的一半变成弧线，值是圆弧的半径
可以设四个值，从左上角顺时针转
```

```
border-collapse:collapse，表格边框合并
border-spacing:0，表格边框距离，只在border-collapse:seperate时生效
```



##### 4）外边距

**宽度**

我们可以使用`margin`属性一次控制一个元素的所有边距，或者每边单独使用等价的普通属性控制：

`margin-top`	`margin-bottom`	`margin-left`	`margin-right`

如果是负值则元素会向相反的方向移动

```
四个值：上 右 下 左
三个值：上 左右 下
两个值：上下 左右
一个值：上下左右
```

**外边距折叠**

如果有两个外边距相接的元素，这些外边距将合并为一个外边距，即最大的单个外边距的大小。

顶部段落的页 `margin-bottom`为50px。第二段的`margin-top` 为30px。因为外边距折叠的概念，所以框之间的实际外边距是50px，而不是两个外边距的总和。

```css
// 外边距重叠和高度塌陷的解决方案
.clearfix::before,
.clearfix::after{
    content: '';
    display: table;
    clear: both;
}
```

* 类型一

  两个元素垂直堆叠时，外边距会取较大的外边距，而不是二者外边距之和

* 类型二

  元素嵌套的情况下，加入没有内边距或边框来分割二者的外边距，它们的上下外边距会重叠

* 类型三

  空元素在没有内边距和边框的情况下，上下外边距直接接触，也会重叠，如果折叠的外边距又遇到了其他元素的外边距，还会继续重叠，所以再多的空段落也只会占用一小块空间，因为它们的外边距会折叠

解决外边距重叠

* 父子元素

  	加上边框或者内边距
	
  	父元素加上after伪类
	
  	父元素开启BFC

##### 5）内边距

和外边距差不多

##### 6) 轮廓

```
outline: 对布局没有任何影响，纯粹的视觉效果。
```

##### 7) 其他

```css
/*利用border-box设计多重边框*/
box-shadow 盒子阴影 background-clip: padding-box 背景渗透的区域是padding-box
div{
	width:300px;
	height:100px;
	background-color:yellow;
	/*右偏移 下偏移 模糊距离 （阴影大小）颜色 */
	box-shadow: 10px 10px 5px  #888888;
}

/*利用border-box设计多重边框*/
background: yellowgreen;
box-shadow: 0 0 0 10px #655,
0 0 0 15px deeppink,
0 2px 5px 15px rgba(0,0,0,.6);
```



### 四. 背景

可以只通过background设置所有属性

```css
.box { 
  background: linear-gradient(105deg, rgba(255,255,255,.2) 39%, rgba(51,56,57,1) 96%) center center / 	400px 200px no-repeat, 
  url(big-star.png) center no-repeat, rebeccapurple; 
} 
```

##### 1. 背景颜色 

`background-color`

颜色 rgb rgba

##### 2. 背景图片

`background-image`

```
background-image: url(star.png); 
```

默认情况下，大图不会缩小以适应方框，因此我们只能看到它的一个小角，而小图则是平铺以填充方框。

###### 1）控制平铺 

`background-repeat`

```
属性用于控制图像的平铺行为。可用的值是:

no-repeat — 不重复。
repeat-x —水平重复。
repeat-y —垂直重复。
repeat — 在两个方向重复。
```

###### 2）调整大小 

`background-size`

```
cover —浏览器将使图像足够大，使它完全覆盖了盒子区，同时仍然保持其高宽比。在这种情况下，有些图像可能会跳出盒子外
contain — 浏览器将使图像的大小适合盒子内。在这种情况下，如果图像的长宽比与盒子的长宽比不同，则可能在图像的任何一边或顶部和底部出现间隙。
background-size: cover  background-size: contain
background-size: 100% 30px; 宽度占满，高度占30px,然后平铺。
```

###### 3）背景图像定位

`background-position`属性允许您选择背景图像显示在其应用到的盒子中的位置。它使用的坐标系中，框的左上角是(0,0)，框沿着水平(x)和垂直(y)轴定位。

**注意：**默认的背景位置值是(0,0)。

最常见的背景位置值有两个单独的值——一个水平值后面跟着一个垂直值。

```
 background-position: top center; 
 background-position: 20px 10%; 
 background-position: top 20px;
 background-position: top 20px right 10px; 
```



###### 4）线性渐变

**渐变是图片**

```
渐变的第一个参数：角度
可选值：
1. to top, right, bottom, left; to top left, top right等
2. 45deg，0deg相当于垂直向上，角度值表示方向顺时针旋转，比如45度相当于斜向上，起点在左下角
默认方向自上而下 to bottom
```

```css
background-image: linear-gradient(red,yellow,#bfa,orange);

/*两个数值之间发生渐变*/
background-image: linear-gradient(red 50px,yellow 100px, green 120px, orange 200px);

/*向右，从红到黄，50px为一个基本单元，重复铺满*/
background-image: repeating-linear-gradient(to right ,red, yellow 50px);

/*切掉右下角15px*/
linear-gradient(-45deg, transparent 15px, #58a 0);

/*条纹渐变，两种颜色的条纹各占15px组成一个基本单元，当两个数值相同时会骤变，不会渐变*/
background: linear-gradient(#fb3 50%, #58a 50%);
background-size: 100% 30px;

/*条纹渐变，第一种颜色10px,第二种颜色20px*/
background: linear-gradient(#fb3 30%, #58a 30%);
background-size: 100% 30px;

/*条纹渐变，第一种颜色10px,第二种颜色20px，第二个数值零会自动变成和前面相同的数字，即30%*/
background: linear-gradient(#fb3 30%, #58a 0);
background-size: 100% 30px;

/*三种颜色的条纹渐变*/
background: linear-gradient(#fb3 33.3%,
      #58a 0, #58a 66.6%, yellowgreen 0);
background-size: 100% 45px;

/*重复斜向渐变，0-15px一个颜色，15px-30px一个颜色，然后重复*/
background: repeating-linear-gradient(60deg,
#fb3, #fb3 15px, #58a 0, #58a 30px)

/*桌布效果，每个30px的方格开头部分有一半横条纹，一半竖条纹，剩下的是背景色白色*/
background-color: white;
background-image: linear-gradient(90deg,
rgba(200,0,0,.5) 50%, transparent 0),
linear-gradient(
rgba(200,0,0,.5) 50%, transparent 0);
background-size: 30px 30px;

/*网格效果，每30px, 30px的方格行和列的开头都有一条1px的白线*/
background-color: #5588aa;
background-image: linear-gradient(white 1px, transparent 0),
linear-gradient(90deg, white 1px, transparent 0);
background-size: 30px 30px;

/*坐标纸效果，75px的方格对应更粗的条纹，15px的方格对应更细的条纹*/
div {
      width: 600px;
      height: 600px;
      background-color: #58a;
      background-image:
        linear-gradient(white 2px, transparent 0),
        linear-gradient(90deg, white 2px, transparent 0),
        linear-gradient(hsla(0,0%,100%,.3) 1px,
        transparent 0),
        linear-gradient(90deg, hsla(0,0%,100%,.3) 1px,
        transparent 0);
      background-size: 75px 75px, 75px 75px,
      15px 15px, 15px 15px;

    }
```

* 径向渐变

```

radial-gradient(大小 at 位置, 颜色 位置 ,颜色 位置 ,颜色 位置)
大小：
circle 圆形
ellipse 椭圆
closest-side 近边	
closest-corner 近角
farthest-side 远边
farthest-corner 远角

位置：
top right left center bottom

background-image: radial-gradient(farthest-corner at 100px 100px, red , #bfa)
```

### 五. 字体

#### 1. 图标字体

似乎一些ui库也会提供这个

```
图标字体（iconfont）
在网页中经常需要使用一些图标，可以通过图片来引入图标，但是图片大小本身比较大，并且非常的不灵活
所以在使用图标时，我们还可以将图标直接设置为字体，这样我们就可以通过使用字体的形式来使用图标

fontawesome 使用步骤
    1.下载 https://fontawesome.com/
    2.解压
    3.将css和webfonts移动到项目中
    4.将all.css引入到网页中
    5.使用图标字体
        - 直接通过类名来使用图标字体
            class="fas fa-bell"
            class="fab fa-accessible-icon"
```

```html
<link rel="stylesheet" href="./fa/css/all.css">

<i class="fas fa-bell" style="font-size:80px; color:red;"></i>
<i class="fas fa-bell-slash"></i>
<i class="fab fa-accessible-icon"></i>
<i class="fas fa-otter" style="font-size: 160px; color:green;"></i>
```

#### 2. 行高

```

行高（line height）
行高指的是文字占有的实际高度，行高可以直接指定一个大小（px em）

也可以直接为行高设置一个整数，如果是一个整数的话，行高将会是字体的指定的倍数

行高经常还用来设置文字的行间距，行间距 = 行高 - 字体大小

字体框
字体框就是字体存在的格子，设置font-size实际上就是在设置字体框的高度

行高会在字体框的上下平均分配

```

```css
/* 可以将行高设置为和高度一样的值，使单行文字在一个元素中垂直居中 */
line-height: 200px;
```

#### 3. 简写

```
font: 字体大小/行高 字体族
font: bold italic 50px/2  微软雅黑, 'Times New Roman', Times, serif;
```

#### 4. 文本样式

```
text-align 文本的水平对齐
可选值：left 左侧对齐	right 右对齐	center 居中对齐	justify 两端对齐
        
vertical-align 设置元素垂直对齐的方式
可选值：baseline 默认值 基线对齐	top 顶部对齐	bottom 底部对齐	middle 居中对齐

text-decoration 设置文本修饰
可选值：none 什么都没有	underline 下划线	line-through 删除线	overline 上划线

white-space 设置网页如何处理空白
可选值：normal 正常	nowrap 不换行	pre 保留空白
```

```css
/*超出文本不换行，隐藏，以省略号形式展示*/
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;
```

```
opacity: 1，不透明度1
```

### 六. 浮动

#### 1. 介绍浮动

```
通过浮动float可以使一个元素向其父元素的左侧或右侧移动
   
可选值：
   none 默认值 ，元素不浮动
   left 元素向左浮动
   right 元素向右浮动

注意，元素设置浮动以后，水平布局的等式便不需要强制成立
元素设置浮动以后，会完全从文档流中脱离，不再占用文档流的位置，
所以元素下边的还在文档流中的元素会自动向上移动

浮动的特点：
    1、浮动元素会完全脱离文档流，不再占据文档流中的位置
    2、设置浮动以后元素会向父元素的左侧或右侧移动，
    3、浮动元素默认不会从父元素中移出
    4、浮动元素向左或向右移动时，不会超过它前边的其他浮动元素
    5、如果浮动元素的上边是一个没有浮动的块元素，则浮动元素无法上移
    6、浮动元素不会超过它上边的浮动的兄弟元素，最多最多就是和它一样高
	7、浮动元素不会盖住文字，文字会自动环绕在浮动元素的周围，所以我们可以利用浮动来设置文字环绕图片的效果

简单总结：
浮动目前来讲它的主要作用就是让页面中的元素可以水平排列，
通过浮动可以制作一些水平方向的布局    
```

```
脱离文档流的特点：
块元素：
    1、块元素不在独占页面的一行
    2、脱离文档流以后，块元素的宽度和高度默认都被内容撑开

行内元素：
	行内元素脱离文档流以后会变成块元素，特点和块元素一样

脱离文档流以后，不需要再区分块和行内了
```

#### 2. BFC

```
BFC(Block Formatting Context) 块级格式化环境
    - BFC是一个CSS中的一个隐含的属性，可以为一个元素开启BFC
        开启BFC该元素会变成一个独立的布局区域
    - 元素开启BFC后的特点：
        1.开启BFC的元素不会被浮动元素所覆盖
        2.开启BFC的元素子元素和父元素外边距不会重叠
        3.开启BFC的元素可以包含浮动的子元素

    - 可以通过一些特殊方式来开启元素的BFC：
        1、设置元素的浮动（不推荐）
        2、将元素设置为行内块元素（不推荐） 
        3、将元素的overflow设置为一个非visible的值
            - 常用的方式 为元素设置 overflow:hidden 开启其BFC 以使其可以包含浮动元素
```

#### 3. 高度塌陷

```
高度塌陷的问题：
    在浮动布局中，父元素的高度默认是被子元素撑开的，
    当子元素浮动后，其会完全脱离文档流，子元素从文档流中脱离
    将会无法撑起父元素的高度，导致父元素的高度丢失

    父元素高度丢失以后，其下的元素会自动上移，导致页面的布局混乱

    所以高度塌陷是浮动布局中比较常见的一个问题，这个问题我们必须要进行处理！

clear
    - 作用：清除浮动元素对当前元素所产生的影响
    - 可选值：
    left 清除左侧浮动元素对当前元素的影响
    right 清除右侧浮动元素对当前元素的影响
    both 清除两侧中最大影响的那侧

    原理：
    设置清除浮动以后，浏览器会自动为元素添加一个上外边距，
    以使其位置不受其他元素的影响
```

```
/* clearfix 这个样式可以同时解决高度塌陷和外边距重叠的问题，
当你在遇到这些问题时，直接使用clearfix这个类即可 */
.clearfix::before,
.clearfix::after{
    content: '';
    display: table;
    clear: both;
}
```

### 七. 定位

```
定位是一种更加高级的布局手段，通过定位可以将元素摆放到页面的任意位置，使用position属性来设置定位
可选值：
    static 默认值，元素是静止的没有开启定位
    relative 开启元素的相对定位
    absolute 开启元素的绝对定位
    fixed 开启元素的固定定位
    sticky 开启元素的粘滞定位
```

#### 1. 相对定位

```
相对定位	positon: relative
当元素的position属性值设置为relative时则开启了元素的相对定位
相对定位的特点：
        1.元素开启相对定位以后，如果不设置偏移量元素不会发生任何的变化
        2.相对定位是参照于元素在文档流中的位置进行定位的
        3.相对定位会提升元素的层级
        4.相对定位不会使元素脱离文档流
        5.相对定位不会改变元素的性质块还是块，行内还是行内
偏移量：top bottom left right 都是相对于自身的位置偏移
```

#### 2. 绝对定位

```

当元素的position属性值设置为absolute时，则开启了元素的绝对定位
绝对定位的特点：
    1.开启绝对定位后，如果不设置偏移量元素的位置不会发生变化
    2.开启绝对定位后，元素会从文档流中脱离
    3.绝对定位会改变元素的性质，行内变成块，块的宽高被内容撑开
    4.绝对定位会使元素提升一个层级
    5.绝对定位元素是相对于其包含块进行定位的

包含块( containing block )
正常情况下，包含块就是离当前元素最近的祖先块元素

绝对定位的包含块:
包含块就是离它最近的开启了定位的祖先元素，如果所有的祖先元素都没有开启定位则根元素就是它的包含块

html（根元素、初始包含块）
```

#### 3. 固定定位

```
将元素的position属性设置为fixed则开启了元素的固定定位，固定定位也是一种绝对定位，所以固定定位的大部分特点都和绝对定位一样，唯一不同的是固定定位永远参照于浏览器的视口进行定位，固定定位的元素不会随网页的滚动条滚动
```

#### 4. 粘滞定位

```
当元素的position属性设置为sticky时则开启了元素的粘滞定位，粘滞定位和相对定位的特点基本一致，不同的是粘滞定位可以在元素到达某个位置时将其固定
```

```
position: sticky;
top: 10px;
```

#### 5. 元素层级

```
对于开启了定位元素，可以通过z-index属性来指定元素的层级
z-index需要一个整数作为参数，值越大元素的层级越高，元素的层级越高越优先显示

如果元素的层级一样，则优先显示靠下的元素

祖先的元素的层级再高也不会盖住后代元素
```

### 八. 动画效果

```
transiton: 一个属性变化时，不是瞬变，慢慢变
例子：正常left=0，hover时left=100，加上transiton: 2s linear left

transform: translateX() rotateY() scale()

annimation: rotate 2s linear
@keyframe rotate {
	from {}
	to {}
}
```



#### 1. 过渡 transition

```
过渡（transition）
- 通过过渡可以指定一个属性发生变化时的切换方式
- 通过过渡可以创建一些非常好的效果，提升用户的体验
例如：初始左外边距为零，鼠标悬浮时做外边距为600px，如果不用transiton就会突然从0到600px，如果使用transiton，可以有一些移动的效果
1. transition-property: 指定要执行过渡的属性  
    多个属性间使用,隔开 
    如果所有属性都需要过渡，则使用all关键字
    不可过渡属性：color background-color border
    大部分属性都支持过渡效果，注意过渡时必须是从一个有效数值向另外一个有效数值进行过渡
    transition-property: height , width;
    transition-property: all;

2. transition-duration: 指定过渡效果的持续时间
    时间单位：s 和 ms  1s = 1000ms
    transition-duration: 2s;

3. transition-timing-function: 过渡的时序函数
    指定过渡的执行的方式  
    可选值：
    ease 默认值，慢速开始，先加速，再减速
    linear 匀速运动
    ease-in 加速运动
    ease-out 减速运动
    ease-in-out 先加速 后减速
    cubic-bezier() 来指定时序函数
    https://cubic-bezier.com
    steps() 分步执行过渡效果
        可以设置一个第二个值：
        end ， 在时间结束时执行过渡(默认值)
        start ， 在时间开始时执行过渡
    
    transition-timing-function: cubic-bezier(.24,.95,.82,-0.88); 
    transition-timing-function: steps(2, start); 
    
4. transition-delay: 过渡效果的延迟，等待一段时间后在执行过渡  
   transition-delay: 2s; 
    
5. transition 可以同时设置过渡相关的所有属性，只有一个要求，如果要写延迟，则两个时间中第一个是持续时间，第二个是	    		  延迟
    transition:2s margin-left 1s cubic-bezier(.24,.95,.82,-0.88);
```

```html
<div class="box1">
  <div class="box2"></div>
</div>
```

```css
// 必须有触发原因，比如鼠标悬浮
.box1 {
      width: 800px;
      height: 600px;
      background-color: slateblue;
    }
    
    .box2 {
      width: 100px;
      height: 100px;
      background-color: yellow;
      margin-top: 100px;
      transition: margin-left 2s linear;
    }

    .box1:hover .box2{
      margin-left: 700px;
    }
```

#### 2. 动画 animation

```
动画和过渡类似，都是可以实现一些动态的效果，不同的是过渡需要在某个属性发生变化时才会触发,动画可以自动触发动态效果
```

```css
 /* 1. 关键帧
		设置动画效果，必须先要设置一个关键帧，关键帧设置了动画执行每一个步骤*/
@keyframes test {
    /* from表示动画的开始位置 也可以使用 0% */
    from{
        margin-left: 0;
        background-color: orange;
    } 

    /* to动画的结束位置 也可以使用100%*/
    to{
        background-color: red;
        margin-left: 700px;
    }
}
/*2. 属性
	1.animation-name: 要对当前元素生效的关键帧的名字
    2.animation-duration: 动画的执行时间
    3.animation-delay: 动画的延时
    4.animation-iteration-count 动画执行的次数
    	可选值：次数 infinite(无限执行)
    5.animation-direction: 指定动画运行的方向
    	可选值： normal 默认值  从 from 向 to运行 每次都是这样
   	 		    reverse 从 to 向 from 运行 每次都是这样
    			alternate 从 from 向 to运行 重复执行动画时反向执行
    6. animation-play-state: 设置动画的执行状态
    	可选值：  running 默认值 动画执行
    			paused 动画暂停
    7. animation-fill-mode: 动画的填充模式
    	可选值：  none 默认值 动画执行完毕元素回到原来位置
    			forwards 动画执行完毕元素会停止在动画结束的位置
    			backwards 动画延时等待时，元素就会处于开始位置
    			both 结合了forwards 和 backwards
    8. animation-timing-function: ease-in-out
    9. 简写与过渡相同，如果有两个时间，延迟在执行后面就行
*/
```


```html
<style>
    *{
        margin: 0;
        padding: 0;
    }

    .box1{
        width: 800px;
        height: 800px;
        background-color: silver;
        overflow: hidden;
    }

    .box1 div{
        width: 100px;
        height: 100px;
        margin-bottom: 100px;
        margin-left: 10px;  
    }

    .box2{
        background-color: #bfa;
        /*2. 动画执行的关键帧是test，一次动画执行时间2s，执行2次
          重复动画时，方向逆转，即第一次从from到to，第二次to到from，第三次from到to...*/
        animation: test 2s 2 1s alternate;
    }

    .box1:hover div{
        /* 3. 鼠标悬浮在box1上时，动画停止*/
        animation-play-state: paused;
    }
 
/*1. 设置动画效果，必须先要设置一个关键帧，关键帧设置了动画执行每一个步骤*/
    @keyframes test {
        /* from表示动画的开始位置 也可以使用 0% */
        from{
            margin-left: 0;
            background-color: orange;
        } 

        /* to动画的结束位置 也可以使用100%*/
        to{
            background-color: red;
            margin-left: 700px;
        }
    }
</style>
```

#### 3. 变形 transform

```
变形就是指通过CSS来改变元素的形状或位置，变形不会影响到页面的布局
transform 用来设置元素的变形效果

/*变形的原点 默认值 center, 元素左上角（0，0），可用于设置旋转，缩放等*/
transform-origin:20px 20px;
```

##### 1) 平移

```
平移：
方向：x右y上，z指向人眼
    translateX() 沿着x轴方向平移
    translateY() 沿着y轴方向平移
    translateZ() 沿着z轴方向平移
    - 平移元素，百分比是相对于自身计算的
```

```css
// 和transiton搭配使用，设置过渡属性为all
.box1 {
      position: absolute;
      top: 100px;
      width: 100px;
      height: 200px;
      background-color: yellow;
      transition: all 0.5s linear;
    }

    .box1:hover {
      transform: translateY(-20%);
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
    }
```

`z轴平移`

```
z轴平移，调整元素在z轴的位置，正常情况就是调整元素和人眼之间的距离，距离越大，元素离人越近
z轴平移属于立体效果（近大远小），默认情况下网页是不支持透视，如果需要看见效果，必须要设置网页的视距
```

```css

html{
    /* 1. 设置当前网页的视距为800px，人眼距离网页的距离 */
    perspective: 800px;
}

body{
    border: 1px red solid;
    background-color: rgb(241, 241, 241);
}
.box1{
    width: 200px;
    height: 200px;
    background-color: #bfa;
    margin: 200px auto;
    /*移动过程在2s内完成*/
    transition:2s;
}

body:hover .box1{
    /*鼠标悬浮在body上时，box1向人眼方向移动800px，效果：元素变大
    transform: translateZ(800px);
}

```

##### 2) 旋转

**旋转之后，元素的坐标轴也会转**

```css
 通过旋转可以使元素沿着x y 或 z旋转指定的角度(x右y上，z指向人眼)，方向顺时针
     rotateX()
     rotateY()
     rotateZ()
/*转0.25圈*/
transform: rotateZ(.25turn);
/*转180度*/
transform: rotateY(180deg) translateZ(400px);
```

```css
html{
    /* 1. 设置当前网页的视距为800px，人眼距离网页的距离 */
    perspective: 800px;
}

body{
    border: 1px red solid;
    background-color: rgb(241, 241, 241);
}
.box1{
    width: 320px;
    height: 320px;
    background-color: #bfa;
    margin: 200px auto;
	/*移动过程在2s内完成*/
    transition:2s;
}

body:hover .box1{  
    /* transform: rotateY(180deg) translateZ(400px);  先旋转后平移*/  
    transform: rotateY(180deg);
    /* 是否显示元素的背面 */
    backface-visibility: hidden;
}

```

```html
<div class="box1">
	2333333333
</div>
```

##### 3) 缩放

```css
对元素进行缩放的函数：
    scaleX() 水平方向缩放
    scaleY() 垂直方向缩放
    scale() 双方向的缩放
    /*放大1.2倍*/
    transform:scale(1.2);            
```

```css
html{
    perspective:800px;    
}
.box1{
    width: 100px;
    height: 100px;
    background-color: #bfa;
    transition:2s;
    margin: 100px auto;


     /*变形的原点 默认值 center
     transform-origin:20px 20px;  */
}

.box1:hover{
    transform:scale(2)
}

```

```html
<div class="box1"></div>
```

### 九. 弹性盒flex

```
flex(弹性盒、伸缩盒)

是CSS中的又一种布局手段，它主要用来代替浮动来完成页面的布局,flex可以使元素具有弹性，让元素可以跟随页面的大小	  的改变而改变
    - 弹性容器
        - 要使用弹性盒，必须先将一个元素设置为弹性容器
        - 我们通过 display 来设置弹性容器
            display:flex  设置为块级弹性容器
            display:inline-flex 设置为行内的弹性容器

    - 弹性元素
        - 弹性容器的子元素是弹性元素（弹性项）
        - 弹性元素可以同时是弹性容器
```

#### 1. 弹性盒

```
1. flex-direction 指定容器中弹性元素的排列方式
     可选值：
         row 默认值，弹性元素在容器中水平排列（左向右）
             - 主轴 自左向右
         row-reverse 弹性元素在容器中反向水平排列（右向左）
             - 主轴 自右向左
         column 弹性元素纵向排列（自上向下）
         column-reverse 弹性元素方向纵向排列（自下向上）

     主轴：
         弹性元素的排列方向称为主轴
     侧轴：
         与主轴垂直方向的称为侧轴
         
2. flex-wrap: 设置弹性元素是否在弹性容器中自动换行
        可选值：
        nowrap 默认值，元素不会自动换行
        wrap 元素沿着辅轴方向自动换行
        wrap-reverse 元素沿着辅轴反方向换行
        
3. justify-content
        - 如何分配主轴上的空白空间（主轴上的元素如何排列）
        - 可选值：
        flex-start 元素沿着主轴起边排列
        flex-end 元素沿着主轴终边排列
        center 元素居中排列
        space-around 空白分布到元素两侧
        space-between 空白均匀分布到元素间
        space-evenly 空白分布到元素的单侧
4. align-items
	定义在辅轴上的对齐方式
  	align-items: flex-start | flex-end | center | baseline | stretch;
```

#### 2. 弹性元素

```
弹性元素的属性：
1. flex-grow 指定弹性元素的伸展的系数
    - 当父元素有多余空间的时，子元素如何伸展
    - 父元素的剩余空间，会按照比例进行分配
2. flex-shrink 指定弹性元素的收缩系数
	- 当父元素中的空间不足以容纳所有的子元素时，如果对子元素进行收缩，系数较为复杂
3. flex-basis 指定的是元素在主轴上的基础长度
    如果主轴是 横向的 则 该值指定的就是元素的宽度
    如果主轴是 纵向的 则 该值指定的是就是元素的高度
    - 默认值是 auto，表示参考元素自身的高度或宽度
    - 如果传递了一个具体的数值，则以该值为准
4. order 指元素的顺序，order: 2, 这个弹性元素成为弹性盒的第二个元素
```

```
flex 可以设置弹性元素所有的三个样式
    flex 增长 缩减 基础;
        initial "flex: 0 1 auto"
        auto  "flex: 1 1 auto"
        none "flex: 0 0 auto" 弹性元素没有弹性
```

```css
li:nth-child(1){
    flex-grow: 0;
    flex-shrink: 1;
}

li:nth-child(2){
    background-color: pink;
    /* flex-grow: 2; */
    flex-shrink: 2;
}

li:nth-child(3){
    background-color: orange;
    /* flex-grow: 3; */
    flex-shrink: 3;
}
```

### 十. 移动端

像素

```
css像素，物理像素（由显示器决定，不会因为代码而改变）
手机单位面积能容纳更多的css像素（px)，即分辨率更高
```

长度

详解：[链接](https://www.cnblogs.com/lidongfeng/p/7243650.html)

```
1、px：绝对单位，页面按精确像素展示

2、em：相对单位，基准点为父节点字体的大小，如果自身定义了font-size按自身来计算（浏览器默认字体是16px），整个页面内1em不是一个固定的值。

3、rem：相对单位，可理解为”root em”, 相对根节点html的字体大小来计算，CSS3新加属性，chrome/firefox/IE9+支持。// 是截止目前用的最多也是最流行的

rem在移动端应用可参考淘宝的页面http://m.taobao.com (html的font-size通过动态计算获取)

4、vw、vh、vmin、vmax 主要用于页面视口大小布局，相对于rem;v*在页面布局上更加方便简单
   大势所趋
        vw：viewpoint width，视窗宽度，1vw等于视窗宽度的1%。
        vh：viewpoint height，视窗高度，1vh等于视窗高度的1%。
        vmin：vw和vh中较小的那个。
        vmax：vw和vh中较大的那个。
```

### 十一. 媒体查询

##### 1. 针对特定媒体

```
媒体种类：all print screen speech(语音合成器等音频渲染设备)
```

```css
/*有三种写法*/
<link rel="stylesheet" href="text.css" media="screen">
<style type="text/css" media="screen"></style>
<style>
    @media screen {
    	body {background-color: #8ca0ff}
    }
</style>
```

##### 2. 媒体特性

```
媒体特性描述符，都有最大最小的取值，例如min-width,max-width
width, height, device-width, device-height
aspect-ratio 媒体特性宽高之比
device-aspect-ratio 媒体特性device-width与device-height之比
color
resolution 分辨率
orientation 取值：portrait | landscape portrait(纵向，高大于宽) 
```

##### 3. 响应式设计

```CSS
/*小于400px时，应用此处的样式*/
@media (max-width: 400px) {
    
      img {
        clip-path: circle(50%);
      }
    }
@media (min-width: 401px) and (max-width: 700px) {
      img {
        clip-path: circle(50%);
      }
    }
@media (min-width: 700px) {
      img {
        clip-path: circle(50%);
      }
    }
```

```css
/*宽度大于高度时应用此样式*/
@media (orientation: landscape) {
	
}
```

##### 4. 分页媒体

```css
1. 定义页面框尺寸
@page {size: 7.5in 10in; margin: 0.5in} /*宽高7.5英寸，10英寸*/
@page {size: landscape} /*水平方向打印*/
@page {size: A4}
@page {margin: 3.5in} 
```

```css
2. 页面类型
/*特定div水平打印，其他地方纵向打印*/
@page normal {size: portrait}
body {page: normal;}

@page rotate {size: landscape}
div {page: rotate;}
```

```css
3. 伪类装饰
@page :first
@page :left
@page :right
```

```css
4. 分页
/*h1标签前后都分页*/
h1 {page-break-after: always;} 
h1 {page-break-before: always;} 

/*h1出现在左页*/
h1 {page-break-after: left;} 

/*h3标签后面避免分页*/
h3 {page-break-after: avoid;} 
```



### 十二. 栅格布局

#### 1. 栅格容器

```
1. display: inline-grid / grid
2. 浮动元素不会打乱栅格容器，即栅格不会移到浮动元素下方，块级容器会
3. 栅格容器的外边距与其后代的外边距不会发生折叠
4. 栅格元素的子元素是栅格元素
5. 相关术语：栅格元素，栅格轨道，栅格流，栅格线，栅格单元，栅格区域，图见：CSS权威指南661页
```

#### 2.手动划分栅格

##### 1）grid-temlate-rows

```
属性一：grid-template-rows, grid-template-columns
适用于：栅格容器
初始值：none
可取值：em px % rem fr minmax(a,b) cal() max-content min-content fit-content() repeat()

fr：容器宽 - 确定长度宽，再进行分配的份数
minmax(a,b): a最小尺寸，b最大尺寸 值可取：上述+auto
max-content：宽度尽量大，尽量让文本完整地出现在同一行
min-content: 宽度尽量小，只保证最长的单词出现在同一行
fit-content(argument): 取min-content, max-content, argument中的中间值

repeat(10, 5em): 5em宽的轨道，重复十次
repeat(4, [start] 23m 1fr 1fr [end]): 第二个参数是一个单元，这个单元重复4次
repeat(auto-fill, 5em): 一直重复，只要不溢出栅格容器即可，同时出现时，先分配固定尺寸的轨道
```

```html
<div>
    <section class="grid">
    <div></div>
    <div></div>
    <div></div>
    </section>
</div>
```

```css
.grid {
    display: grid;
    height: 200px;
    border: 2px solid black;
    grid-template-columns: 200px 50% 100px;
}
.grid > div {
    border: 1px solid red;
}
```

![](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201112214709157.png)

##### 2）grid-template-areas

```
名称任取,"." "..." 表示空白区域
初始值：none
```

```css
.grid {
      display: grid;
      height: 200px;
      border: 2px solid black;
      grid-template-areas: "header header header"
                           "leftside content rightside"
                           "leftside footer footer";
                  ;
    }
.grid > div {
    border: 1px solid red;
}

.header {
    grid-area: header;
}
.leftSide {
    grid-area: leftside;
}
.content {
    grid-area: content;
}
.rightSide {
    grid-area: rightside;
}
.footer {
    grid-area: footer;
}
```

```html
<div>
    <section class="grid">
        <div class="header"></div>
        <div class="leftSide"></div>
        <div class="content"></div>
        <div class="rightSide"></div>
        <div class="footer"></div>
    </section>
</div>
```

![image-20201112224227735](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201112224227735.png)



#### 3. 栅格线

```
1. 默认：名称为数字，从左到右，从上到下的默认编号
2. 显式：显式命名栅格线：grid=template-columns: [start col-a] 200px [col-b] 50% [col-c]
3. 隐式：使用grid-template-areas划分栅格时，对于header区域，左上栅格线名为header-start，右下栅格线名为	     header-end
4. 多条栅格线命名重复时，用编号+名称（2 col-A)等方式表示一条栅格线
```

![image-20201112215403700](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201112215403700.png)

#### 4. 添加元素

##### (1）引用栅格线

###### 1）grid-row-start

```
方式一：确定元素的四条边界 grid-row-start/end,grid-column-start/end
初始值：auto 
适用于：栅格元素
取值：数值 名称 span none
```

```css
.one {
    grid-row-start: 1;grid-row-end: 2; 
    grid-column-start: 1;grid-column-end: 2;
}
.two {
    grid-row-start: 1; /*省略结束线，只跨一行*/
    grid-column-start: 1;grid-column-end: 2;
}
.three {
    grid-row-start: 1; grid-row-end: span 2; /*结束线使用span，跨两行*/
    grid-column-start: 1;grid-column-end: 2;
}
.four {
    grid-row-start: span 1;grid-row-end: 2; /*开始线使用span，往前跨一行*/
    grid-column-start: 1;grid-column-end: 2;
}
.five {
    grid-row-start: -1;grid-row-end: -2; /*使用负数，表示右下角的单元格,只限于显式定义的栅格线*/
    grid-column-start: -1;grid-column-end: -2;
}
.six {
    grid-row-start: 1; grid-row-end: span 2 col-A; /*结束线使用span，跨两条col-A*/
    grid-column-start: 1;grid-column-end: 2;
}
.seven {
    grid-row-start: footer;grid-row-end: footer; /*从footer出现在开始位置表示footer-start,结束位置表示footer-end*/
    grid-column-start: 1;grid-column-end: 2;
}
```

###### 2）简写

```css
.one {
    grid-row: R / span R 2; /*R相当于R1,即横从R1开始，跨两条R*/
    grid-column: 1 / 2; 
}
.one {
    grid-row: col-B; /*这两条等同，从col-B到下一条col-B*/
    grid-column: col-B / col-B;
}
.one {
    grid-row: 2; /*这两条等同，应该是跨一行*/
    grid-column: 2 / auto;
}
.one {
    grid-row: footer; /*这两条等同*/
    grid-column: footer-start / footer-end;
}
```

###### 3）隐式栅格

```
我觉得应该不重要，虽然我看懂了。见CSS权威指南p700
同错误处理 p701
```

#### （2） 使用区域

```css
.one {
    grid-area: header; /*使用区域名称*/
}
.one {
    grid-area: 1 / 1 / 2 / 2; /*使用栅格线名称，顺序为逆时针*/
}
```

#### 5. 栅格流

##### 1）grid-auto-flow

```
栅格流：自动放入栅格，围绕显式定位的栅格元素放置
相关术语：行优先，列优先，密集流(见CSS权威指南p710)
grid-auto-flow：[row | column] || [dense row | dense column]
初始值：row
```

```css
.grid {
  display: grid;
  width:600px; height: 200px;
  grid-template-rows: repeat(2, 100px);
  grid-template-columns: repeat(3, 200px);
  border: 2px solid black;
  grid-auto-flow: column;
}
.grid > div {
  border: 1px solid red;
  grid-row: auto; /*自动流，默认也这样*/
  grid-column: auto;
}
```

```html
<div>
  <section class="grid">
    <div class="one">1</div>
    <div class="two">2</div>
    <div class="three">3</div>
    <div class="four">4</div>
    <div class="five">5</div>
  </section>
</div>
```

![image-20201113004137283](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201113004137283.png)

```css
.grid > div {
  border: 1px solid red;
  grid-row: auto;
  grid-column: auto;
  width: 50px; height: 50px; /*显式设定了元素的大小*/
}
```

![image-20201113004507974](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201113004507974.png)

##### 2）grid-auto-rows

```
当元素数量超过栅格区域数量时，决定溢出元素大小，如果没有设置，会尽可能小。
```

```css
.grid {
  display: grid;
  width:200px; height: 200px;
  grid-template-rows: repeat(2, 100px);
  grid-template-columns: repeat(2, 100px);
  border: 2px solid black;
  grid-auto-flow: row;
  grid-auto-rows: 50px; /*第五个元素的高为50px*/
  grid-auto-columns: 50px; /*行优先宽度设置无效，与上一行相同*/
}
.grid > div {
  border: 1px solid red;
  grid-row: auto;
  grid-column: auto;
}
```

```html
<div>
  <section class="grid">
    <div class="one">1</div>
    <div class="two">2</div>
    <div class="three">3</div>
    <div class="four">4</div>
    <div class="five">5</div>
  </section>
</div>
```

![image-20201113010329340](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201113010329340.png)

##### 3）grid简写

```
作用：定义栅格模板 或 设定栅格流并为自动添加的轨道设定尺寸。
```

```css
/*作用一：定义栅格模板*/
.grid {
  display: grid;
  width:200px; height: 200px;
  grid: "header header header"3em /*行宽被打散*/
        "[main-start] . content sidebar [main-stop]" 1fr
        "footer footer footer [page-end]" 5em / 
        2em 3fr 2em;
  border: 2px solid black;
  grid-auto-flow: row;
  grid-auto-rows: 50px;
  grid-auto-columns: 50px;
}
.grid > div {
  border: 1px solid red;
  grid-row: auto;
  grid-column: auto;
}
```

```html
<div>
  <section class="grid">
    <div class="one">1</div>
    <div class="two">2</div>
    <div class="three">3</div>
    <div class="four">4</div>
    <div class="five">5</div>
  </section>
</div>
```

![image-20201113091545709](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201113091545709.png)

```
作用二：设定栅格流，并为自动添加的栅格确定尺寸，即grid-auto-flow,grid-auto-rows/columns的合并写法

试验失败。
```

#### 6. 栏距

```
row-gap, column-gap
简写：gap: 12px 2em;
```

#### 7. 绝对定位

```
1. 普通行为：定位相对于栅格区域，栅格元素划定的四条栅格线内定位
2. 边界存在auto,相对于栅格容器进行定位
```

#### 8. 对齐

| 属性            | 对齐的目标         | 适用于   | 取值                       |
| :-------------- | :----------------- | -------- | :------------------------- |
| justify-self    | 横向的上一个元素   | 栅格元素 | start end center 等        |
| justify-items   | 横向上所有栅格元素 | 栅格容器 |                            |
| justify-content | 横向上的整个栅格   | 栅格容器 | 可以实现分布与对齐两种效果 |
| align-self      |                    |          |                            |
| align-items     |                    |          |                            |
| align-content   |                    |          |                            |

```
1. 栅格元素没有显式设定width和height，内容会自动收缩，不会铺满整个栅格区域
```

#### 9. 分层

```
order: 10;
z-index: 10;
抬升重叠的元素，适用于栅格元素
```

### 十三. 滤镜混合剪裁遮罩

```
1. 滤镜
filter: blur, opacity等
img {filter: blur()}
```

```
2. 剪裁
clip-path
取值：url
inset(10px 10px 10px 10px) 一到四个值，值为四边的偏移量
circle(5em at 50% 50%) 半径加圆心
ellipse(100px 50px at 75% 25%) 长轴短轴加椭圆中心
polygon(0 0，50px 100px) 各边坐标，剪成多边形

剪裁框
取值：margin-box content-box 
```

### 二十九. Scss

#### 1. 变量声明

```css
/*1. 变量声明，有块级作用域,可以用下划线和中划线*/
$nav-color: #F90;
nav {
  $width: 100px;
  width: $width;
  color: $nav-color;
}

//编译后

nav {
  width: 100px;
  color: #F90;
}
```

#### 2. 嵌套规则

```scss
/*2.嵌套规则*/
/*示例一*/
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}
 /* 编译后 */
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }

/*示例二：分组选择器*/
.container {
  h1, h2, h3 {margin-bottom: .8em}
}
/* 编译后 */
.container h1, .container h2, .container h3 { margin-bottom: .8em }

/*示例三*/
nav, aside {
  a {color: blue}
}
/* 编译后 */
nav a, aside a {color: blue}
```

```scss
/*示例四，兄弟选择器*/
article {
  ~ article { border-top: 1px dashed #ccc }
  > section { background: #eee }
  dl > {
    dt { color: #333 }
    dd { color: #555 }
  }
  nav + & { margin-top: 0 }
}

/*示例五,属性嵌套*/
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}
/* 编译后 */
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
```

```scss
/*3.父选择器标识符&*/
/*作用一：伪类*/
article a {
  color: blue;
  &:hover { color: red }
}

article a { color: blue }
article a:hover { color: red }

/*作用二：多个选择器*/
#content aside {
  color: red;
  body.ie & { color: green }
}

/*编译后*/
#content aside {color: red};
body.ie #content aside { color: green }


```

#### 3. 导入Sass

```
/*1.局部文件*/

当通过@import把sass样式分散到多个文件时，你通常只想生成少数几个css文件。那些专门为@import命令而编写的sass文件，并不需要生成对应的独立css文件，这样的sass文件称为局部文件。对此，sass有一个特殊的约定来命名这些文件。

此约定即，sass局部文件的文件名以下划线开头。这样，sass就不会在编译时单独编译这个文件输出css，而只把这个文件用作导入。当你@import一个局部文件时，还可以不写文件的全名，即省略文件名开头的下划线。举例来说，你想导入themes/_night-sky.scss这个局部文件里的变量，你只需在样式表中写@import "themes/night-sky";。
```

```scss
/*2. 默认变量
如果用户在导入你的sass局部文件之前声明了一个$fancybox-width变量，那么你的局部文件中对$fancybox-width赋值400px的操作就无效。如果用户没有做这样的声明，则$fancybox-width将默认为400px。*/
$fancybox-width: 400px !default;
.fancybox {
width: $fancybox-width;
}
```

```scss
/*3.注释*/
// ： 不会写入css
/**/ : 会写入css
```

#### 4. 混合器

```
目的：有一段样式需要反复使用
```

```scss
/*@mixin标识 定义一个混合器*/
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
/*@include标识 使用混合器*/
notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}
/*最终生成*/
.notice {
  background-color: green;
  border: 2px solid #00aa00;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```

```scss
/*混合器传参*/
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}

a {
  @include link-colors(blue, red, green);
}

//Sass最终生成的是：

a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }

/*指定名字传参*/
a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}

// 默认参数
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```

#### 选择器继承

```scss
//.seriousError从.error继承样式
.error a{  //应用到.seriousError a
  color: red;
  font-weight: 100;
}
h1.error { //应用到hl.seriousError
  font-size: 1.2rem;
}
```



### 三十. Css技巧归纳

#### 1. 水平居中

```css

.box3{   
    position: absolute;
    /*这种居中方式，只适用于元素的大小确定的情况*/
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
    margin: auto; */
}

```

```css
.box3{
    position: absolute;

    left: 50%;/*父元素的百分比*/
    top: 50%;
    transform: translateX(-50%) translateY(-50%);
}
```

```
display: table-cell
text-align: center
vertical-align: 
```

```
calc()
```



#### 2. 垂直居中

```
/* 可以将行高设置为和高度一样的值，使单行文字在一个元素中垂直居中 */
line-height: 200px;
```

```css
/* 内部只有一个元素 */
/* 使div中的p标签居中 */
.right div {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}

```

```
position: absolute
top: 
left: 
transform: translate()
```



#### 3. 浮现效果

`鼠标悬浮时，向上移动，加上阴影`

```css
.box4, .box5{
    width: 220px;
    height: 300px;
    background-color: #fff;
    float: left;
    margin: 0 10px;
    transition:all .3s;
}

.box4:hover,.box5:hover{
    transform: translateY(-4px);
    box-shadow: 0 0 10px rgba(0, 0, 0, .3)
}
```

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



