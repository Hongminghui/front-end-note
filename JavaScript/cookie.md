## 第25章 客户端存储

与特定用户相关的信息应该保存在用户的机器上。无论是登录信息、个人偏好，还是其他数据， Web 应用程序提供者都需要有办法把它们保存在客户端。

### 1. cookie

HTTP cookie 通常也叫作 cookie，最初用于在客户端存储会话信息。这个规范要求服务器在响应 HTTP 请求时，通过发送 Set-Cookie HTTP 头部包含会话信息。例如，下面是包含这个头部的一个 HTTP 响应：

```http
HTTP/1.1 200 OK
Content-type: text/html
Set-Cookie: name=value
Other-header: other-header-value
```

这个 HTTP 响应会设置一个名为"name"，值为"value"的 cookie。名和值在发送时都会经过 URL 编码。浏览器会存储这些会话信息，并在之后的每个请求中都会通过 HTTP 头部 cookie 再将它们发回服 务器，比如：

```http
GET /index.jsl HTTP/1.1
Cookie: name=value
Other-header: other-header-value
```

#### 1.1 限制

cookie 是与特定域绑定的。设置 cookie 后，它会与请求一起发送到创建它的域。这个限制能保证 cookie 中存储的信息只对被认可的接收者开放，不被其他域访问。 因为 cookie 存储在客户端机器上，所以为保证它不会被恶意利用，浏览器会施加限制。同时，cookie 也不会占用太多磁盘空间。

```
通常，只要遵守以下大致的限制，就不会在任何浏览器中碰到问题：
 不超过 300 个 cookie；
 每个 cookie 不超过 4096 字节；
 每个域不超过 20 个 cookie；
 每个域不超过 81 920 字节。

每个域能设置的 cookie 总数也是受限的，但不同浏览器的限制不同。例如：
 最新版 IE 和 Edge 限制每个域不超过 50 个 cookie；
 最新版 Firefox 限制每个域不超过 150 个 cookie；
 最新版 Opera 限制每个域不超过 180 个 cookie；
 Safari 和 Chrome 对每个域的 cookie 数没有硬性限制。
如果 cookie 总数超过了单个域的上限，浏览器就会删除之前设置的 cookie。IE 和 Opera 会按照最近
最少使用（LRU，Least Recently Used）原则删除之前的 cookie，以便为新设置的 cookie 腾出空间。Firefox

好像会随机删除之前的 cookie，因此为避免不确定的结果，最好不要超出限制。
浏览器也会限制 cookie 的大小。大多数浏览器对 cookie 的限制是不超过 4096 字节，上下可以有一
个字节的误差。为跨浏览器兼容，最好保证 cookie 的大小不超过 4095 字节。这个大小限制适用于一个
域的所有 cookie，而不是单个 cookie。
如果创建的 cookie 超过最大限制，则该 cookie 会被静默删除。注意，一个字符通常会占 1 字节。如
果使用多字节字符（如 UTF-8 Unicode 字符），则每个字符最多可能占 4 字节。
```

#### 1.2 cookie的构成

cookie 在浏览器中是由以下参数构成的。

```
 名称：唯一标识 cookie 的名称。cookie 名不区分大小写，因此 myCookie 和 MyCookie 是同一
个名称。不过，实践中最好将 cookie 名当成区分大小写来对待，因为一些服务器软件可能这样
对待它们。cookie 名必须经过 URL 编码。

 值：存储在 cookie 里的字符串值。这个值必须经过 URL 编码。

 域：cookie 有效的域。发送到这个域的所有请求都会包含对应的 cookie。这个值可能包含子域（如
www.wrox.com），也可以不包含（如.wrox.com 表示对 wrox.com 的所有子域都有效）。如果不明
确设置，则默认为设置 cookie 的域。

 路径：请求 URL 中包含这个路径才会把 cookie 发送到服务器。例如，可以指定 cookie 只能由
http://www.wrox.com/books/访问，因此访问 http://www.wrox.com/下的页面就不会发送 cookie，即
使请求的是同一个域。

 过期时间：表示何时删除 cookie 的时间戳（即什么时间之后就不发送到服务器了）。默认情况下，
浏览器会话结束后会删除所有 cookie。不过，也可以设置删除 cookie 的时间。这个值是 GMT 格
式（Wdy, DD-Mon-YYYY HH:MM:SS GMT），用于指定删除 cookie 的具体时间。这样即使关闭
浏览器 cookie 也会保留在用户机器上。把过期时间设置为过去的时间会立即删除 cookie。

 安全标志：设置之后，只在使用 SSL 安全连接的情况下才会把 cookie 发送到服务器。例如，请
求 https://www.wrox.com 会发送 cookie，而请求 http://www.wrox.com 则不会。

```

这些参数在 Set-Cookie 头部中使用分号加空格隔开，比如：

```http
HTTP/1.1 200 OK
Content-type: text/html
Set-Cookie: name=value; expires=Mon, 22-Jan-07 07:10:24 GMT; domain=.wrox.com
Other-header: other-header-value
```

这个头部设置一个名为"name"的 cookie，这个 cookie 在 2007 年 1 月 22 日 7:10:24 过期，对 www.wrox.com 及其他 wrox.com 的子域（如 p2p.wrox.com）有效。 安全标志 secure 是 cookie 中唯一的非名/值对，只需一个 secure 就可以了。比如：

