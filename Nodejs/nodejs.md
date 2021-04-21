## 1. nodejs介绍

学习：https://segmentfault.com/a/1190000010548504?utm_source=sf-related

学习：https://github.com/chyingp/nodejs-learning-guide

### 1.1 nodejs特点

* Node.js 是 JavaScript 运行时环境
* 没有Bom，Dom
* 在 Node 中这个 JavaScript 执行环境为 JavaScript 提供了一些服务器级别的 API
  * 例如文件的读写
  * 网络服务的构建
  * 网络通信
  * http 服务器

* envent-driven 事件驱动
* non-blocking I/O mode 非阻塞I/O模型（异步）
* ightweight and efficent. 轻量和高效

### 1.2 nodejs模块

* 核心模块
  * fs 文件操作模块
  * http 网络服务构建模块
  * os 操作系统信息模块
  * path 路径处理模块
* 用户自定义模块
  * 通过require('路径') 加载模块，例如：require('./b.js')，`相对路径 ./ 不能省略`
  * 可以省略文件后缀名，默认 .js
* 第三方模块
  * art-template
  * 必须通过npm来下载才可以使用



## 2. CommonJs模块规范

### 2.1 加载（require）

语法：

```javascript
var 自定义变量名 = require('模块')
```

作用：

* 执行被加载模块中的代码
* 得到被加载模块中的`exports`导出接口对象

使用require加载自定义模块，会先执行模块内的语句，结束以后再执行后面的语句

```js
console.log('a start');

// 相对路径./不能少，不然会被认为是加载核心模块
require('./3.requireTest');

console.log('a end');
```

```js
// 3.requireTest
console.log('b start');
```

### 2.2 导出（exports）

* Node 中是模块作用域，默认文件中所有的成员只在当前模块有效
* 对于希望可以被其他模块访问到的成员，我们需要把这些公开的成员都挂载到`exports`接口对象中就可以了

导出多个成员（必须在对象中）：

```javascript
exports.a = 123;
exports.b = 'hello';
exports.c = function(){
    console.log('bbb')
};
exports.d = {
    foo:"bar"
};
```

导出单个成员：

```javascript
module.exports = 'hello';
// 后者会覆盖前者
module.exports = function add(x,y) {
    return x+y;
}
```

也可以通过以下方法来导出多个成员：

```javascript
module.exports = {
    foo = 'hello',
    add:function(){
        return x+y;
    }
};
```

### 2.3 原理

在node中，每个模块内部都有一个自已的 module 对象

在 module 对象中，有一个成员 exports 也是一个对象

```javascript
module = {
     exports = {
     }
}
```

`当给exports重新赋值后，exports！= module.exports.`
`最终return的是module.exports,无论exports中的成员是什么都没用。`

exports是`module.exports`的一个引用：

```javascript
console.log(exports === module.exports);	// true
exports.foo = 'bar';
//等价于
module.exports.foo = 'bar';
```

真正去使用的时候：

* 导出单个成员：module.exports = xxx;     // 获取的是
* 导出多个成员：module.exports.xxx 或者 modeule.exports = {};

`exports = module.exports`用来重新建立关系

### 2.4 require方法加载规则

* 优先从缓存加载（不会重复加载）
* 判断模块标识符
  * 核心模块
  * 用户自定义模块（以路径形式加载）
  * 第三方模块
    * 先找到当前文件所处目录中的 node_modules 目录
    * node_modules/art-template（模块名）
    * node_modules/art-template/package.json 文件
    * node_modules/art-template/package.json 文件中的 main 属性
    * main 属性中就记录了 art-template 的入口模块
    * 然后加载使用这个第三方包
    * 如果 package.json 文件不存在或者 main 指定的入口模块也没有
    * 则 node 会默认找该目录下的 Index.js
    * 如果以上任何一个条件都不成立，则会进入上一级目录按以上规则查找
    * 直到磁盘根目录，最后报错

### 2.5 npm

#### 常用命令

* npm init 生成package.json说明书文件
  * npm init -y 可以跳过向导，快速生成
* npm install
  * dependencies选项中的依赖项全部安装
  * npm i
* npm install 包名
  * 只下载
  * npm i 包名
* npm install --save/-S 包名
  * 下载并且保存依赖项 package.json文件中的dependencies选项，生产环境依赖
  * npm i 包名

* npm install --dev/-D 开发环境依赖

