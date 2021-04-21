# 一. 地图项目

## 1. 初始化

文件目录

```
css > index.less index.css
images 
font 
js > 复制js目录
index.html
```

插件

```shell
# less转css
easy less 
# px转rem
cssrem
扩展设置，将Root Font Size设置为80px
```

引入

```
flexible.js
```

## 2. 头部盒子

```html
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>数据可视化</title>
  <link rel="stylesheet" href="css/index.css">
</head>
<body>
  <!-- 头部盒子 -->
  <header>
    <h1>数据可视化-ECharts</h1>
    <div class="showTime"></div>
    <script>
      var t = null;
      t = setTimeout(time, 1000);//開始运行
      function time() {
          clearTimeout(t);//清除定时器
          dt = new Date();
          var y = dt.getFullYear();
          var mt = dt.getMonth() + 1;
          var day = dt.getDate();
          var h = dt.getHours();//获取时
          var m = dt.getMinutes();//获取分
          var s = dt.getSeconds();//获取秒
          document.querySelector(".showTime").innerHTML = '当前时间：' + y + "年" + mt + "月" + day + "-" + h + "时" + m + "分" + s + "秒";
          t = setTimeout(time, 1000); //设定定时器，循环运行     
      }
    </script>
  </header>
  <script src="./js/flexible.js"></script>
</body>
</html>
```

```less
// css初始化

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background: url(../images/bg.jpg) 
  no-repeat top center;
  line-height: 1.15;
}

header {
  position: relative;
  height: 1.25rem;
  background: url(../images/head_bg.png)
  no-repeat;
  background-size: 100% 100%;
  h1 {
    font-size: .475rem;
    color: white;
    text-align: center;
    line-height: 1rem;
  }
  .showTime {
    position: absolute;
    top: 0;
    right: .375rem;
    line-height: .9375rem;
    color: rgba(255, 255, 255, 0.7);
    font-size: 0.25rem;
  }
}
```

## 3. 页面布局

```html
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>数据可视化</title>
  <link rel="stylesheet" href="css/index.css">
</head>
<body>
  <!-- 头部盒子 -->
  <header>
    <h1>数据可视化-ECharts</h1>
    <div class="showTime"></div>
    <script>
      var t = null;
      t = setTimeout(time, 1000);//開始运行
      function time() {
          clearTimeout(t);//清除定时器
          dt = new Date();
          var y = dt.getFullYear();
          var mt = dt.getMonth() + 1;
          var day = dt.getDate();
          var h = dt.getHours();//获取时
          var m = dt.getMinutes();//获取分
          var s = dt.getSeconds();//获取秒
          document.querySelector(".showTime").innerHTML = '当前时间：' + y + "年" + mt + "月" + day + "-" + h + "时" + m + "分" + s + "秒";
          t = setTimeout(time, 1000); //设定定时器，循环运行     
      }
    </script>
  </header>

  <!-- 页面主题部分 -->
  <section class="mainbox">
    <!-- 左边图表 -->
    <div class="column">
      <div class="panel bar">
        <h2>柱形图-就业行业</h2>
        <div class="chart"></div>
        <div class="panel-footer"></div>
      </div>
      <div class="panel line">
        <h2>柱形图-就业行业</h2>
        <div class="chart"></div>
        <div class="panel-footer"></div>
      </div>
      <div class="panel pie">
        <h2>柱形图-就业行业</h2>
        <div class="chart"></div>
        <div class="panel-footer"></div>
      </div>
    </div>
    <div class="column">
      <div class="no">
        <div class="no-hd">
          <ul>
            <li>12578</li>
            <li>10000</li>
          </ul>
        </div>
        <div class="no-bd">
          <ul>
            <li>前端需求人数</li>
            <li>市场供应认数</li>
          </ul>
        </div>
      </div>
      <!-- 地图模块 -->
      <div class="map">
        <div class="map1"></div>
        <div class="map2"></div>
        <div class="map3"></div>
        <div class="chart"></div>
      </div>
    </div>
    <!-- 右边图表 -->
    <div class="column">
      <div class="panel bar">
        <h2>柱形图-就业行业</h2>
        <div class="chart"></div>
        <div class="panel-footer"></div>
      </div>
      <div class="panel line">
        <h2>柱形图-就业行业</h2>
        <div class="chart"></div>
        <div class="panel-footer"></div>
      </div>
      <div class="panel pie">
        <h2>柱形图-就业行业</h2>
        <div class="chart"></div>
        <div class="panel-footer"></div>
      </div>
    </div>
  </section>
  <script src="./js/flexible.js"></script>
</body>
</html>
```

