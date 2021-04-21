# Express

## 一. 快速上手

### 1. 启动web服务

* 此时一个web服务已经启动，监听4000端口

```js
const express = require('express')

const app = express()

app.listen(4000, () => {
  console.log('App listening on port 3000')
})


```

### 2. 简单路由

```js
const express = require('express')

const app = express()

/* 当客户端请求这个路径时的响应 */
// 监听4000端口的根路径，第二个参数是接收到请求后要执行的函数
// 函数有两个参数，req请求相关，res响应相关
app.get('/', function(req, res) {
  // 服务端响应一条信息：ok
  // res.send('ok')

  // 传了一个数组，里面有个对象，但是客户端接收到的是一个json{ "user": "Johnny" }
  res.send([
    { user: 'Johnny' }
  ])
})

app.listen(4000, () => {
  console.log('App listening on port 4000')
})
```



匹配多个路由：

```js
const express = require('express')

const app = express()

app.get('/', function(req, res) {
  
  res.send({ page: 'home' })
  
})

app.get('/about', function(req, res) {
  res.send({ page: 'About Us' })
})

app.get('/products', function(req, res) {
  res.send([
    { id: 1, title: 'Product A'},
    { id: 2, title: 'Product B'},
    { id: 3, title: 'Product C'},
  ])
})

app.listen(4000, () => {
  console.log('App listening on port 4000')
})
```

目前res.send()的参数能传字符串，对象，数组

### 3. 自动重启项目

```
// index.js的目录终端
// npm i -g nodemon
nodemon index.js
```

### 4. 静态文件托管

```js
const express = require('express')

const app = express()

// 静态文件托管，public目录下的文件，可以通过路径来访问，例如http://127.0.0.1:4000/index.html
//app.use(express.static('public'))

// 第一个参数加上一个前缀路径
// 现在可以通过http://127.0.0.1:4000/static/index.html来访问
app.use('/static', express.static('public'))

app.get('/about', function(req, res) {
  res.send({ page: 'About Us' })
})

app.get('/products', function(req, res) {
  res.send([
    { id: 1, title: 'Product A'},
    { id: 2, title: 'Product B'},
    { id: 3, title: 'Product C'},
  ])
})

app.listen(4000, () => {
  console.log('App listening on port 4000')
})
```

### 5. 跨域

简单呈现跨域效果

这是服务器的一个静态资源，index.html，服务器启动在4000端口，'http://127.0.0.1:4000/products'路径有一个json数据，直接通过浏览器查看这个index.html就会出现跨域问题

```html
<script>
  /* res.json()：获得响应的内容 */
  // res: 一个Response对象
  // res.json(): 一个Promise，值为产品数组
  fetch('http://127.0.0.1:4000/products').then(res => res.json()).then(data => {
    console.log(data)
  })
  
</script>
```

解决跨域问题

* 首先安装: npm i cors
* 在路由之前引用

```js
const express = require('express')

const app = express()

// require('cors')返回值是一个函数，执行后返回一个中间件，使用后就能解决跨域问题
// 要写在路由前面
app.use(require('cors')())

// 静态文件托管，public目录下的文件，可以通过路径来访问，
// 例如http://127.0.0.1:4000/index.html 或者http://127.0.0.1:4000
app.use(express.static('public'))

// 现在可以通过http://127.0.0.1:4000/static/index.html来访问
// app.use('/static', express.static('public'))

app.get('/about', function(req, res) {
  res.send({ page: 'About Us' })
})

app.get('/products', function(req, res) {
  res.send([
    { id: 1, title: 'Product A'},
    { id: 2, title: 'Product B'},
    { id: 3, title: 'Product C'},
  ])
})

app.listen(4000, () => {
  console.log('App listening on port 4000')
})
```

### 6. mongoose的快速上手