* npm uninstall 包名    
  * 只删除，如果有依赖项会依然保存
  * npm un 包名
* npm uninstall --save 包名
  * 删除的同时也会把依赖信息全部删除
  * npm un 包名
* npm help
  * 查看使用帮助
* npm 命令 --help
  * npm uninstall --help 查看具体命令的使用帮助

## 3. 核心模块

### 3.1 fs模块

#### 3.1.1 读取文本

读取文本 -- 异步读取

```js
var fs = require('fs');
// 异步读取
// 参数1：文件路径，
// 参数2：读取的字符集(可选)
// 参数3：读取文件后的回调
// err：读取成功时为null，失败时为错误对象，
// data: 读取成功时为文件二进制信息，失败时为undefined
fs.readFile('demoFile.txt', 'utf8', function (err, data) {
   if (err) {
       return console.error(err);
   }
   console.log("异步读取: " + data.toString());
});

```

读取文本 -- 同步读取

```js
// fs.readFileSync
var fs = require('fs');
var data;

try{
    data = fs.readFileSync('./fileForRead.txt', 'utf8');
    console.log('文件内容: ' + data);
}catch(err){
    console.error('读取文件出错: ' + err.message);
}
```

```js
// 封装文件读写，返回promise
const promiseReadFile = function (path, encoding) {
  return new Promise((resolve, reject) => {
    fs.readFile(path, encoding,(err, data) => {
      if(err) {
        reject(err);
      }else {
        resolve(data);
      }
    })
  })
}
```



#### 3.1.2 写入文本

覆盖写入

```js
var fs = require('fs');
//每次写入文本都会覆盖之前的文本内容
fs.writeFile('input.txt', '抵制一切不利于中国和世界和平的动机！',  function(err) {
   if (err) {
       return console.error(err);
   }
   console.log("数据写入成功！");
   console.log("--------我是分割线-------------")
   console.log("读取写入的数据！");
   fs.readFile('input.txt', function (err, data) {
      if (err) {
         return console.error(err);
      }
      console.log("异步读取文件数据: " + data.toString());
   });
});
```

追加写入

```js
var fs = require('fs');
fs.appendFile('input.txt', '愿世界和平！', function (err) {
   if (err) {
       return console.error(err);
   }
   console.log("数据写入成功！");
   console.log("--------我是分割线-------------")
   console.log("读取写入的数据！");
   fs.readFile('input.txt', function (err, data) {
      if (err) {
         return console.error(err);
      }
      console.log("异步读取文件数据: " + data.toString());
   });
});
```

#### 3.1.3 读取目录

```js
const fs = require('fs')
const {promiseAppendFile} = require('./2.writeFile')

// 读目录，参数一：文件路径；参数二：字符集，可选；参数三：回调函数，函数的第二个参数是目录下的文件名数组
fs.readdir('../data', 'utf8', (err, files) => {
  if(err) {
    console.log(err)
  }else {
    files.forEach(file => {
      // 遍历数组，将其写入hello.txt中
      promiseAppendFile('../data/hello.txt', file + '\n').then(
        () => console.log('success')
      )
    })
  }
})
```

### 3.2 http模块

#### 3.1.1. ip地址与端口号

```
ip 地址用来定位计算机
端口号用来定位具体的应用程序
所有需要联网通信的应用程序都会占用一个端口号
```

#### 3.1.2 基本使用

所有后端动态语言要想运行起来，都得先搭建服务器。Node.js 搭建服务器需要用到一个原生的模块 http。

1. 加载 http 模块
2. 调用 http.createServer() 方法创建服务，方法接受一个回调函数，回调函数中有两个参数，第一个是请求体，第二个是响应体。
3. 在回调函数中一定要使用 response.end() 方法，用于结束当前请求，不然当前请求会一直处在等待的状态。
4. 调用 listen 监听一个端口。

```js
//原生模块
var http = require('http');

// request：请求信息，response：响应信息
http.createServer(function(reqeust, response){
	response.end('Hello Node');
}).listen(8080);
```

#### 3.1.2 request.setHeader

```js
request.setHeader('Content-Type', 'application/json');
```

```js
request.setHeader('Cookie', ['type=ninja', 'language=javascript']);
```

#### 3.1.3 简单路由判断

