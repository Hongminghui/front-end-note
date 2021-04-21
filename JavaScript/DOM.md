## 第12章 BOM

BOM，Browser Object Model 浏览器对象模型

### 1. window对象

#### 1.1 Global对象

BOM 的核心是 window 对象，表示浏览器的实例。 window 对象在浏览器中有两重身份，一个是ECMAScript 中的 **Global 对象**，另一个就是浏览器窗口的 **JavaScript 接口**。这意味着网页中定义的所有对象、变量和函数都以 window 作为其 Global 对象，都可以访问其上定义的 parseInt() 等全局方法。

通过 var 声明的所有全局变量和函数都会变成 window 对象的属性和方法。（let和const不会）比如：

```js 
var age = 29;
var sayAge = () => alert(this.age);
alert(window.age); // 29
```

JavaScript 中有很多对象都暴露在全局作用域中，比如 location 和 navigator ，因而它们也是 window 对象的属性。

#### 1.2 窗口

top: 顶部窗口

parent: 父窗口

self: 当前窗口

所有现代浏览器都支持 4 个属性：
innerWidth 、 innerHeight 、 outerWidth 和 outerHeight 。 

outerWidth 和 outerHeight 返回浏览器窗口自身的大小（不管是在最外层 window 上使用，还是在窗格 <frame> 中使用）。 innerWidth和 innerHeight 返回浏览器窗口中页面视口的大小（不包含浏览器边框和工具栏）。

document.documentElement.clientWidth 和 document.documentElement.clientHeight返回页面视口的宽度和高度。

#### 1.3 定时器

setTimeout() 用于指定在一定时间后执行某些代码，

setInterval() 用于指定每隔一段时间执行某些代码。

##### 1.3.1 setTimeout()

setTimeout() 方法通常接收两个参数：要执行的代码	等待的毫秒数

```js
// 在 1 秒后显示警告框
setTimeout(() => alert("Hello world!"), 1000);
```

**任务队列**：setTimeout() 的第二个参数只是告诉 JavaScript 引擎在指定的毫秒数过后把任务添加到这个队列。如果队列是空的，则会立即执行该代码。如果队列不是空的，则代码必须等待前面的任务执行完才能执行。

**取消任务**：调用 setTimeout() 时，会返回一个表示该超时排期的数值 ID。这个超时 ID 是被排期执行代码的唯一标识符，可用于取消该任务。要取消等待中的排期任务，可以调用 clearTimeout() 方法并传入超时 ID，如下面的例子所示：

```js
// 设置超时任务
let timeoutId = setTimeout(() => alert("Hello world!"), 1000);
// 取消超时任务
clearTimeout(timeoutId);
```

**注意** 所有超时执行的代码（函数）都会在全局作用域中的一个匿名函数中运行，因此函数中的 this 值在非严格模式下始终指向 window ，而在严格模式下是 undefined 。如果给 setTimeout() 提供了一个箭头函数，那么 this 会保留为定义它时所在的词汇作用域。

**第三个参数开始的参数都是第一个参数（函数）的参数**。

##### 1.3.2 setInterval()

setInterval()方法同样接收两个参数：要执行的代码，和间隔毫秒数

```js
setInterval(() => alert("Hello world!"), 10000);
```

注意 这里的关键点是，第二个参数，也就是间隔时间，指的是向队列添加新任务之前等待的时间。比如，调用 setInterval() 的时间为 01:00:00，间隔时间为 3000 毫秒。这意味着 01:00:03 时，浏览器会把任务添加到执行队列。浏览器不关心这个任务什么时候执行或者执行要花多长时间。因此，到了 01:00:06，它会再向队列中添加一个任务。由此可看出，**执行时间短、非阻塞的回调函数比较适合 setInterval()**。

**取消任务**：setInterval() 方法也会返回一个循环定时 ID

```js
let num = 0, intervalId = null;
let max = 10;
let incrementNumber = function() {
    num++;
    // 如果达到最大值，则取消所有未执行的任务
    if (num == max) {
        clearInterval(intervalId);
        alert("Done");
    }
}
intervalId = setInterval(incrementNumber, 500);
```

用setTimeout()实现定时任务

```js
let num = 0;
let max = 10;
let incrementNumber = function() {
    num++;
    //  如果还没有达到最大值，再设置一个超时任务
    if (num < max) {
    	setTimeout(incrementNumber, 500);
    } else {
    	alert("Done");
    }
}
setTimeout(incrementNumber, 500);
```

setIntervale() 在实践中很少会在生产环境下使用，因为一个任务结束和下一个任务开始之间的时间间隔是无法保证的，有些循环定时任务可能会因此而被跳过。而像前面这个例子中一样使用 setTimeout() 则能确保不会出现这种情况。一般来说，**最好不要使用 setInterval()** 。

#### 1.4 系统对话框

 alert() 、 confirm() 和 prompt()

**警告框**

alert() 只接收一个参数。调用 alert() 时，传入的字符串会显示在一个系统对话框中。对话框只有一个“OK”（确定）按钮。如果传给 alert() 的参数不是一个原始字符串，则会调用这个值的 toString() 方法将其转换为字符串。

警告框（alert）通常用于向用户显示一些他们无法控制的消息，比如报错。用户唯一的选择就是在
看到警告框之后把它关闭。

**确认框**

确认框（confirm）有两个按钮：“Cancel”（取消）和“OK”（确定）。

要知道用户单击了 OK 按钮还是 Cancel 按钮，可以判断 confirm() 方法的返回值： true 表示单击了 OK 按钮， false 表示单击了 Cancel 按钮或者通过单击某一角上的 X 图标关闭了确认框。确认框的典型用法如下所示：

```js
if (confirm("Are you sure?")) {
	alert("I'm so glad you're sure!");
} else {
	alert("I'm sorry to hear you're not sure.");
}
```

**提示框**

提示框prompt()的用途是提示用户输入消息。除了 OK 和 Cancel 按钮，提示框还会显示一个文本框，让用户输入内容。 prompt() 方法接收两个参数：要显示给用户的文本，以及文本框的默认值（可以是空字符串）。

如果用户单击了 OK 按钮，则 prompt() 会返回文本框中的值。如果用户单击了 Cancel 按钮，或者
对话框被关闭，则 prompt() 会返回 null 。下面是一个例子：

