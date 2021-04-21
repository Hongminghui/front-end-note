网页布局都有哪种？一般都用什么布局？ - 阿里巴巴淘系技术的回答 - 知乎 https://www.zhihu.com/question/21775016/answer/1358336033

## Html5

### 1. 特殊符号

```html
 &实体的名字; &nbsp; 空格， &gt; 大于号， &lt; 小于号， &copy; 版权符号
```

### 2. meta

```
主要用于设置网页中的一些元数据，元数据不是给用户看
    charset 指定网页的字符集
    name 指定的数据的名称
    content 指定的数据的内容
    keywords 表示网站的关键字，可以同时指定多个关键字，关键字间使用,隔开
```

```html
 <meta name="Keywords" content="网上购物,网上商城,手机,笔记本,
            电脑,MP3,CD,VCD,DV,相机,数码,配件,手表,存储卡,京东"/>
            <meta name="keywords" content="网购,网上购物,在线购物,网购网站,网购商城,购物网站,网购中			心,购物中心,卓越,亚马逊,卓越亚马逊,亚马逊中国,joyo,amazon">
```

```
description 用于指定网站的描述， 网站的描述会显示在搜索引擎的搜索的结果中，title标签的内容会作为搜索结果的超链接上的文字显示     
```

```html 
 <meta name="description" content="京东JD.COM-专业的综合网上购物商城,
            数万个品牌优质商品.便捷、诚信的服务，为您提供愉悦的网上购物体验!"/>
         
```

### 3. 标题标签

```
title: 标题显示的内容
h1 ~ h6 一共有六级标题
从h1~h6重要性递减，h1最重要，h6最不重要
标题标签都是块元素（在页面中独占一行的元素称为块元素（block element））
行内元素（inline element）行内元素主要用来包裹文字
hgroup标签用来为标题分组，可以将一组相关的标题同时放入到hgroup
```

### 4. 布局标签

```
header 表示网页的头部
main 表示网页的主体部分(一个页面中只会有一个main)
footer 表示网页的底部
nav 表示网页中的导航
aside 和主体相关的其他内容（侧边栏）
article 表示一个独立的文章
section 表示一个独立的区块，上边的标签都不能表示时使用section

div 没有语义，就用来表示一个区块，目前来讲div还是我们主要的布局元素
span 行内元素，没有任何的语义，一般用于在网页中选中文字
```

### 5. 列表

```
列表  有序列表，使用ol标签来创建无序列表 使用li表示列表项
   	 无序列表，使用ul标签来创建无序列表  使用li表示列表项
   	 定义列表，使用dl标签来创建一个定义列表 使用dt来表示定义的内容
            使用dd来对内容进行解释说明
```

### 6. 超链接

```
超链接：可以让我们从一个页面跳转到其他页面，或者是当前页面的其他的位置
        使用 a 标签来定义超链接，超链接是也是一个行内元素，在a标签中可以嵌套除它自身外的任何元素
        属性：href 指定跳转的目标路径
                - 值可以是一个外部网站的地址
                - 也可以写一个内部页面的地址
               
              target，用来指定超链接打开的位置
                可选值：_self 默认值 在当前页面中打开超链接
                       _blank 在一个新的要么中打开超链接
```

```html
<!-- 给元素设置id，href="#id",就会跳转到相应位置 -->
<a href="#p3">去第三个自然段</a>
<p id="p3">在我的后园，可以看见墙外有两株树，一株是枣树，还有一株也是枣树。 这上面的夜的天空，奇怪而高，</p>

<!-- 在开发中可以将#作为超链接的路径的展位符使用 -->
<a href="#">这是一个新的超链接</a>

<!-- 可以使用 javascript:; 来作为href的属性，此时点击这个超链接什么也不会发生 -->
<a href="javascript:;">这是一个新的超链接</a>

<!--可以直接将超链接的href属性设置为#，这样点击超链接以后,页面不会发生跳转，而是转到当前页面的顶部的位置-->
<!-- 似乎开发中不会这样用 -->
<a href="#bottom">去底部</a>
<a id="bottom" href="#">回到顶部</a>
```

### 7. 图片