```js
const express = require('express')

const app = express()

// 防废弃的操作
mongoose.set('useCreateIndex', true)

// 1. mongoose的连接
const mongoose = require('mongoose')

mongoose.connect('mongodb://localhost:27017/express-test', {
  useNewUrlParser: true,
  useUnifiedTopology: true
})

// 2. 定义模型，利用模型来操作这个集合
// 第一个参数是集合的名称，第二个参数是集合的结构
const Product = mongoose.model('Product', new mongoose.Schema({
  title: String
}))

// 3. 向数据库里插入了三条数据，以便后续使用，使用一次之后就注释掉，不然每次都会插入三条数据
Product.insertMany([
  { id: 1, title: 'Product A'},
  { id: 2, title: 'Product B'},
  { id: 3, title: 'Product C'},
 ])

// require('cors')返回值是一个函数，执行后返回一个中间件，使用后就能解决跨域问题
// 要写在路由前面
app.use(require('cors')())

// 静态文件托管，public目录下的文件，可以通过路径来访问，
// 例如http://127.0.0.1:4000/index.html 或者http://127.0.0.1:4000
app.use(express.static('public'))

// 现在可以通过http://127.0.0.1:4000/static/index.html来访问
// app.use('/static', express.static('public'))

app.get('/about', function(req, res) {
  res.send({ page: 'About Us' })
})

// 4. 数据库的查询
app.get('/products', async function(req, res) {
  res.send(await Product.find())
})

app.listen(4000, () => {
  console.log('App listening on port 4000')
})
```

### 7. mongoose查询

```js
app.get('/products', async function(req, res) {
  // limit：限制显示几条数据，skip：跳过几条数据，结合用作分页
  // 现在的效果是显示二三条数据
  // const data = await Product.find().skip(1).limit(2)

  // 条件查询
  // const data = await Product.find().where({
  //   title: 'Product B'
  // })

  // 排序，查询结果按_id倒序显示
  const data = await Product.find().sort({
    _id: -1
  })
  res.send(data)
})

app.get('/products/:id', async function(req, res) {
  // 获取:id的值
  const data = await Product.findById(req.params.id)
  res.send(data)
})
```

### 8. 增加产品

使用test.html来发送post请求

这样就发送了带有请求体的post请求

```html
<script>
  // 可以像下面这样发送简单 JSON 字符串：
  let payload = JSON.stringify({
    title: 'Product E'
  });
  let jsonHeaders = new Headers({
    // 发送的是json类型数据
    'Content-Type': 'application/json'
  });
  fetch('http://127.0.0.1:4000/products', {
    method: 'POST', // 发送请求体时必须使用一种 HTTP 方法
    body: payload,
    headers: jsonHeaders
  }).then(res => res.json()).then(data => {
    console.log(data) // res.json() 返回响应的内容
  });
</script>
```

```js
// 处理提交到products的post请求，将新增的商品插入数据库
app.post('/products', async function(req, res) {
  const data = req.body
  // create也是插入数据
  const product = await Product.create(data)
  res.send(data)
})
```

### 9. 修改产品

test.html

发送PUT请求

```html
<script>
  // 可以像下面这样发送简单 JSON 字符串：
  let payload = JSON.stringify({
    title: 'Product E'
  });
  let jsonHeaders = new Headers({
    'Content-Type': 'application/json'
  });
  fetch('http://127.0.0.1:4000/products/60007f7e047a04007cdeb359', {
    method: 'PUT', // 发送请求体时必须使用一种 HTTP 方法
    body: payload,
    headers: jsonHeaders
  }).then(data => {
    console.log('data', data)
    console.log('json()', data.json())
  });


</script>
```

server.js

处理PUT请求

```js
// 修改，直接覆盖，修改某个id的产品
app.put('/products/:id', async function(req, res) {
  const product = await Product.findById(req.params.id)
  product.title = req.body.title
  // 直接通过文档修改后，需要保存
  await product.save()
  res.send(product)
})
```

### 10. 删除产品

test.html

发送删除请求

```html
<script>
  // 可以像下面这样发送简单 JSON 字符串：
  let payload = JSON.stringify({
    title: 'Product E'
  });
  let jsonHeaders = new Headers({
    'Content-Type': 'application/json'
  });
  fetch('http://127.0.0.1:4000/products/60007f7e047a04007cdeb359', {
    method: 'DELETE', // 发送请求体时必须使用一种 HTTP 方法
    // body: payload,
    // headers: jsonHeaders
  }).then(data => {
    console.log('data', data)
    console.log('json()', data.json())
  });


</script>
```

server.js

处理删除请求

```js
// 删除产品
app.delete('/products/:id', async function(req, res) {
  const product = await Product.findById(req.params.id)
  // 通过文档删除
  await product.remove()
  res.send({
    success: true
  })
})
```