```js
let result = prompt("What is your name? ", "");
if (result !== null) {
	alert("Welcome, " + result);
}
```

### 2. Location对象

* 既是 window 的属性，也是 document 的属性。也就是说，window.location 和 document.location 指向同一个对象。
* 最有用的 BOM对象之一，提供了当前窗口中加载文档的信息，以及通常的导航功能。
* location 对象不仅保存着当前加载文档的信息，也保存着把 URL 解析为离散片段后能够通过属性访问的信息。

#### 2.1 location属性

假 设 浏 览 器 当 前 加 载 的 URL 是 http://foouser:barpassword@www.wrox.com:80/WileyCDA/?q=
javascript#contents， location 对象的内容如下表所示。

![image-20201123104918865](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201123104918865.png)

#### 2.2 查询字符串

location 的多数信息都可以通过上面的属性获取。但是 URL 中的查询字符串并不容易使用。虽然location.search 返回了从问号开始直到 URL 末尾的所有内容，但没有办法逐个访问每个查询参数。下面的函数解析了查询字符串，并返回一个以每个查询参数为属性的对象：

```js
let getQueryStringArgs = function() {
// 取得没有开头问号的查询字符串
    let qs = (location.search.length > 0 ? location.search.substring(1) : ""),
    // 保存数据的对象
    args = {};
    // 把每个参数添加到 args 对象
    for (let item of qs.split("&").map(kv => kv.split("="))) {
        let name = decodeURIComponent(item[0]),
        value = decodeURIComponent(item[1]);
        if (name.length) {
            args[name] = value;
        }
    }
    return args;
}
```

这个函数首先删除了查询字符串开头的问号，当然前提是 location.search 必须有内容。解析后的参数将被保存到 args 对象，这个对象以字面量形式创建。接着，先把查询字符串按照 & 分割成数组，每个元素的形式name=value 。 for 循环迭代这个数组，将每一个元素按照 = 分割成数组，这个数组第一项是参数名，第二项是参数值。参数名和参数值在使用 decodeURIComponent() 解码后（这是因为查询字符串通常是被编码后的格式）分别保存在 name 和 value 变量中。最后， name 作为属性而 value作为该性的值被添加到 args 对象。这个函数可以像下面这样使用：

```js
// 假设查询字符串为?q=javascript&num=10
let args = getQueryStringArgs();
alert(args["q"]); // "javascript"
alert(args["num"]); // "10"
```

#### 2.3 操作地址

可以通过修改 location 对象修改浏览器的地址。

```js
// 这行代码会立即启动导航到新 URL 的操作，同时在浏览器历史记录中增加一条记录。
location.assign("http://www.wrox.com");

window.location = "http://www.wrox.com";
// 最常见
location.href = "http://www.wrox.com";
```

修改 location 对象的属性也会修改当前加载的页面。其中， hash 、 search 、 hostname 、 pathname
和 port 属性被设置为新值之后都会修改当前 URL，如下面的例子所示：

```js
// 假设当前 URL 为 http://www.wrox.com/WileyCDA/
// 把 URL 修改为 http://www.wrox.com/WileyCDA/#section1
location.hash = "#section1";
// 把 URL 修改为 http://www.wrox.com/WileyCDA/?q=javascript
location.search = "?q=javascript";
// 把 URL 修改为 http://www.somewhere.com/WileyCDA/
location.hostname = "www.somewhere.com";
// 把 URL 修改为 http://www.somewhere.com/mydir/
location.pathname = "mydir";
// 把 URL 修改为 http://www.somewhere.com:8080/WileyCDA/
location.port = 8080;
```

除了 hash 之外，只要修改 location 的一个属性，就会导致页面重新加载新 URL。

```js
// replace不会增加历史记录
setTimeout(() => location.replace("http://www.wrox.com/"), 1000);
```

最后一个修改地址的方法是 reload() ，它能重新加载当前显示的页面。调用 reload() 而不传参
数，页面会以最有效的方式重新加载。也就是说，如果页面自上次请求以来没有修改过，浏览器可能会
从缓存中加载页面。如果想强制从服务器重新加载，可以像下面这样给 reload() 传个 true ：

```js
location.reload(); // 重新加载，可能是从缓存加载
location.reload(true); // 重新加载，从服务器加载
```

脚本中位于 reload() 调用之后的代码可能执行也可能不执行，这取决于网络延迟和系统资源等因
素。为此，最好把 reload() 作为最后一行代码。

### 3. history对象

#### 3.1 go

go() 方法可以在用户历史记录中沿任何方向导航，可以前进也可以后退。这个方法只接收一个参数，
这个参数可以是一个整数，表示前进或后退多少步。

在旧版本的一些浏览器中， go() 方法的参数也可以是一个字符串，这种情况下浏览器会导航到历
史中包含该字符串的第一个位置。最接近的位置可能涉及后退，也可能涉及前进。如果历史记录中没有
匹配的项，则这个方法什么也不做，

```js
// 后退一页
history.go(-1);
// 前进一页
history.go(1);
// 前进两页
history.go(2);
```

```js
// 导航到最近的 wrox.com 页面
history.go("wrox.com");
// 导航到最近的 nczonline.net 页面
history.go("nczonline.net");
```

go() 有两个简写方法： back() 和 forward() 。顾名思义，这两个方法模拟了浏览器的后退按钮和前进按钮：

```js
// 后退一页
history.back();
// 前进一页
history.forward();
```

history 对象还有一个 length 属性，表示历史记录中有多个条目。

```js
if (history.length == 1){
// 这是用户窗口中的第一个页面
}
```

**注意** 如果页面 URL 发生变化，则会在历史记录中生成一个新条目。对于 2009 年以来发
布的主流浏览器，这包括改变 URL 的散列值（因此，把 location.hash 设置为一个新
值会在这些浏览器的历史记录中增加一条记录）。这个行为常被单页应用程序框架用来模
拟前进和后退，这样做是为了不会因导航而触发页面刷新。

#### 3.1 历史状态管理

现代 Web 应用程序开发中最难的环节之一就是历史记录管理。用户每次点击都会触发页面刷新的时代早已去，“后退”和“前进”按钮对用户来说就代表“帮我切换一个状态”的历史也就随之结束了。

为解决这个问题，首先出现的是 hashchange 事件（第 17 章介绍事件时会讨论）。HTML5 也为history 对象增加了方便的状态管理特性。