```js
let http = require("http");
//http模块为内置模块，不需要安装
//创建server服务器对象
let server = http.createServer()
//监听对当前服务器对象的请求
server.on('request',function(req,res){
    //当服务器被请求时，会触发请求事件，并传入请求对象和响应对象；
    console.log(req.url)
    //console.log(req.headers)
    res.setHeader("Content-Type","text/html; charset=UTF-8")
    //根据路径信息，显示不同的页面内容
    if(req.url==="/"){
        res.end('<h1>首页</h1><img src="https://www.baidu.com/img/bd_logo1.png?where=super">')
    }else if(req.url==="/gnxw"){
        res.end("<h1>国内新闻</h1>")
    }else if(req.url==="/ylxw"){
        res.end("<h1>娱乐新闻</h1>")
    }else{
        res.end("<h1>404!页面找不到！</h1>")
    }
    

})

//服务器监听的端口号
server.listen(80,function(){
    //启动监听端口号成功时触发
    console.log("服务器启动成功！")
})
```



### 3.3 url模块

请求的 url 都是字符串类型，url 所包含的信息也比较多，比如有：协议、主机名、端口、路径、参数、锚点等，如果对字符串解析这些信息的话，会相对麻烦，因此，Node.js 的原生模块 url 模块便可轻松解决这一问题

#### 3.3.1 字符串转对象

- 格式：url.parse(urlstring, boolean)
- 参数
  - urlstring：字符串格式的 url
  - boolean：在 url 中有参数，默认参数为字符串，如果此参数为 true，则会自动将参数转转对象
- 常用属性
  - href： 解析前的完整原始 URL，协议名和主机名已转为小写
  - protocol： 请求协议，小写
  - host： url 主机名，包括端口信息，小写
  - hostname: 主机名，小写
  - port: 主机的端口号
  - pathname: URL中路径，下面例子的 /one
  - search: 查询对象，即：queryString，包括之前的问号“?”
  - path: pathname 和 search的合集
  - query: 查询字符串中的参数部分（问号后面部分字符串），或者使用 querystring.parse() 解析后返回的对象
  - hash: 锚点部分（即：“#”及其后的部分）

```js
var url = require('url');

//第二个参数为true
var urlObj = url.parse('http://www.dk-lan.com/one?a=index&t=article&m=default#dk', true);
//urlObj.query 为一个对象，{a: 'index', t: 'article', m: 'default'}
// urlObj拥有path search query等属性
console.log(urlObj);

//第二个参数为 false
urlObj = url.parse('http://www.dk-lan.com/one?a=index&t=article&m=default#dk', false);
//urlObj.query 为一个字符串 => ?a=index&t=article&m=default
console.log(urlObj);
```

#### 3.3.2 对象转字符串

- 格式：url.format(urlObj)
- 参数 urlObj 在格式化的时候会做如下处理
  - href: 会被忽略，不做处理
  - protocol：无论末尾是否有冒号都会处理，协议包括 http, https, ftp, gopher, file 后缀是 :// (冒号-斜杠-斜杠)
  - hostname：如果 host 属性没被定义，则会使用此属性
  - port：如果 host 属性没被定义，则会使用此属性
  - host：优先使用，将会替代 hostname 和port
  - pathname：将会同样处理无论结尾是否有/ (斜杠)
  - search：将会替代 query属性，无论前面是否有 ? (问号)，都会同样的处理
  - query：(object类型; 详细请看 querystring) 如果没有 search,将会使用此属性.
  - hash：无论前面是否有# (井号, 锚点)，都会同样处理

```
var url = require('url');

var urlObj = { 
    firstname: 'dk',
    url: 'http://dk-lan.com',
    lastname: 'tom',
    passowrd: '123456' 
}
var urlString = url.format(urlObj);
console.log(urlString);
```

#### 3.3.3 url.resolve

当有多个 url 需要拼接处理的时候，可以用到 url.resolve

```
var url = require('url');
url.resolve('http://dk-lan.com/', '/one')// 'http://dk-lan.com/one'
```

### 3.4 querystring模块

在nodejs中，提供了**querystring**这个模块，用来做url查询参数的解析，使用非常简单。

模块总共有四个方法，绝大部分时，我们只会用到 **.parse()**、 **.stringify()**两个方法。剩余的方法，感兴趣的同学可自行查看文档。

- **.parse()**：对url查询参数（字符串）进行解析，生成易于分析的json格式。
- **.stringif()**：跟**.parse()**相反，用于拼接查询查询。

```
querystring.parse(str[, sep[, eq[, options]]])
querystring.stringify(obj[, sep[, eq[, options]]])
```