```less
// css初始化

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background: url(../images/bg.jpg) 
  no-repeat top center;
  line-height: 1.15;
}

li {
  list-style: none;
}

/* 声明字体*/
@font-face {
  font-family: electronicFont;
  src: url(../font/DS-DIGIT.TTF);
}

header {
  position: relative;
  height: 1.25rem;
  background: url(../images/head_bg.png)
  no-repeat;
  background-size: 100% 100%;
  h1 {
    font-size: .475rem;
    color: white;
    text-align: center;
    line-height: 1rem;
  }
  .showTime {
    position: absolute;
    top: 0;
    right: .375rem;
    line-height: .9375rem;
    color: rgba(255, 255, 255, 0.7);
    font-size: 0.25rem;
  }
}

// 页面主题盒子
.mainbox {
  display: flex;
  min-width: 1024px;
  max-width: 1920px;
  height: 300px;
  margin: 0 auto;
  // 上 左右 下
  padding: .125rem .125rem 0;
  .column {
    flex: 3;
    &:nth-child(2) {
      flex: 5;
      margin: 0 .125rem .1875rem;
    }
  }
  .panel {
    position: relative;
    height: 3.875rem;
    border: 1px solid rgba(25, 186, 139, 0.17);
    background: url(../images/line\(1\).png) 
    rgba(255, 255, 255, 0.03);
    padding: 0 .1875rem .5rem;
    margin-bottom: .1875rem;
    // 边框加角
    &::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 10px;
      height: 10px;
      border-left: 2px solid #02a6b5;
      border-top: 2px solid #02a6b5;
    }
    &::after {
      content: '';
      position: absolute;
      top: 0;
      right: 0;
      width: 10px;
      height: 10px;
      border-right: 2px solid #02a6b5;
      border-top: 2px solid #02a6b5;
    }
    .panel-footer {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      &::before {
        content: '';
        position: absolute;
        bottom: 0;
        left: 0;
        width: 10px;
        height: 10px;
        border-left: 2px solid #02a6b5;
        border-bottom: 2px solid #02a6b5;
      }
      &::after {
        content: '';
        position: absolute;
        bottom: 0;
        right: 0;
        width: 10px;
        height: 10px;
        border-right: 2px solid #02a6b5;
        border-bottom: 2px solid #02a6b5;
      }
    }
    h2 {
      height: .6rem;
      color: #fff;
      line-height: .6rem;
      text-align: center;
      font-size: 00.25rem;
      font-weight: 400;
    }
    .chart {
      height: 3rem;
      background-color: pink;
    }
  }
}

.no {
  background: rgba(101, 132, 226, 0.1);
  padding: .1875rem;
  .no-hd {
    position: relative;
    border: 1px solid rgba(25, 186, 139, 0.17);
    &::before {
      z-index: 999;
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 30px;
      height: 10px;
      border-left: 2px solid #02a6b5;
      border-top: 2px solid #02a6b5;
    }
    &::after {
      content: '';
      position: absolute;
      bottom: 0;
      right: 0;
      width: 30px;
      height: 10px;
      border-right: 2px solid #02a6b5;
      border-bottom: 2px solid #02a6b5;
    }
    ul {
      display: flex;
      li {
        position: relative;
        line-height: 1rem;
        font-size: .875rem;
        color: #ffeb7b;
        flex: 1;
        text-align: center;
        // 引入字体
        font-family: "electronicFont";
        &:nth-child(1)::after {
            position: absolute;
            content: '';
            top: 25%;
            right: 0;
            height: 50%;
            width: 1px;
            background: rgba(255, 255, 255, 0.2);     
        }
      }
    }
  }
  .no-bd {
    ul {
      display: flex;
      li {
        flex: 1;
        text-align: center;
        color: rgba(255, 255, 255, 0.7);
        font-size: .225rem0;
        height: .5rem;
        line-height: .5rem;
        padding-top: .125rem;
      }
    }
  }
}

.map {
  height: 10.125rem;
  position: relative;
  .map1 {
    width: 6.475rem;
    height: 6.475rem;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: url(../images/map.png);
    background-size: 100% 100%;
    opacity: 0.3;
  }
  .map2 {
    width: 8.0375rem;
    height: 8.0375rem;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: url(../images/lbx.png);
    background-size: 100% 100%;
    opacity: 0.6;
    animation: rotate1 linear 15s infinite;
  }
  .map3 {
    width: 7.075rem;
    height: 7.075rem;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: url(../images/jt.png);
    background-size: 100% 100%;
    opacity: 0.6;
    animation: rotate2 linear 15s infinite;
  }
  .chart {
    position: absolute;
    top: 0;
    left: 0;
    // background-color: pink;
    width: 100%;
    height: 10.125rem;
  }
  

  @keyframes rotate1 {
    from {
      // translate和rotate的顺序有影响
      transform: translate(-50%, -50%)
      rotate(0deg);
    }
    to {
      transform: translate(-50%, -50%) 
      rotate(360deg);

    }
  }
  @keyframes rotate2 {
    from {
      // translate和rotate的顺序有影响
      transform: translate(-50%, -50%)
      rotate(0deg);
    }
    to {
      transform: translate(-50%, -50%) 
      rotate(-360deg);

    }
  }
}
```



## 10. 问题

css背景

```css
body {
  background: url(../images/bg.jpg) 
  no-repeat top center;
  line-height: 1.15;
}

header {
  height: 1.25rem;
  background: url(../images/head_bg.png)
  no-repeat;
  background-size: 100% 100%;
}
```

```
1. css背景
2. 背景颜色/图片到盒子的哪一个部分
3. font-face
4. background-size: 100% 100%;
4. 修改收藏图标
```