hashchange 会在页面 URL 的散列变化时被触发，开发者可以在此时执行某些操作。而状态管理API 则可以让开发者改变浏览器 URL 而不会加载新页面。为此，可以使用 history.pushState() 方法。这个方法接收 3 个参数：一个 state 对象、一个新状态的标题和一个（可选的）相对 URL。例如：

```js
let stateObject = {foo:"bar"};
history.pushState(stateObject, "My title", "baz.html");
```

pushState() 方法执行后，状态信息就会被推到历史记录中，浏览器地址栏也会改变以反映新的相对 URL。除了这些变化之外，即使 location.href 返回的是地址栏中的内容，浏览器页不会向服务器发送请求。第二个参数并未被当前实现所使用，因此既可以传一个空字符串也可以传一个短标题。第一个参数应该包含正确初始化页面状态所必需的信息。为防止滥用，这个状态的对象大小是有限制的，通常在 500KB～1MB 以内。

因为 pushState() 会创建新的历史记录，所以也会相应地启用“后退”按钮。此时单击“后退”
按钮，就会触发 window 对象上的 popstate 事件。 popstate 事件的事件对象有一个 state 属性，其
中包含通过 pushState() 第一个参数传入的 state 对象：

```js
window.addEventListener("popstate", (event) => {
    let state = event.state;
    if (state) { // 第一个页面加载时状态是 null
    	processState(state);
    }
});
```

**注意** 使用 HTML5 状态管理时，要确保通过 pushState() 创建的每个“假”URL 背后都对应着服务器上一个真实的物理 URL。否则，单击“刷新”按钮会导致 404 错误。所有单页应用程序（SPASingle Page Application）框架都必须通过服务器或客户端的某些配置解决这个问题。

## 第14章 DOM

文档对象模型（DOM，Document Object Model）是 HTML 和 XML 文档的编程接口。

![image-20201123160036117](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201123160036117.png)

### 1. 节点层级

在 HTML 中， <body> 元素是 <html> 元素的子元素，而 <html> 元素则是 <body> 元素的父元素。 <head>
元素是 <body> 元素的同胞元素，因为它们有共同的父元素 <html> 。

#### 1.1 Node类型

节点的属性方法

##### 1.1.1 节点关系

所有的关系节点都是只读的。

**childNodes**

每个节点都有一个 childNodes 属性，其中包含一个 NodeList 的实例。 NodeList 是一个类数组
对象，用于存储可以按位置存取的有序节点。

* 类数组对象，有length属性，可以通过中括号访问，非Array的实例。
* 动态， DOM 结构的变化会自动地在 NodeList 中反映出来，而不是第一次访问时所获得内容的快照。

下面的例子展示了如何使用中括号或使用 item() 方法访问 NodeList 中的元素：

```js
let firstChild = someNode.childNodes[0];
let secondChild = someNode.childNodes.item(1);
let count = someNode.childNodes.length;
```

更倾向使用中括号语法访问

```js
// 转换为真正的数组
let arrayOfNodes = Array.from (someNode.childNodes);
```

![image-20201123161047435](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201123161047435.png)

* hasChildNodes() ，这个方法如果返回 true 则说明节点有一个或多个子节点。

* ownerDocument 属性是一个指向代表整个文档的文档节点 的指针。用来迅速访问文档节点

##### 1.1.2 操纵节点

**appendChild() **

用于在 childNodes 列表末尾添加节点。添加新节点会更新相关的关系指针，包括父节点和之前的最后一个子节点。 appendChild() 方法返回新添加的节点，如下所示：

```js
let returnedNode = someNode.appendChild(newNode);
alert(returnedNode == newNode); // true
alert(someNode.lastChild == newNode); // true 
```

如果把文档中已经存在的节点传给 appendChild() ，则这个节点会从之前的位置被转移到新位置。即使 DOM 树通过各种关系指针维系，一个节点也不会在文档中同时出现在两个或更多个地方。因此，如果调用 appendChild() 传入父元素的第一个子节点，则这个节点会成为父元素的最后一个子节点，如下所示：

```js
// 假设 someNode 有多个子节点
let returnedNode = someNode.appendChild(someNode.firstChild);
alert(returnedNode == someNode.firstChild); // false
alert(returnedNode == someNode.lastChild); // true
```

**insertBefore()**

想把节点放到 childNodes 中的特定位置，要插入的节点和参照节点。调用这个方法后，要插入的节点会变成参照节点的 前一个同胞节点，并被返回。如果参照节点是 null，则 insertBefore()与 appendChild()效果相 同，如下面的例子所示：

```js
// 作为最后一个子节点插入
returnedNode = someNode.insertBefore(newNode, null);
alert(newNode == someNode.lastChild); // true
// 作为新的第一个子节点插入
returnedNode = someNode.insertBefore(newNode, someNode.firstChild);
alert(returnedNode == newNode); // true
alert(newNode == someNode.firstChild); // true
// 插入最后一个子节点前面
returnedNode = someNode.insertBefore(newNode, someNode.lastChild);
alert(newNode == someNode.childNodes[someNode.childNodes.length - 2]); // true 

```

**replaceChild()**

接收两个参数：要插入的节点和要替换的节点。要替换的节点会被返回并从文档 树中完全移除，要插入的节点会取而代之。下面看一个例子：

```js
// 替换第一个子节点
let returnedNode = someNode.replaceChild(newNode, someNode.firstChild);
// 替换最后一个子节点
returnedNode = someNode.replaceChild(newNode, someNode.lastChild); 
```

**removeChild()**

移除一个节点，返回移除的节点

```js
// 删除第一个子节点
let formerFirstChild = someNode.removeChild(someNode.firstChild);
// 删除最后一个子节点
let formerLastChild = someNode.removeChild(someNode.lastChild);
```

上面介绍的 4 个方法都用于操纵某个节点的子元素，也就是说使用它们之前必须先取得父节点（使 用前面介绍的 parentNode 属性）。并非所有节点类型都有子节点，如果在不支持子节点的节点上调用 这些方法，则会导致抛出错误。

**cloneNode()**

会返回与调用它的节点一模一样的节 点。

