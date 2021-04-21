## 1. 方法

get:  GET是最常用的方法，通常用于请求服务器发送某个资源。

POST方法期初是用来向服务器输入数据的，实际上，通常会用它来支持HTML的表单。表单中填好的数据通常会被送给服务器，然后由服务器将其发送到它要去的地方。

**GET和POST的区别：**

​    1、GET请求的数据是放在HTTP包头中的，也就是URL之后，通常是像下面这样定义格式的：
login.action?name=hyddd&password=idontknow&verify=%E4%BD%E5%A5%BD
其中，以?来分隔URL和数据；以&来分隔参数；如果数据是英文或数字，原样发送；如果数据是中文或其它字符，则进行BASE64编码。
而Post是把提交的数据放在HTTP正文中的。
​    2、GET提交的数据比较少，最多1024B，因为GET数据是附在URL之后的，而URL则会受到不同环境的限制的，比如说IE对其限制为2K+35，而POST可以传送更多的数据（理论上是没有限制的，但一般也会受不同的环境，如浏览器、操作系统、服务器处理能力等限制，IIS4可支持80KB，IIS5可支持100KB）。
​    3、Post的安全性要比Get高，因为Get时，参数数据是明文传输的，参数直接暴露在url中，所以不能用来传递敏感信息。而且使用GET的话，还可能造成Cross-site request forgery攻击。而POST数据则可以加密的，但GET的速度可能会快些。
​    4、get请求只能进行url编码，而post支持多种编码方式；get请求会浏览器主动cache，而post支持多种编码方式；get请求参数会被完整保留在浏览历史记录里，而post中的参数不会被保留。
​    5、GET和POST本质上就是TCP链接，并无差别。但是由于HTTP的规定和浏览器/服务器的限制，导致他们在应用过程中体现出一些不同。
​    6、GET产生一个TCP数据包；POST产生两个TCP数据包。对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。





[关于HTTP协议，一篇就够了 - ranyonsue - 博客园 (cnblogs.com)](https://www.cnblogs.com/ranyonsue/p/5984001.html)