#### 3.4.1 查询参数解析

> 参数：querystring.parse(str[, sep[, eq[, options]]])

第四个参数几乎不会用到,直接不讨论. 第二个, 第三个其实也很少用到,但某些时候还是可以用一下。直接看例子

```js
var querystring = require('querystring');
var str = 'nick=casper&age=24';
var obj = querystring.parse(str);
console.log(obj);
console.log(JSON.stringify(obj, null, 4));
```

输出如下

```js
{
    nick: "casper",
    age: "24"
}
```

```js
{
    "nick": "casper",
    "age": "24"
}
```

再来看下`sep`、`eq`有什么作用。相当于可以替换`&`、`=`为自定义字符，对于下面的场景来说还是挺省事的。

```js
var str1 = 'nick=casper&age=24&extra=name-chyingp|country-cn';
var obj1 = querystring.parse(str1);
var obj2 = querystring.parse(obj1.extra, '|', '-');
console.log(JSON.stringify(obj2, null, 4));
```

输出如下

```js
{
    "name": "chyingp",
    "country": "cn"
}
```

#### 3.4.2 查询参数拼接

> querystring.stringify(obj[, sep[, eq[, options]]])

没什么好说的，相当于`parse`的逆向操作。直接看代码

```js
var querystring = require('querystring');

var obj1 = {
    "nick": "casper",
    "age": "24"
};
var str1 = querystring.stringify(obj1);
console.log(str1);

var obj2 = {
    "name": "chyingp",
    "country": "cn"
};
var str2 = querystring.stringify(obj2, '|', '-');
console.log(str2);
```

输出如下

```js
nick=casper&age=24
name-chyingp|country-cn
```

### 3.5 path模块

- 获取路径：path.dirname(filepath)
- 获取文件名：path.basename(filepath)
- 获取扩展名：path.extname(filepath)

#### 3.5.1 获取所在路径

例子如下：

```
var path = require('path');
var filepath = '/tmp/demo/js/test.js';

// 输出：/tmp/demo/js
console.log( path.dirname(filepath) );
```

#### 3.5.2 获取文件名

严格意义上来说，path.basename(filepath) 只是输出路径的最后一部分，并不会判断是否文件名。

但大部分时候，我们可以用它来作为简易的“获取文件名“的方法。

```
var path = require('path');

// 输出：test.js
console.log( path.basename('/tmp/demo/js/test.js') );

// 输出：test
console.log( path.basename('/tmp/demo/js/test/') );

// 输出：test
console.log( path.basename('/tmp/demo/js/test') );
```

如果只想获取文件名，单不包括文件扩展呢？可以用上第二个参数。

```
// 输出：test
console.log( path.basename('/tmp/demo/js/test.js', '.js') );
```

#### 3.5.3 获取文件扩展名

简单的例子如下：

```
var path = require('path');
var filepath = '/tmp/demo/js/test.js';

// 输出：.js
console.log( path.extname(filepath) );
```

#### 3.5.4 path.join()

拼接路径

```js
path.join('/目录1', '目录2', '目录3/目录4', '目录5', '..');
// 返回: '/目录1/目录2/目录3/目录4'
// 猜测是最后一个..，返回上一级目录了

path.join('目录1', {}, '目录2');
// 抛出 'TypeError: Path must be a string. Received {}'
```

#### 3.5.5 path.resolve()

拼接成绝对路径

```js
path.resolve('/目录1/目录2', './目录3');
// 返回: '/目录1/目录2/目录3'

path.resolve('/目录1/目录2', '/目录3/目录4/');
// 返回: '/目录3/目录4'

path.resolve('目录1', '目录2/目录3/', '../目录4/文件.gif');
// 如果当前工作目录是 /目录A/目录B，
// 则返回 '/目录A/目录B/目录1/目录2/目录4/文件.gif'
```

### 3.6 readline模块