* cloneNode()方法接收一个布尔值参数，表示是否深复制。
* 在传入 true 参数时，会进行深复制， 即复制节点及其整个子 DOM 树。
* 如果传入 false，则只会复制调用该方法的节点。
* 复制返回的节点属于文档所有，但尚未指定父节点，所以可称为孤儿节点（orphan）。可以通过appendChild()、 insertBefore()或 replaceChild()方法把孤儿节点添加到文档中。

```js
<ul>
 <li>item 1</li>
 <li>item 2</li>
 <li>item 3</li>
</ul>
// 如果myList保存着对这个<ul>元素的引用，则下列代码展示了使用cloneNode()方法的两种方式：
let deepList = myList.cloneNode(true);
alert(deepList.childNodes.length); // 3（IE9 之前的版本）或 7（其他浏览器）
let shallowList = myList.cloneNode(false);
alert(shallowList.childNodes.length); // 0 
```

**注意** cloneNode()方法不会复制添加到 DOM 节点的 JavaScript 属性，比如事件处理程 序。这个方法只复制 HTML 属性，以及可选地复制子节点。除此之外则一概不会复制。 IE 在很长时间内会复制事件处理程序，这是一个 bug，所以推荐在复制前先删除事件处 理程序。

**normalize()**

normalize()方法会检测这个节点的所有后代，从中搜索上述两种 情形。如果发现空文本节点，则将其删除；如果两个同胞节点是相邻的，则将其合并为一个文本节点。

#### 1.2 文档节点

document 是 window 对象的属性，因此是一个全局对象。

##### 1.2.1 属性

**documentElement**属性

```js
let html = document.documentElement; // 取得对<html>的引用
```

**body**属性

```
let body = document.body; // 取得对<body>的引用
```

**title**属性

```js
// 读取文档标题
let originalTitle = document.title;
// 修改文档标题
document.title = "New page title"; 
```

**URL、domain 和 referrer**

其中，URL 包含当前页面的完整 URL（地 址栏中的 URL），domain 包含页面的域名，而 referrer 包含链接到当前页面的那个页面的 URL。如 果当前页面没有来源，则 referrer 属性包含空字符串。所有这些信息都可以在请求的 HTTP 头部信息中获取，只是在 JavaScript 中通过这几个属性暴露出来而已，如下面的例子所示：

```js
// 取得完整的 URL
let url = document.URL;
// 取得域名
let domain = document.domain;
// 取得来源
let referrer = document.referrer; 
```

##### 1.2.2 查找方法

根据id 元素名 name属性 选择器 类名

**getElementById()**  

getElementById()方法接收一个参数，即要获取元素的 ID，如果找到了则返回这个元素，如果 没找到则返回 null。

```js
let div = document.getElementById("myDiv"); // 取得对这个<div>元素的引用，区分大小写
```

如果页面中存在多个具有相同 ID 的元素，则 getElementById()返回在文档中出现的第一个元素。

**getElementsByTagName()**

getElementsByTagName()是另一个常用来获取元素引用的方法。这个方法接收一个参数，即要 获取元素的标签名，返回包含零个或多个元素的 NodeList。在 HTML 文档中，这个方法返回一个 HTMLCollection 对象。考虑到二者都是“实时”列表，HTMLCollection 与 NodeList 是很相似的。

```js
let images = document.getElementsByTagName("img"); 

alert(images.length); // 图片数量
alert(images[0].src); // 第一张图片的 src 属性
alert(images.item(0).src); // 同上
```

HTMLCollection 对象还有一个额外的方法 namedItem()，可通过标签的 name 属性取得某一项 的引用。

```js
<img src="myimage.gif" name="myImage">
// 那么也可以像这样从 images 中取得对这个<img>元素的引用：
let myImage = images.namedItem("myImage"); 
```

**getElementsByName()**

顾名思义，这个 方法会返回具有给定 name 属性的所有元素。getElementsByName()方法最常用于单选按钮，因为同 一字段的单选按钮必须具有相同的 name 属性才能确保把正确的值发送给服务器，比如下面的例子：

```html
   <ul>
    <li>
      <input type="radio" value="red" name="color" id="colorRed">
      <label for="colorRed">Red</label>
    </li>
    <li>
      <input type="radio" value="green" name="color" id="colorGreen">
      <label for="colorGreen">Green</label>
    </li>
    <li>
      <input type="radio" value="blue" name="color" id="colorBlue">
      <label for="colorBlue">Blue</label>
    </li>
  </ul>
```

```js
let radios = document.getElementsByName("color"); 
```

与 getElementsByTagName()一样，getElementsByName()方法也返回 HTMLCollection。不 过在这种情况下，namedItem()方法只会取得第一项（因为所有项的 name 属性都一样）。

```
document 对象上还暴露了几个特殊集合，这些集合也都是 HTMLCollection 的实例。这些集合是
访问文档中公共部分的快捷方式，列举如下。
 document.anchors 包含文档中所有带 name 属性的<a>元素。
 document.applets 包含文档中所有<applet>元素（因为<applet>元素已经不建议使用，所
以这个集合已经废弃）。
 document.forms 包含文档中所有<form>元素（与 document.getElementsByTagName ("form")
返回的结果相同）。
 document.images 包含文档中所有<img>元素（与 document.getElementsByTagName ("img")
返回的结果相同）。
 document.links 包含文档中所有带 href 属性的<a>元素。
这些特殊集合始终存在于 HTMLDocument 对象上，而且与所有 HTMLCollection 对象一样，其内
容也会实时更新以符合当前文档的内容。
```

**querySelector()**方法接收 CSS 选择符参数，返回匹配该模式的第一个后代元素，如果没有匹配项则返回 null。下面是一些例子：

```js
// 取得<body>元素
let body = document.querySelector("body");
// 取得 ID 为"myDiv"的元素
let myDiv = document.querySelector("#myDiv"); 
// 取得类名为"selected"的第一个元素
let selected = document.querySelector(".selected");
// 取得类名为"button"的图片
let img = document.body.querySelector("img.button"); 
```

在 Document 上使用 querySelector()方法时，会从文档元素开始搜索；在 Element 上使用 querySelector()方法时，则只会从当前元素的后代中查询。 用于查询模式的 CSS 选择符可繁可简，依需求而定。如果选择符有语法错误或碰到不支持的选择符， 则 querySelector()方法会抛出错误。

**querySelectorAll()**方法跟 querySelector()一样，也接收一个用于查询的参数，但它会返回 所有匹配的节点，而不止一个。这个方法返回的是一个 NodeList 的静态实例。 

