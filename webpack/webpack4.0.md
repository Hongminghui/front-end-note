## 一. 初始化

### 1. 初始化

```shell
# 初始化
npm init
# 生产环境安装，上线不需要
cnpm i webpack webpack-cli -D
# 打包命令，执行此命令，默认打包输出dist > main.js
npx webpack
# 默认webpack配置文件
根目录下创建webpack.config.js
```

### 2. webpack.config.js简单配置

```js
// webpack是node写出来的 用的都是node的写法

let path = require('path') // node核心模块，内置

module.exports = {
  mode: 'development', // 模式，默认两种 production/development
  entry: './src/index.js', // 入口文件
  output: {
    filename: 'bundle.js', // 打包后的文件名
    path: path.resolve(__dirname, 'dist'), 
    // 打包后文件的路径，路径必须是绝对路径，会找到dist目录的绝对路径
    // path.resolve(__dirname, 'dist')会找到webpack.config.js的目录，然后在这个目录下找dist目录
  }
}
```

### 3. 修改配置文件名字

修改配置文件(webpack.config.js)名字，例如修改为webpack.config.my.js

方法一：打包时使用npx webpack --config webpack.config.my.js命令

方法二：在package.json里修改，增加一个脚本

```json
"scripts": {
    "build": "webpack --config webpack.config.my.js" 
  },
```

执行npm run build，就会执行build后面的语句

目前只能打包js文件

## 二. 扩展功能

### 1. webpack-dev-server

```shell
# 根目录下执行
npm i webpack-dev-server -D
npx webpack-dev-server 
```