```
图片：使用img标签来引入外部图片，img标签是一个自结束标签,img这种元素属于替换元素（块和行内元素之间，具有两种元素	  的特点）
    属性：
        src 属性指定的是外部图片的路径（路径规则和超链接是一样的）
        alt 图片的描述，这个描述默认情况下不会显示，有些浏览器会图片无法加载时显示
        	搜索引擎会根据alt中的内容来识别图片，如果不写alt属性则图片不会被搜索引擎所收录
        width 图片的宽度 (单位是像素)
        height 图片的高度
        宽度和高度中如果只修改了一个，则另一个会等比例缩放
        注意： 一般情况在pc端，不建议修改图片的大小，需要多大的图片就裁多大
        但是在移动端，经常需要对图片进行缩放（大图缩小）

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
```

### 8. 音视频

```html
<!-- 
    audio 标签用来向页面中引入一个外部的音频文件的
    音视频文件引入时，默认情况下不允许用户自己控制播放停止

    属性：
        controls 是否允许用户控制播放
        autoplay 音频文件是否自动播放
        - 如果设置了autoplay 则音乐在打开页面时会自动播放
        但是目前来讲大部分浏览器都不会自动对音乐进行播放 
        loop 音乐是否循环播放  
-->
<audio src="./source/audio.mp3" controls autoplay loop></audio>	

<!-- 除了通过src来指定外部文件的路径以外，还可以通过source来指定文件的路径 -->
<audio controls>
     <!-- 对不起，您的浏览器不支持播放音频！请升级浏览器！ -->
     <source src="./source/audio.mp3">
     <source src="./source/audio.ogg">
     <embed src="./source/audio.mp3" type="audio/mp3" width="300" height="100">
 </audio>    

      
   
```

### 9. 内联框架

```html
<!-- 网页中引入另一个网页，并设置这个网页的宽高，边框 -->
<iframe src="https://www.qq.com" width="1000" height="200" frameborder="2"></iframe>
<h1>Hello</h1>
```

### 10. 表格

```
在table中使用tr表示表格中的一行，有几个tr就有几行,在tr中使用td表示一个单元格，有几个td就有几个单元格

td的属性：rowspan 纵向合并，colspan 横向合并

border-collapse: collapse; 设置边框的合并
```

```html
<table border="1" width='50%' align="center">
    <!-- 在table中使用tr表示表格中的一行，有几个tr就有几行 -->
    <tr>
        <!-- 在tr中使用td表示一个单元格，有几个td就有几个单元格 -->
        <td>A1</td>
        <td>B1</td>
        <td>C1</td>
        <td>D1</td>
    </tr>
    <tr>
        <td>A2</td>
        <td>B2</td>
        <td>C2</td>
        <!-- rowspan 纵向的合并单元格 -->
        <td rowspan="2">D2</td>
    </tr>
    <tr>
        <td>A3</td>
        <td>B3</td>
        <td>C3</td>
    </tr>
    <tr>
        <td>A4</td>
        <td>B4</td>
        <!-- 
            colspan 横向的合并单元格
         -->
        <td colspan="2">C4</td>
    </tr>
</table>
```

```
可以将一个表格分成三个部分：

头部 thead	主体 tbody	底部 tfoot	th 表示头部的单元格, 用tr也可以
```

```html
<table border="1" width='50%' align="center">
  
    <thead>
        <tr>
            <th>日期</th>
            <th>收入</th>
            <th>支出</th>
            <th>合计</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2000.1.1</td>
            <td>500</td>
            <td>200</td>
            <td>300</td>
        </tr>
        <tr>
            <td>2000.1.1</td>
            <td>500</td>
            <td>200</td>
            <td>300</td>
        </tr>
        <tr>
            <td>2000.1.1</td>
            <td>500</td>
            <td>200</td>
            <td>300</td>
        </tr>
        <tr>
            <td>2000.1.1</td>
            <td>500</td>
            <td>200</td>
            <td>300</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td></td>
            <td></td>
            <td>合计</td>
            <td>300</td>
        </tr>
    </tfoot>

</table>
```

```
实现隔行换色表格
```