再强调一次，querySelectorAll()返回的 NodeList 实例一个属性和方法都不缺，但它是一 个静态的“快照”，而非“实时”的查询。这样的底层实现避免了使用 NodeList 对象可能造成的性 能问题。 以有效 CSS 选择符调用 querySelectorAll()都会返回 NodeList，无论匹配多少个元素都可以。 如果没有匹配项，则返回空的 NodeList 实例。

```js
// 取得 ID 为"myDiv"的<div>元素中的所有<em>元素
let ems = document.getElementById("myDiv").querySelectorAll("em");
// 取得所有类名中包含"selected"的元素
let selecteds = document.querySelectorAll(".selected");
// 取得所有是<p>元素子元素的<strong>元素
let strongs = document.querySelectorAll("p strong");
返回的 NodeList 对象可以通过 for-of 循环、item()方法或中括号语法取得个别元素。比如：
let strongElements = document.querySelectorAll("p strong"); 
```

**matches()**方法（在规范草案中称为 matchesSelector()）接收一个 CSS 选择符参数，如果元素 匹配则该选择符返回 true，否则返回 false。例如：

```js
if (document.body.matches("body.page1")){ // true } 
```

使用这个方法可以方便地检测某个元素会不会被 querySelector()或 querySelectorAll()方 法返回。

**getElementsByClassName()**

getElementsByClassName()方法接收一个参数，即包含一个或多个类名的字符串，返回类名中 包含相应类的元素的 NodeList。如果提供了多个类名，则顺序无关紧要。下面是几个示例：

```js
// 取得所有类名中包含"username"和"current"元素
// 这两个类名的顺序无关紧要
let allCurrentUsernames = document.getElementsByClassName("username current");
// 取得 ID 为"myDiv"的元素子树中所有包含"selected"类的元素
let selected = document.getElementById("myDiv").getElementsByClassName("selected"); 

```



##### 1.2.3 文档写入

write()、 writeln()、open()和 close()

write()和 writeln()方法都接收一个字符串参数，可以将 这个字符串写入网页中。write()简单地写入文本，而 writeln()还会在字符串末尾追加一个换行符 （\n）。这两个方法可以用来在页面加载期间向页面中动态添加内容，如下所示：

```js
<html>
<head>
 <title>document.write() Example</title>
</head>
<body>
 <p>The current date and time is:
 <script type="text/javascript">
 document.write("<strong>" + (new Date()).toString() + "</strong>");
 </script>
</p>
</body>
</html> 
```

如果是在页面加 载完之后再调用 document.write()，则输出的内容会重写整个页面，如下面的例子所示：

```js
<html>
<head>
 <title>document.write() Example</title>
</head>
<body>
 <p>This is some content that you won't get to see because it will be
 overwritten.</p>
 <script type="text/javascript">
 window.onload = function(){
 document.write("Hello world!");
 };
 </script>
</body>
</html> 
```

这个例子使用了 window.onload 事件处理程序，将调用 document.write()的函数推迟到页面 加载完毕后执行。执行之后，字符串"Hello world!"会重写整个页面内容。 open()和 close()方法分别用于打开和关闭网页输出流。在调用 write()和 writeln()时，这两 个方法都不是必需的。

#### 1.3 元素节点

```js
 nodeType 等于 1；
 nodeName 值为元素的标签名；
 nodeValue 值为 null；
 parentNode 值为 Document 或 Element 对象；
 子节点可以是 Element、Text、Comment、ProcessingInstruction、CDATASection、
EntityReference 类型。
```

可以通过 nodeName 或 tagName 属性来获取元素的标签名。

```js
<div id="myDiv"></div>
// 可以像这样取得这个元素的标签名：
let div = document.getElementById("myDiv");
alert(div.tagName); // "DIV"
alert(div.tagName == div.nodeName); // true 
```

div.tagName 实际上返回的是"DIV"而不是 "div"，推荐先进行大小写转换

```js
if (element.tagName == "div"){ // 不要这样做，可能出错！
 // do something here
}
if (element.tagName.toLowerCase() == "div"){ // 推荐，适用于所有文档
 // 做点什么
} 
```

##### 1.3.1 属性

HTML元素都具有如下属性:

```
 id，元素在文档中的唯一标识符；
 title，包含元素的额外信息，通常以提示条形式展示；
 lang，元素内容的语言代码（很少用）；
 dir，语言的书写方向（"ltr"表示从左到右，"rtl"表示从右到左，同样很少用）；
 className，相当于 class 属性，用于指定元素的 CSS 类（因为 class 是 ECMAScript 关键字，
所以不能直接用这个名字）。
```

这些属性都可读可写

```js
<div id="myDiv" class="bd" title="Body text" lang="en" dir="ltr"></div>
// 这个元素中的所有属性都可以使用下列 JavaScript 代码读取：
let div = document.getElementById("myDiv");
alert(div.id); // "myDiv"
alert(div.className); // "bd"
alert(div.title); // "Body text"
alert(div.lang); // "en"
alert(div.dir); // "ltr"
// 而且，可以使用下列代码修改元素的属性：
div.id = "someOtherId";
div.className = "ft";
div.title = "Some other text";
div.lang = "fr";
div.dir ="rtl"; 
```

##### 1.3.2 操作属性

getAttribute()、setAttribute()和 removeAttribute()

```js
// 属性不存在返回null，不区分大小写
let div = document.getElementById("myDiv");
alert(div.getAttribute("id")); // "myDiv"
alert(div.getAttribute("class")); // "bd"
alert(div.getAttribute("title")); // "Body text"
alert(div.getAttribute("lang")); // "en"
alert(div.getAttribute("dir")); // "ltr" 
```

通过 DOM 对象访问的属性中有两个返回的值跟使用 getAttribute()取得的值不一样。首先是 style 属性，这个属性用于为元素设定 CSS 样式。在使用 getAttribute()访问 style 属性时，返回的 是 CSS 字符串。而在通过 DOM 对象的属性访问时，style 属性返回的是一个（CSSStyleDeclaration） 对象。DOM 对象的 style 属性用于以编程方式读写元素样式，因此不会直接映射为元素中 style 属 性的字符串值。

 第二个属性其实是一类，即事件处理程序（或者事件属性），比如 onclick。在元素上使用事件属 性时（比如 onclick），属性的值是一段 JavaScript 代码。如果使用 getAttribute()访问事件属性， 则返回的是字符串形式的源代码。而通过 DOM 对象的属性访问事件属性时返回的则是一个 JavaScript 函数（未指定该属性则返回 null）。这是因为 onclick 及其他事件属性是可以接受函数作为值的。 **考虑到以上差异，开发者在进行DOM编程时通常会放弃使用getAttribute()而只使用对象属性。 getAttribute()主要用于取得自定义属性的值。**