### 11. rest clinet使用

ctrl shift p 调出一个搜索框

输入install 进入应用商店下载插件

中文插件 Chinese...

post请求插件 rest client

rest client 使用: 

```http
@uri=http://localhost:3001/api // 定义变量
@json=Content-Type: application/json

### // 三个#分隔开每个请求

get {{uri}}/users

### 注册
post {{uri}}/register
{{json}}

{
    "username": "user3",
    "password": "123456"
}

### 登录请求
post {{uri}}/login
{{json}}

{
    "username": "user3",
    "password": "123456"
}

### 个人信息 加上授权，和token，实际情况可能是从localStorage里面读取
get {{uri}}/profile
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjYwMDEyYmI1ZDIwYTU4MTk0MDBkZWQxOSIsImlhdCI6MTYxMDY5MTUwMn0.W3bgu2BNPibdd-2GYORrGUYdHGu31TLejd4QUl6bP1w
```



## 二. 登录验证

```
npm i express @next 装下一个版本

npm i jsonwebtoken 生产token的包 用户登录凭证

bearer 是规范
```

### 1. 准备工作

启动一个express网络服务

```js
const express = require('express')

const app = express()


app.listen(3000, () => {
  console.log('http://localhost:3000 is running')
})
```

连接数据库

```js
const mongoose = require('mongoose')

// 防废弃的操作
mongoose.set('useCreateIndex', true)

mongoose.connect('mongodb://localhost:27017/token-test', {
  useNewUrlParser: true,
  useUnifiedTopology: true
})

const UserSchema = new mongoose.Schema({
  username: {
    type: String,
  },
  password: {
    type: String
  }
})

const User = mongoose.model('User', UserSchema)

module.exports = { User }
```

### 2. 注册

创建一个server.html，用来发起请求

```js
let payload = JSON.stringify({
  username: '用户2',
  password: '123456'
})
let jsonHeaders = new Headers({
  'Content-type': 'application/json'
})
fetch('http://localhost:5000/api/register', {
  method: 'POST',
  body: payload,
  headers: jsonHeaders
}).then(res => res.json()).then( data => console.log(data))
```

server.js

```js

// 处理客户端传来的json数据，不然后面接收到的是空值
app.use(express.json())

// 加上了跨域处理
app.use(require('cors')())

// 处理注册，将提交的用户添加进数据库，再将新用户返回客户端
app.post('/api/register', async (req, res) => {
  const user = await User.create({
    username: req.body.username,
    password: req.body.password
  })

  res.send(user)
})

```

```js
// 请求这个路径时，返回数据库中所有文档，便于查看

app.get('/api', async (req, res) => {
  const users = await User.find()
  res.send(users)
})
```

### 3. 加密

由于密码明文存储不安全，因此需要加密

npm i bcrypt 或者 npm i bcryptjs 

model.js

```js
const UserSchema = new mongoose.Schema({
  username: {
    type: String,
    unique: true,
  },
  password: {
    type: String,
    // 加密val
    set(val) {
      // 第一个参数是明文，第二个参数是密码强度，宜选10，小了密码强度低，大了效率低
      return require('bcrypt').hashSync(val, 10)
    }
  }
})
```

### 4. 登录

server.html

发送登录请求

```js
let payload = JSON.stringify({
  username: '用户4',
  password: '123456'
})
let jsonHeaders = new Headers({
  'Content-type': 'application/json'
})
fetch('http://localhost:5000/api/login', {
  method: 'POST',
  body: payload,
  headers: jsonHeaders
}).then(res => res.json()).then( data => console.log(data)).catch(err => console.log(err))
```

server.js

npm i jsonwebtoken

```js
const jwt = require('jsonwebtoken')
const SECRETE = 'asdfqweasdf/masdf'
```

```js
// 登录
app.post('/api/login', async (req, res) => {


  const user = await User.findOne({
    username: req.body.username
  })

  // 1. 用户不存在
  if(!user) {
    return res.status(422).send({
      message: '用户名不存在'
    })
  }

  // 2. 密码不匹配，第一个参数请求体密码，第二个参数数据库密码
  const isPasswordValid = require('bcrypt').compareSync(
    req.body.password,
    user.password
  )
  if(!isPasswordValid) {
    return res.status(422).send({
      message: '密码不匹配'
    })
  }

  // 根据用户id生成token，第二个参数为密钥，用来加密，解密的
  // token作为用户登录凭证
  const token = jwt.sign({
    id: user._id
  }, SECRETE)
  res.send({
    user,
    token
  })
})
```

