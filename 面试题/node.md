### 1. npm命令

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

### 2. cookie

一、关于Cookie
在我们关闭一个登录过的网址并重新打开它后，我们的登录信息依然没有丢失；当我们浏览了商品后历史记录里出现了我们点击过的商品；当我们推回到首页后，推荐商品也为我们选出了相似物品；事实上当我们有过此类操作后，浏览器会将我们的操作信息保存到cookie上面。阿进而言之，cookie就是储存在用户本地终端上的数据。

Cookie的特点

* cookie保存在浏览器本地，只要不过期关闭浏览器也会存在。
* 正常情况下cookie不加密，用户可轻松看到
* 用户可以删除或者禁用cookie
* cookie可以被篡改
* cookie可用于攻击
* cookie存储量很小，大小一般是4k
* 发送请求自动带上登录信息
  　　

二、Cookie的安装及使用

1. 安装

```bash
cnpm install cookie-parser --save
```

2. 引入

```js
const cookieParser=require("cookie-parser");
```

　3. 设置中间件

```js
app.use(cookieParser());
```

4. 设置cookie

```js
res.cookie("name",'zhangsan',{maxAge: 900000, httpOnly: true}); 
//res.cookie(名称,值,{配置信息})
```

关于设置cookie的参数说明：

domain: 域名 

name=value：键值对，可以设置要保存的 Key/Value，注意这里的 name 不能和其他属性项的名字一样

Expires： 过期时间（秒），在设置的某个时间点后该 Cookie 就会失效，如 expires=Wednesday, 09-Nov-99 23:12:40 GMT。

maxAge： 最大失效时间（毫秒），设置在多少后失效 。

secure： 当 secure 值为 true 时，cookie 在 HTTP 中是无效，在 HTTPS 中才有效 。

Path： 表示 在那个路由下可以访问到cookie。

httpOnly：是微软对 COOKIE 做的扩展。如果在 COOKIE 中设置了“httpOnly”属性，则通过程序（JS 脚本、applet 等）将无法读取到COOKIE 信息，防止 XSS 攻击的产生 。

singed：表示是否签名cookie, 设为true 会对这个 cookie 签名，这样就需要用 res.signedCookies 而不是 res.cookies 访问它。被篡改的签名 cookie 会被服务器拒绝，并且 cookie 值会重置为它的原始值。

5.获取cookie

```js
req.cookie.name;　　
```

下面是一个基础实例：

```js

const express=require("express");
const cookieParser=require("cookie-parser");
 
var app=express();
 
//设置中间件
app.use(cookieParser());
 
app.get("/",function(req,res){
    res.send("首页");
});
 
//设置cookie
app.get("/set",function(req,res){
    res.cookie("userName",'张三',{maxAge: 20000, httpOnly: true});
    res.send("设置cookie成功");
});
 
//获取cookie
app.get("/get",function(req,res){
    console.log(req.cookies.userName);
    res.send("获取cookie成功，cookie为："+ req.cookies.userName);
});
 
app.listen(8080);
　　
```

当访问set路由后会设置cookie，当访问get路由后会获取到设置的cookie值。当然你也可以在其他页面继续获取当前cookie，以实现cookie共享。 