```js
div.setAttribute("id", "someOtherId");
div.setAttribute("class", "ft");
div.setAttribute("title", "Some other text");
div.setAttribute("lang","fr");
div.setAttribute("dir", "rtl"); 
```

##### 1.3.3 创建元素

```js
let div = document.createElement("div"); 
```

然后再使用 appendChild()、insertBefore()或 replaceChild()。把元素添加到文档树，可以

#### 1.4 文本节点

```js
// 创建并添加到文档树中

let element = document.createElement("div");
element.className = "message";
let textNode = document.createTextNode("Hello world!");
element.appendChild(textNode);
let anotherTextNode = document.createTextNode("Yippee!");
element.appendChild(anotherTextNode);
document.body.appendChild(element);
alert(element.childNodes.length); // 2
// 使用normalize()合并文本节点
element.normalize();
alert(element.childNodes.length); // 1
alert(element.firstChild.nodeValue); // "Hello world!Yippee!" 
```

### 2. DOM编程

#### 2.1 动态脚本

**引入动态脚本的三种方式**

动态脚本就是在页面初始加载时不存在，之后又通过 DOM 包含的脚本。

```js
<script src="foo.js"></script>
```

```js
let script = document.createElement("script");
script.src = "foo.js";
document.body.appendChild(script);

```

```js
function loadScript(url) {
 let script = document.createElement("script");
 script.src = url;
 document.body.appendChild(script);
}

loadScript("client.js"); 
```

```js
<script>
 function sayHi() {
 alert("hi"); }
</script> 

```

### 3. 元素遍历

IE9 之前的版本不会把元素间的空格当成空白节点，而其他浏览器则会。这样就导致了 childNodes 和 firstChild 等属性上的差异。为了弥补这个差异，同时不影响 DOM 规范，W3C 通过新的 Element Traversal 规范定义了一组新属性。

```
 childElementCount，返回子元素数量（不包含文本节点和注释）；
 firstElementChild，指向第一个 Element 类型的子元素（Element 版 firstChild）；
 lastElementChild，指向最后一个 Element 类型的子元素（Element 版 lastChild）；
 previousElementSibling ，指向前一个 Element 类型的同胞元素（ Element 版
previousSibling）；
 nextElementSibling，指向后一个 Element 类型的同胞元素（Element 版 nextSibling）。
```

#### 3.1 遍历子节点

```js
let parentElement = document.getElementById('parent');
let currentChildNode = parentElement.firstChild;
// 没有子元素，firstChild 返回 null，跳过循环
while (currentChildNode) {
 if (currentChildNode.nodeType === 1) {
 // 如果有元素节点，则做相应处理
 processChild(currentChildNode);
 }
 if (currentChildNode === parentElement.lastChild) {
 break;
 }
 currentChildNode = currentChildNode.nextSibling;
}

// 使用 Element Traversal 属性之后，以上代码可以简化如下：
let parentElement = document.getElementById('parent');
let currentChildElement = parentElement.firstElementChild; 

// 没有子元素，firstElementChild 返回 null，跳过循环
while (currentChildElement) {
 // 这就是元素节点，做相应处理
 processChild(currentChildElement);
 if (currentChildElement === parentElement.lastElementChild) {
 break;
 }
 currentChildElement = currentChildElement.nextElementSibling; 
```

#### 3.2 NodeIterator

通过 document.createNodeIterator()方 法创建其实例。这个方法接收以下 4 个参数。

```
 root，作为遍历根节点的节点。
 whatToShow，数值代码，表示应该访问哪些节点。
 filter，NodeFilter 对象或函数，表示是否接收或跳过特定节点。
 entityReferenceExpansion，布尔值，表示是否扩展实体引用。这个参数在 HTML 文档中没有效果，因为实体引用永远不扩展。

whatToShow 参数是一个位掩码，通过应用一个或多个过滤器来指定访问哪些节点。这个参数对应
的常量是在 NodeFilter 类型中定义的。

经验证，只需要三个参数，可能是第四个不需要
```

```
 NodeFilter.SHOW_ALL，所有节点。
 NodeFilter.SHOW_ELEMENT，元素节点。
 NodeFilter.SHOW_ATTRIBUTE，属性节点。由于 DOM 的结构，因此实际上用不上。
 NodeFilter.SHOW_TEXT，文本节点。
 NodeFilter.SHOW_CDATA_SECTION，CData 区块节点。不是在 HTML 页面中使用的。
 NodeFilter.SHOW_ENTITY_REFERENCE，实体引用节点。不是在 HTML 页面中使用的。
 NodeFilter.SHOW_ENTITY，实体节点。不是在 HTML 页面中使用的。
 NodeFilter.SHOW_PROCESSING_INSTRUCTION，处理指令节点。不是在 HTML 页面中使用的。
 NodeFilter.SHOW_COMMENT，注释节点。
 NodeFilter.SHOW_DOCUMENT，文档节点。
 NodeFilter.SHOW_DOCUMENT_TYPE，文档类型节点。
 NodeFilter.SHOW_DOCUMENT_FRAGMENT，文档片段节点。不是在 HTML 页面中使用的。
 NodeFilter.SHOW_NOTATION，记号节点。不是在 HTML 页面中使用的。
这些值除了 NodeFilter.SHOW_ALL 之外，都可以组合使用。比如，可以像下面这样使用按位或
操作组合多个选项：
```

```js
let whatToShow = NodeFilter.SHOW_ELEMENT | NodeFilter.SHOW_TEXT; 
```