### 5. 授权

第二次登录时，客户端记住用户的登录信息

server.html

```js
// 请求时，将token加入请求头
let authHeaders = new Headers({
  'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjYwMDE5MzdlNjZkNzYwMmE1YzJiZDkzYiIsImlhdCI6MTYxMDcxNzI3OX0.MZwc01bnS3MjFA8J7eloT26nkZ3NWwWMafHK7Blz0wA'
})
fetch('http://localhost:5000/api/profile', {
  method: 'GET',
  headers: authHeaders
}).then(res => res.json()).then( data => console.log(data)).catch(err => console.log(err))
```

server.js

如果请求的token正确的话，就将用户信息返回

实际情况应该是，token发送时，就存入localStorage，使用时，也从localStorage中读

```js
const auth = async (req, res, next) => {
  // 拿到加密的token
  const raw = String(req.headers.authorization).split(' ').pop()
  // 解密，拿到id
  // 参数为加密后的token和密钥
  const { id } = jwt.verify(raw, SECRETE)
  // 将user挂载到req上，以便后续使用
  req.user = await User.findById(id)
  // 可以在这之间做一些验证
  next()
}


app.get('/api/profile', auth, (req, res) => {
  res.send(req.user)
})
```

## 三. express api

### 1. router

用router可以把公共前缀提出来

```js
const express = require('express')
// 子路由
const router = express.Router()
router.post('/categories', async (req, res) => {

})
// 中间件，使用这个中间件就可以将/admin/api拼接到前面的路由中
// /categories变成/admin/api/categories，避免每个路由都写/admin/api
// server运行在http://localhost:3000端口
app.use('/admin/api', router)
```

### 2. req res

```js
const express = require('express'); 
const router = express.Router()
router.get('/',(req,res)=>{
    // Request
    // req.baseUrl 基础路由地址
    // req.body post发送的数据解析出来的对象
    // req.cookies 客户端发送的cookies数据
    // req.hostname 主机地址 去掉端口号
    // req.ip 查看客户端的ip地址
    // req.ips 代理的IP地址
    // req.originalUrl 对req.url的一个备份
    // req.params 在使用/:id/:name 匹配params
    // req.path 包含请求URL的路径部分
    // req.protocol http 或https协议
    // req.query 查询字符串解析出来的对象 username=zhangsan&password=123 { username:zhangsan }
    // req.route 当前匹配的路由 正则表达式
    // req.params 获取路由匹配的参数
    // req.get 获取请求header里的参数
    // req.is 判断请求的是什么类型的文件
    // req.param(key名称) 用来获取某一个路由匹配的参数
 
 
    //Response
    // res.headersSent 查看http响应是否响应了http头
    // res.append(名称,value) 追加http响应头
    // res.attachment(文件路径) 响应文件请求 
    // res.cookie() 设置cookie
    
    //res.setHeader('Content-Type','text/html;charset=utf8')
    // res.append('Content-Type','text/html;charset=utf8')
    // res.append('hehe','1008')
    // res.append('haha','1008')
    // res.attachment('./xx.zip') //Content-Disposition: attachment; filename="xx.zip"
    // res.clearCookie(cookiename) 删除cookie
    // res.cookie('zhangsan','lisi') 设置cookie
    // res.cookie('zhangsan1','lisi2',{
    //     maxAge:900000,
    //     httpOnly:true,
    //     path: '/admin', 
    //     secure: true,
    //     signed:true
    // })
    // res.clearCookie('zhangsan')
 
    // res.download(文件的path路径) 跟attachment类似 用来处理文件下载的 参数是文件地址
    // res.end http模块自带的
    // res.format()协商请求文件类型 format匹配协商的文件类型
    // res.format({
    //     'text/plain': function(){
    //         res.send('hey');
    //     },
        
    //     'text/html': function(){
    //         res.send('<p>hey</p>');
    //     },
        
    //     'application/json': function(){
    //         res.send({ message: 'hey' });
    //     },
        
    //     'default': function() {
    //         // log the request and respond with 406
    //         res.status(406).send('Not Acceptable');
    //     }
    // });
 
    // res.get('key') 获取响应header数据
    // res.json() 返回json数据 会自动设置响应header Content-type 为json格式 application/json
 
    // res.json({
    //     xx:100
    // })
 
    // res.json({
    //     xx:100
    // })
 
    // jsonp 利用的就是浏览器加载其他服务器的文件不会存在跨域问题
    // ajax请求就会有跨域问题
 
    // res.setHeader('Content-Type','text/javascript;charsert=utf8')
    // res.end(`typeof ${req.query.callback} == 'function' ? ${req.query.callback}({aa:100}):null`)
 
    // res.jsonp({aaa:100})
 
 
    // 重定向 把访问的地址跳转到另一个地址上
    // res.redirect(301,'/api/aes')
 
    // express jade
    // res.render('index',{title:"hehe",test:"23"})
    // res.send('') 发送数据 可以是任意类型的数据
    // res.sendFile() 发送文件的 
    // res.sendStatus(200) 设置发送时的状态码
    // res.set('Content-Type', 'text/plain') //设置响应header
    // res.status(200) // 设置状态码
    // res.type('') // 直接设置响应的文件类型
 
    // res.type('pdf')
 
    // res.send({aa:100})
    // res.end('ok')
    // res.end({aa:100})
 
    // res.end('你好')
 
 
    // res.end(req.get('Accept-Language'))
    // res.json({
    //     is:req.is('text/html')
    // })
 
    // res.json({
    //     type:req.baseUrl,
    //     hostname:req.hostname,
    //     // ip:req.ip,
    //     // ips:req.ips,
    //     // route:req.route,
    //     ct:req.get('Accept'),
    //     cs:'22'
    // })
})
 
router.get('/:id/:date',(req,res)=>{
    console.log(req.params)
    // res.json(req.params)
    res.end(req.param('date'))
})
 
router.get('/aes',(req,res)=>{
    res.json({
        type:req.baseUrl
    })
})
module.exports = router
```