```http
HTTP/1.1 200 OK
Content-type: text/html
Set-Cookie: name=value; domain=.wrox.com; path=/; secure
Other-header: other-header-value
```

这里创建的 cookie 对所有 wrox.com 的子域及该域中的所有页面有效（通过 path=/指定）。不过， 这个 cookie 只能在 SSL 连接上发送，因为设置了 secure 标志。 要知道，域、路径、过期时间和 secure 标志用于告诉浏览器什么情况下应该在请求中包含 cookie。 这些参数并不会随请求发送给服务器，实际发送的只有 cookie 的名/值对。

### 2. Web Storage

Web Storage 的目的是解决 通过客户端存储不需要频繁发送回服务器的数据 时使用 cookie 的问题。

Web Storage 规范最新的版本是第 2 版，这一版规范主要有两个目标：

```
 提供在 cookie 之外的存储会话数据的途径；
 提供跨会话持久化存储大量数据的机制。
```

Web Storage 的第 2 版定义了两个对象：`localStorage` 和 `sessionStorage`。`localStorage` 是 永久存储机制，`sessionStorage` 是跨会话的存储机制。这两种浏览器存储 API 提供了在浏览器中不 受页面刷新影响而存储数据的两种方式。2009 年之后所有主要供应商发布的浏览器版本在 `window` 对象 上支持 `localStorage `和 `sessionStorage`。

#### 2.1 sessionStorage

sessionStorage 对象只存储会话数据，这意味着数据只会存储到浏览器关闭。这跟浏览器关闭时 会消失的会话 cookie 类似。存储在 sessionStorage 中的数据不受页面刷新影响，可以在浏览器崩溃 并重启后恢复。（取决于浏览器，Firefox 和 WebKit 支持，IE 不支持。） 

因为 sessionStorage 对象与服务器会话紧密相关，所以在运行本地文件时不能使用。存储在 sessionStorage 对象中的数据只能由最初存储数据的页面使用，在多页应用程序中的用处有限。

#### 2.2 localStorage

在修订的 HTML5 规范里，localStorage 对象取代了 globalStorage，作为在客户端持久存储数据的机制。要访问同一个 localStorage 对象，页面必须来自同一个域（子域不可以）、在相同的端 口上使用相同的协议。

| 名称                 | 作用                                         |
| -------------------- | -------------------------------------------- |
| clear                | 清空localStorage上存储的数据                 |
| getItem              | 读取数据                                     |
| hasOwnProperty       | 检查localStorage上是否保存了变量x，需要传入x |
| key                  | 读取第i个数据的名字或称为键值(从0开始计数)   |
| length               | localStorage存储变量的个数                   |
| propertyIsEnumerable | 用来检测属性是否属于某个对象的               |
| removeItem           | 删除某个具体变量                             |
| setItem              | 存储数据                                     |
| toLocaleString       | 将（数组）转为本地字符串                     |
| valueOf              | 获取所有存储的数据                           |

##### 清空localStorage

```javascript
localStorage.clear()    // undefined    
localStorage            // Storage {length: 0}
```

##### 存储数据

```javascript
localStorage.setItem("name","caibin") //存储名字为name值为caibin的变量
localStorage.name = "caibin"; // 等价于上面的命令
localStorage // Storage {name: "caibin", length: 1}
```

##### 读取数据

```javascript
localStorage.getItem("name") //caibin,读取保存在localStorage对象里名为name的变量的值
localStorage.name // "caibin"
localStorage.valueOf() //读取存储在localStorage上的所有数据
localStorage.key(0) // 读取第一条数据的变量名(键值)
//遍历并输出localStorage里存储的名字和值
for(var i=0; i<localStorage.length;i++){
    console.log('localStorage里存储的第'+i+'条数据的名字为：'+localStorage.key(i)+',值为：'+localStorage.getItem(localStorage.key(i)));
}
```

##### 删除某个变量

```javascript
localStorage.removeItem("name"); //undefined
localStorage // Storage {length: 0} 可以看到之前保存的name变量已经从localStorage里删除了
```

##### 检查localStorage里是否保存某个变量

```javascript
// 这些数据都是测试的，是在我当下环境里的，只是demo哦～
localStorage.hasOwnProperty('name') // true
localStorage.hasOwnProperty('sex')  // false
```

##### 将数组转为本地字符串

```javascript
var arr = ['aa','bb','cc']; // ["aa","bb","cc"]
localStorage.arr = arr //["aa","bb","cc"]
localStorage.arr.toLocaleString(); // "aa,bb,cc"
```

##### 将JSON存储到localStorage里

```javascript
var students = {
    xiaomin: {
        name: "xiaoming",
        grade: 1
    },
    teemo: {
        name: "teemo",
        grade: 3
    }
}

students = JSON.stringify(students);  //将JSON转为字符串存到变量里
console.log(students);
localStorage.setItem("students",students);//将变量存到localStorage里

var newStudents = localStorage.getItem("students");
newStudents = JSON.parse(students); //转为JSON
console.log(newStudents); // 打印出原先对象
```

### 3. IndexDB