createNodeIterator()方法的 filter 参数可以用来指定自定义 NodeFilter 对象，或者一个 作为节点过滤器的函数。NodeFilter 对象只有一个方法 acceptNode()，如果给定节点应该访问就返 回 NodeFilter.FILTER_ACCEPT，否则返回 NodeFilter.FILTER_SKIP。因为 NodeFilter 是一个 抽象类型，所以不可能创建它的实例。只要创建一个包含 acceptNode()的对象，然后把它传给 createNodeIterator()就可以了。以下代码定义了只接收`<p>`元素的节点过滤器对象：

```js
let filter = {
  acceptNode(node) {
    return node.tagName.toLowerCase() == "p" ?
      NodeFilter.FILTER_ACCEPT :
      NodeFilter.FILTER_SKIP;
  }
};
let iterator = document.createNodeIterator(root, NodeFilter.SHOW_ELEMENT, filter, false); 
```

filter 参数还可以是一个函数，与 acceptNode()的形式一样，如下面的例子所示：

```js
let filter = function(node) {
 return node.tagName.toLowerCase() == "p" ?
 NodeFilter.FILTER_ACCEPT :
 NodeFilter.FILTER_SKIP;
};
let iterator = document.createNodeIterator(root, NodeFilter.SHOW_ELEMENT,
 filter, false); 
```

要创建一个简单的遍历所有节点的 NodeIterator，可以使用以下代码：

```js
let iterator = document.createNodeIterator(document, NodeFilter.SHOW_ALL,null, false); 
```

NodeIterator 的两个主要方法是 nextNode()和 previousNode()。nextNode()方法在 DOM 子树中以深度优先方式进前一步，而 previousNode()则是在遍历中后退一步。创建 NodeIterator 对象的时候，会有一个内部指针指向根节点，因此第一次调用 nextNode()返回的是根节点。当遍历到 达 DOM 树最后一个节点时，nextNode()返回 null。previousNode()方法也是类似的。当遍历到达 DOM 树最后一个节点时，调用 previousNode()返回遍历的根节点后，再次调用也会返回 null。 以下面的 HTML 片段为例：

```html
<div id="div1">
 <p><b>Hello</b> world!</p>
 <ul>
 <li>List item 1</li>
 <li>List item 2</li>
 <li>List item 3</li>
 </ul>
</div> 

```

假设想要遍历`<div>`元素内部的所有元素，那么可以使用如下代码：

```js
let div = document.getElementById("div1");
let iterator = document.createNodeIterator(div, NodeFilter.SHOW_ELEMENT,
 null, false); // 或许不需要false
let node = iterator.nextNode();
while (node !== null) {
 console.log(node.tagName); // 输出标签名
 node = iterator.nextNode();
} 
```

如果只想遍历`<li>`元素，可以传入一个过滤器，比如：

```js
let div = document.getElementById("div1");
let filter = function(node) {
 return node.tagName.toLowerCase() == "li" ?
 NodeFilter.FILTER_ACCEPT :
 NodeFilter.FILTER_SKIP;
};
let iterator = document.createNodeIterator(div, NodeFilter.SHOW_ELEMENT,
 filter, false);
let node = iterator.nextNode();
while (node !== null) {
 console.log(node.tagName); // 输出标签名
 node = iterator.nextNode();
} 
```

nextNode()和 previousNode()方法共同维护 NodeIterator 对 DOM 结构的内部指针，因此修 改 DOM 结构也会体现在遍历中。





### 4. HTML5

#### 1. 修改类名

要删除一个类名，以前的写法：

```html
<div class="bd user disabled">...</div> 
```

```js
// 要删除"user"类
let targetClass = "user";
// 把类名拆成数组
let classNames = div.className.split(/\s+/);
// 找到要删除类名的索引
let idx = classNames.indexOf(targetClass);
// 如果有则删除
if (idx > -1) {
 classNames.splice(i,1);
}
// 重新设置类名
div.className = classNames.join(" "); 
```

**classList属性**

HTML5 通过给所有元素增加 classList 属性为这些操作提供了更简单也更安全的实现方式。 classList 是一个新的集合类型 DOMTokenList 的实例。与其他 DOM 集合类型一样，DOMTokenList 也有 length 属性表示自己包含多少项，也可以通过 item()或中括号取得个别的元素。此外， DOMTokenList 还增加了以下方法。

```
 add(value)，向类名列表中添加指定的字符串值 value。如果这个值已经存在，则什么也不做。
 contains(value)，返回布尔值，表示给定的 value 是否存在。
 remove(value)，从类名列表中删除指定的字符串值 value。
 toggle(value)，如果类名列表中已经存在指定的 value，则删除；如果不存在，则添加。
```

现在删除一个类名：

```js
div.classList.remove("user"); 
```

其他操作：

```js
// 删除"disabled"类
div.classList.remove("disabled");
// 添加"current"类
div.classList.add("current"); 
// 切换"user"类
div.classList.toggle("user");
// 检测类名
if (div.classList.contains("bd") && !div.classList.contains("disabled")){
 // 执行操作
)
// 迭代类名
for (let class of div.classList){
 doStuff(class);
} 
```

#### 2. 焦点管理

HTML5 增加了辅助 DOM 焦点管理的功能。首先是 document.activeElement，始终包含当前拥 有焦点的 DOM 元素。页面加载时，可以通过用户输入（按 Tab 键或代码中使用 focus()方法）让某个 元素自动获得焦点。例如：

```js
let button = document.getElementById("myButton"); 
button.focus(); 
console.log(document.activeElement === button); // true 
```

默认情况下，document.activeElement 在页面刚加载完之后会设置为 document.body。而在 页面完全加载之前，document.activeElement 的值为 null。 其次是 document.hasFocus()方法，该方法返回布尔值，表示文档是否拥有焦点： 

```js
let button = document.getElementById("myButton"); button.focus(); console.log(document.hasFocus()); // true 
```

确定文档是否获得了焦点，就可以帮助确定用户是否在操作页面。 第一个方法可以用来查询文档，确定哪个元素拥有焦点，第二个方法可以查询文档是否获得了焦点， 而这对于保证 Web 应用程序的无障碍使用是非常重要的。无障碍 Web 应用程序的一个重要方面就是焦 点管理，而能够确定哪个元素当前拥有焦点（相比于之前的猜测）是一个很大的进步。

#### 3. 其他属性方法

##### 3.1 readyState 属性 

readyState 是 IE4 最早添加到 document 对象上的属性，后来其他浏览器也都依葫芦画瓢地支持 这个属性。最终，HTML5 将这个属性写进了标准。document.readyState 属性有两个可能的值： 

 loading，表示文档正在加载； 

 complete，表示文档加载完成。