## 四. 第三方库

### 1. inflection 格式转换

https://www.npmjs.com/package/inflection

小写复数变大写单数

```js
const modelName = require('inflection').classify(req.params.resource)
```

### 2. multer 文件上传

https://github.com/expressjs/multer/blob/master/doc/README-zh-cn.md

```js
// 处理上传文件数据的中间件：multer
const multer = require('multer')
// dest: 要上传的地址，server下的uploads目录，__dirname:当前文件所在文件夹的绝对路径
const upload = multer({dest: __dirname + '/../../uploads'}) 
// upload.single('file'): 上传单个文件，字段名为file，上传时前端提交表单数据的就是file
// multer中间件允许这个接口可以接收文件上传，并且把上传后的文件信息挂载到req.file
// req.file包含文件路径大小等信息，express本身没有req.file
app.post('/admin/api/upload', upload.single('file'), async(req, res) => {
    const file = req.file
    res.send(file)
})
```

### 3. bcypt 加密

require('bcrypt').hashSync(val, 12)

```js
// 定义Category的模型，数据库中会变成复数
const mongoose = require('mongoose')

const schema = new mongoose.Schema({
  username: { type: String },
 
  // set函数，将输入的密码加密之后再存储
  password: { 
    type: String, 
    // select：编辑的时候密码不会显示，这样再次保存的时候，密码就不会二次加密了
    select: false, 
    set(val) {
    // 第一个参数是输入的值，第二个参数是加密等级，应该在10-12比较合理
    return require('bcrypt').hashSync(val, 12)
  }}
})

module.exports = mongoose.model('AdminUser', schema)
```

```js
// 2. 校验密码，参数是明文和密文，返回boolean值，true为正确
const isValid = require('bcrypt').compareSync(password, user.password)
```

### 4. jwt

```js
// 3. 返回token  npm i jsonwebtoken
// app.get(): 只有一个参数，获取属性；多个参数，请求接口
// sign: 第一个参数是对象，可以包含任何信息，一般用id这类可以唯一标识的信息
//       第二个参数是密钥，随机的字符串，将来用来验证token的
//       根据这两个参数就能生成token了
const jwt = require('jsonwebtoken')
const token = jwt.sign({id: user._id,}, app.get('secret'))
```

```js
const { id } = jwt.verify(token, app.get('secret'))
```