`readline` 模块提供了一个接口，用于一次一行地读取[可读流](http://nodejs.cn/s/zz7Dx3)（例如 [`process.stdin`](http://nodejs.cn/s/gagmJq)）中的数据。 可以使用以下方式访问它：

```js
const readline = require('readline')
const fs = require('fs')

// 创建readline.Interface实例，每个实例都关联一个input，output
const rl = readline.createInterface({
  // 大概是有一个模块对象process，process.stdin会读取用户输入的内容
  input: process.stdin,
  // process.stdout，输出，大概就是输出给用户看的内容
  output: process.stdout
})

// answer接收到的就是输入的内容
rl.question('你喜欢Nodejs吗？\n', answer => {
  // 模板字符串
  console.log(`感谢您的回答：${answer}`)
  // 关闭input, output
  rl.close()
})

// 监听rl.close()事件，还有换行line，暂停pause等事件
rl.on('close', () => {
  console.log('事件关闭')
})
```

一个创建包的小案例

```js
const readline = require('readline')
const fs = require('fs')

// 创建readline.Interface实例，每个实例都关联一个input，output
const rl = readline.createInterface({
  // 大概是有一个模块对象process，process.stdin会读取用户输入的内容
  input: process.stdin,
  // process.stdout，输出，大概就是输出给用户看的内容
  output: process.stdout
})

// 封装提问事件，将答案保存下来
function promiseQuestion(question) {
  return new Promise((resolve, reject) => {
    rl.question(question, answer => {
      resolve(answer)
    })
  })
}

async function createPackage() {
  let name = await promiseQuestion('你的名字是：')
  let description = await promiseQuestion('你对自己的描述：')
  let main = await promiseQuestion('主要信息：')
  let author = await promiseQuestion('你的职业：')

  let content = `{
        "name": "${name}",
        "version": "1.0.0",
        "description": "${description}",
        "main": "${main}",
        "scripts": {
          "test": "echo Error: no test specified && exit 1"
        },
        "keywords": [
          "'LAOCHEN'"
        ],
        "author": "${author}",
        "license": "ISC",
        "dependencies": {}
      }`
  
  fs.appendFile('../data/package.json', content, err => {
    console.log(err)
  })
  rl.close()
  
}

createPackage().then(() => {})

```

```
read > index > write/append > delete > buffer
04fsdir index > deleteDir > input 
stream 监听事件
```

### 3.7 事件

```js
let fs = require("fs");

// 异步任务
fs.readFile("hello.txt",{flag:"r",encoding:"utf-8"},function(err,data){
    if(err){
        console.log(err)
    }else{
        console.log(data)
        console.log(lcEvent)
        // 为什么在声明前就使用了lcEvent
        lcEvent.emit('helloSuccess',data)
        //1数据库查看所有的用详细信息
        //2统计年龄比例
        //3查看所有用户学校的详细信息
        
    }
})


// console.log(lcEvent)
let lcEvent = {
    event:{
        //fileSuccess:[fn,fn,fn]
    },
    // 事件监听
    // 参数一：事件名，参数二：事件函数
    on:function(eventName,eventFn){
        if(this.event[eventName]){
            this.event[eventName].push(eventFn)
        }else{
            this.event[eventName] = []
            this.event[eventName].push(eventFn)
        }
    },
    // 事件触发
    emit:function(eventName,eventMsg){
        if(this.event[eventName]){
            this.event[eventName].forEach(itemFn => {
                itemFn(eventMsg)
            });
        }else {
            console.log('none')
        }
    }
}

// 猜测先执行了这里的函数
lcEvent.on('helloSuccess',function(eventMsg){
    console.log("1数据库查看所有的用户详细信息")
})
lcEvent.on('helloSuccess',function(eventMsg){
    console.log("2统计用户年龄比例")
})
lcEvent.on('helloSuccess',function(eventMsg){
    console.log("3查看所有用户学校的详细信息")
})
```

```js
let eventEmitter = require("events");
let fs = require("fs")

// let ee = new events.EventEmitter();
// 文档写法
let ee = new eventEmitter();

ee.on("helloSuccess",function(eventMsg){
    console.log("1吃夜宵")
    console.log(eventMsg)
})
ee.on("helloSuccess",function(){
    console.log("2唱K")
})
ee.on("helloSuccess",function(){
    console.log("3打王者")
})
ee.on("helloSuccess",function(){
    console.log("4打dota")
})

function lcReadFile(path){
    return new Promise(function(resolve,reject){
        fs.readFile(path,{encoding:"utf-8"},function(err,data){
            if(err){
                //console.log(err)
                reject(err)
            }else{
                //console.log(data)
                resolve(data)
                //ee.emit("helloSuccess",data)
            }
        })
    })
}

lcReadFile('hello.txt').then(function(data){
    ee.emit("helloSuccess",data)
})

async function test(){
    let data = await lcReadFile('hello.txt')
    ee.emit("helloSuccess",data)
}
test()
```