实际开发中，最好是把 document.readState 当成一个指示器，以判断文档是否加载完毕。在这 个属性得到广泛支持以前，通常要依赖 onload 事件处理程序设置一个标记，表示文档加载完了。这个 属性的基本用法如下：

```js
if (document.readyState == "complete"){
 // 执行操作
} 
```

##### 3.2 head 字符集

```js
let head = document.head;  // 指向<head>元素

console.log(document.characterSet); // 默认值，"UTF-16"
document.characterSet = "UTF-8"; 

```

##### 3.3 innerHtml

```js
div.innerHTML = "Hello & welcome, <b>\"reader\"!</b>"; 
```

##### 3.4 contain

如果目标节点是被搜索节点的后代，contains()返回 true，否则返回 false。

```js
console.log(document.documentElement.contains(document.body)); // true 
```

##### 3.5 scrollIntoView() 

DOM 规范中没有涉及的一个问题是如何滚动页面中的某个区域。为填充这方面的缺失，不同浏览 器实现了不同的控制滚动的方式。在所有这些专有方法中，HTML5 选择了标准化 scrollIntoView()。 scrollIntoView()方法存在于所有 HTML 元素上，可以滚动浏览器窗口或容器元素以便包含元 素进入视口。

这个方法的参数如下： 

```
 alignToTop 是一个布尔值。
     true：窗口滚动后元素的顶部与视口顶部对齐。
     false：窗口滚动后元素的底部与视口底部对齐。
 scrollIntoViewOptions 是一个选项对象。
     behavior：定义过渡动画，可取的值为"smooth"和"auto"，默认为"auto"。
     block：定义垂直方向的对齐，可取的值为"start"、"center"、"end"和"nearest"，默
    认为 "start"。
     inline：定义水平方向的对齐，可取的值为"start"、"center"、"end"和"nearest"，默
    认为 "nearest"。
 不传参数等同于 alignToTop 为 true。
```

来看几个例子：

```js
// 确保元素可见
document.forms[0].scrollIntoView();
// 同上
document.forms[0].scrollIntoView(true);
document.forms[0].scrollIntoView({block: 'start'});
// 尝试将元素平滑地滚入视口
document.forms[0].scrollIntoView({behavior: 'smooth', block: 'start'}); 
```

 这个方法可以用来在页面上发生某个事件时引起用户关注。把焦点设置到一个元素上也会导致浏览 器将元素滚动到可见位置。

### 5. 样式表

**读写样式**

```js
let myDiv = document.getElementById("myDiv");
// 设置背景颜色
myDiv.style.backgroundColor = "red";
// 修改大小
myDiv.style.width = "100px";
myDiv.style.height = "200px";
// 设置边框
myDiv.style.border = "1px solid black"; 

console.log(myDiv.style.backgroundColor); // "blue"
console.log(myDiv.style.width); // "10px"
console.log(myDiv.style.height); // "25px" 
```

**getComputedStyle()**

这个方法接收两个参数：要取得计算样式的元素和伪元素字符串（如":after"）。如果不需要查 询伪元素，则第二个参数可以传 null。



```js
let myDiv = document.getElementById("myDiv");
let computedStyle = document.defaultView.getComputedStyle(myDiv, null);
console.log(computedStyle.backgroundColor); // "red"
console.log(computedStyle.width); // "100px"
console.log(computedStyle.height); // "200px"
console.log(computedStyle.border); // "1px solid black"（在某些浏览器中）
```

### 6. 元素尺寸

#### 6.1 偏移尺寸

```
 offsetHeight，元素在垂直方向上占用的像素尺寸，包括它的高度、水平滚动条高度（如果可
见）和上、下边框的高度。
 offsetLeft，元素左边框外侧距离包含元素左边框内侧的像素数。
 offsetTop，元素上边框外侧距离包含元素上边框内侧的像素数。
 offsetWidth，元素在水平方向上占用的像素尺寸，包括它的宽度、垂直滚动条宽度（如果可
见）和左、右边框的宽度。
```

其中，offsetLeft 和 offsetTop 是相对于包含元素的，包含元素保存在 offsetParent 属性中。

![image-20201125171332686](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201125171332686.png)

要确定一个元素在页面中的偏移量，可以把它的 offsetLeft 和 offsetTop 属性分别与 offsetParent 的相同属性相加，一直加到根元素。下面是一个例子：

```js
function getElementLeft(element) {
  let actualLeft = element.offsetLeft;
  let current = element.offsetParent;
  while (current !== null) {
    actualLeft += current.offsetLeft;
    current = current.offsetParent;
  }
  return actualLeft;
}
```

**注意** 所有这些偏移尺寸属性都是只读的，每次访问都会重新计算。因此，应该尽量减少 查询它们的次数。比如把查询的值保存在局量中，就可以避免影响性能。

#### 6.2 客户端尺寸

![image-20201126135432773](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201126135432773.png)

这两个属性最常用于确定 浏览器视口尺寸，即检测 document.documentElement 的 clientWidth 和 clientHeight。这两个 属性表示视口（或元素）的尺寸。

**注意** 与偏移尺寸一样，客户端尺寸也是只读的，而且每次访问都会重新计算。*

#### 6.3 滚动尺寸

```
 scrollHeight，没有滚动条出现时，元素内容的总高度。
 scrollLeft，内容区左侧隐藏的像素数，设置这个属性可以改变元素的滚动位置。
 scrollTop，内容区顶部隐藏的像素数，设置这个属性可以改变元素的滚动位置。
 scrollWidth，没有滚动条出现时，元素内容的总宽度。
```

![image-20201126140052420](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201126140052420.png)

下面这个函数检测元素是不是位于顶部，如果不是则把它滚动回顶部：

```js
function scrollToTop(element) {
  if (element.scrollTop != 0) {
 	element.scrollTop = 0;
  }
}
```

### 7. 元素位置

#### 一、clientX、clientY

点击位置距离当前body可视区域的x，y坐标

#### 二、pageX、pageY

对于整个页面来说，包括了被卷去的body部分的长度

#### 三、screenX、screenY

点击位置距离当前电脑屏幕的x，y坐标

#### 四、offsetX、offsetY

相对于带有定位的父盒子的x，y坐标