```html
<table>
  <tr>
    <td>学号</td>
    <td>姓名</td>
    <td>性别</td>
    <td>年龄</td>
    <td>地址</td>
  </tr>
  <tr>
    <td>1</td>
    <td>孙悟空</td>
    <td>男</td>
    <td>18</td>
    <td>花果山</td>
  </tr>
  <tr>
    <td>2</td>
    <td>猪八戒</td>
    <td>男</td>
    <td>28</td>
    <td>高老庄</td>
  </tr>
  <tr>
    <td>3</td>
    <td>沙和尚</td>
    <td>男</td>
    <td>38</td>
    <td>流沙河</td>
  </tr>
  <tr>
    <td>4</td>
    <td>唐僧</td>
    <td>男</td>
    <td>16</td>
    <td>女儿国</td>
  </tr>
  <tr>
    <td>1</td>
    <td>孙悟空</td>
    <td>男</td>
    <td>18</td>
    <td>花果山</td>
  </tr>
  <tr>
    <td>2</td>
    <td>猪八戒</td>
    <td>男</td>
    <td>28</td>
    <td>高老庄</td>
  </tr>
  <tr>
    <td>3</td>
    <td>沙和尚</td>
    <td>男</td>
    <td>38</td>
    <td>流沙河</td>
  </tr>
  <tr>
    <td>4</td>
    <td>唐僧</td>
    <td>男</td>
    <td>16</td>
    <td>女儿国</td>
  </tr>
  <tr>
    <td>1</td>
    <td>孙悟空</td>
    <td>男</td>
    <td>18</td>
    <td>花果山</td>
  </tr>
</table>
```

```css
<style>
    table {
      width: 50%;
      border: 1px solid black;
      margin: 0 auto;

      /* border-spacing: 指定边框之间的距离 */
      /* border-spacing: 0px; */

      /* border-collapse: collapse; 设置边框的合并 */
      border-collapse: collapse;
    }

    td {
      border: 1px solid black;
      height: 100px;
      /* 默认情况下元素在td中是垂直居中的 可以通过 vertical-align 来修改*/
      vertical-align: middle;
      text-align: center;
    }

    /*
      如果表格中没有使用tbody而是直接使用tr，那么浏览器会自动创建一个tbody，并且将tr全都放到tbody中,tr不是		  table的子元素
     */
    tbody > tr:nth-child(odd) {
      background-color: #bfa;
    }

 
  </style>
```

### 11. 表单

```html
<!-- form的属性，action 表单要提交的服务器的地址-->
<!--1. label中for的值与input中id值相同，可实现点击label中的字，也能选中，每一个input都应该搭配一个label-->
<label for="male">
  <input type="radio" id="male" value="男" v-model="sex">男
</label>

</form>
```

**HTML 表单用于搜集不同类型的用户输入。**

#### 1）表单属性

* actions

```
action 属性定义在提交表单时执行的动作。向服务器提交表单的通常做法是使用提交按钮。通常，表单会被提交到 web 服务器上的网页。
<form action="action_page.php">
如果省略 action 属性，则 action 会被设置为当前页面。
```

* method

```
method 属性规定在提交表单时所用的 HTTP 方法（GET 或 POST）：

<form action="action_page.php" method="GET">
或：
<form action="action_page.php" method="POST">

GET（默认方法）：

如果表单提交是被动的（比如搜索引擎查询），并且没有敏感信息。使用 GET 时，表单数据在页面地址栏中是可见的：

action_page.php?firstname=Mickey&lastname=Mouse
注释：GET 最适合少量数据的提交。浏览器会设定容量限制。

POST：

如果表单正在更新数据，或者包含敏感信息（例如密码）。POST 的安全性更加，因为在页面地址栏中被提交的数据是不可见的。
```

| 属性           | 描述                                                       |
| :------------- | :--------------------------------------------------------- |
| accept-charset | 规定在被提交表单中使用的字符集（默认：页面字符集）。       |
| action         | 规定向何处提交表单的地址（URL）（提交页面）。              |
| autocomplete   | 规定浏览器应该自动完成表单（默认：开启）。                 |
| enctype        | 规定被提交数据的编码（默认：url-encoded）。                |
| method         | 规定在提交表单时所用的 HTTP 方法（默认：GET）。            |
| name           | 规定识别表单的名称（对于 DOM 使用：document.forms.name）。 |
| novalidate     | 规定浏览器不验证表单。                                     |
| target         | 规定 action 属性中地址的目标（默认：_self）。              |



#### 2）表单元素

```
<form> 元素定义 HTML 表单：
表单元素指的是不同类型的 input 元素、复选框、单选按钮、提交按钮等等。
```

##### input 输入框

* 类型

