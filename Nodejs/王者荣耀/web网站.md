## 一. 初始化

### 1. 使用scss

下载对应loader

```
npm i -D sass sass-loader
```

现在引入scss文件即可

### 2. 重置样式

src > style.scss

```scss
// reset
* {
  box-sizing: border-box;
  outline: none;
}

html {
  font-size: 13px;
}

body {
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.2em; // 字体的1.2倍
  background-color: #f1f1f1;
}

a {
  color: #999;  
}
```

src > main.js

```js
import './style.scss'
```