```html
 <!-- 单行输入-->
<input type="text" name="firstname">
<!-- 输入密码-->
<input type="password" name="psw">
<!-- 提交按钮-->
<input type="submit" value="Submit">

<!-- 单选框，通过name规定是同一组-->
<form>
    <input type="radio" name="sex" value="male" checked>Male
    <br>
    <input type="radio" name="sex" value="female">Female
</form> 

<!-- 多选框，通过name规定是同一组-->
<form>
    <input type="checkbox" name="vehicle" value="Bike">I have a bike
    <br>
    <input type="checkbox" name="vehicle" value="Car">I have a car 
</form> 
<!-- 表单还有checked，defaultChecked，disable等属性 -->
<!-- 按钮 -->
<input type="button" onclick="alert('Hello World!')" value="Click Me!">
<!--输入的类型是数字-->
<form>
  Quantity (between 1 and 5):
  <input type="number" name="quantity" min="1" max="5">
</form>

<select name="haha">
    <option value="i">选项一</option>
    <option selected value="ii">选项二</option>
    <option value="iii">选项三</option>
</select>
```

```
html5新增类型

color date datetime datetime-locale mail month number range search tel time url week

注释：老式 web 浏览器不支持的输入类型，会被视为输入类型 text。
```

* 属性

  [更多请查看](https://www.w3school.com.cn/html/html_form_attributes.asp)

```html
<!--value 属性规定输入字段的初始值-->
<input type="text" name="firstname" value="John">

<!--readonly 属性规定输入字段为只读（不能修改）-->
<input type="text" name="firstname" value="John" readonly>

<!--disabled 属性规定输入字段是禁用的。被禁用的元素是不可用和不可点击的，不会被提交。在vue中可以动态绑定值，使某个条件达成时禁用-->
<input type="text" name="firstname" value="John" disabled>

<!--size 属性规定输入字段的尺寸（以字符计）-->
<input type="text" name="firstname" value="John" size="40">

<!--placeholder 属性规定用以描述输入字段预期值的提示（样本值或有关格式的简短描述）。该提示会在用户输入值之前显示在输入字段中。-->
<input type="text" name="fname" placeholder="First name">

<!--required 属性是布尔属性。如果设置，则规定在提交表单之前必须填写输入字段。-->
Username: <input type="text" name="usrname" required>
```



##### 其他

* select 下拉菜单

```html
<select name="cars">
    <!-- selected默认选中-->
    <option value="volvo" selected>Volvo</option>
    <option value="saab">Saab</option>
    <option value="fiat">Fiat</option>
    <option value="audi">Audi</option>
</select>
```

* textarea 多行输入（文本域）

```html
<textarea name="message" rows="10" cols="30">
The cat was playing in the garden.
</textarea>
```

* button 按钮

  button的默认类型是submit

```html
<button type="button" onclick="alert('Hello World!')">Click Me!</button>
有一个属性：disable
```

### 12. title与alt

```
1.alt属性
使用alt属性是为了给那些不能看到你文档中图像的浏览者提供文字说明。这包括那些使用本来就不支持图像显示或者图像显示被关闭的浏览器的用户，视觉障碍的用户和使用屏幕阅读器的用户。替换文字是用来替代图像而不是提供额外说明文字的。alt属性包括替换说明，对于图像和图像热点是必须的。它只能用在img、area和input元素中（包括applet元素）。

2.title属性
title属性为设置该属性的元素提供建议性的信息。使用title属性提供非本质的额外信息。大部分的可视化浏览器在鼠标悬浮在特定元素上时显示title文字为提示信息（tool tip），然而这又由制造商来决定如何渲染title文字。
title属性有一个很好的用途，即为链接添加描述性文字，特别是当连接本身并不是十分清楚的表达了链接的目的。这样就使得访问者知道那些链接将会带他们到什么地方，他们就不会加载一个可能完全不感兴趣的页面。另外一个潜在的应用就是为图像提供额外的说明信息，比如日期或者其他非本质的信息。
title属性可以用在除了base，basefont，head，html，meta，param，script和title之外的所有标签。但是并不是必须的。

```

### 13. fieldset lengend

```html
<form>
  <fieldset>
    <legend>health information</legend>
    height: <input type="text" />
    weight: <input type="text" />
  </fieldset>
</form>
```

### 14. 修改收藏图标

```html
<link rel="icon" href="./images/favicon-2x.ico">
```

