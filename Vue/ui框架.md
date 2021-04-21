## 一.   element-ui

### 1. 基本使用

1. 安装 npm i element-ui -S
2. main.js中导入并安装

```js
import Vue from 'vue';
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import App from './App.vue';

Vue.use(ElementUI);

new Vue({
  el: '#app',
  render: h => h(App)
});
```

3. 使用

示例：轮播图

这样就有了八张轮播图，可以在`el-carousel-item`标签内部插入图片

```html
<div class="hello">
    <el-carousel :interval="5000" arrow="always">
        <el-carousel-item v-for="item in 4" :key="item">
            <h3>{{ item }}</h3><img src="../assets/logo.png" alt="">
        </el-carousel-item>
        <el-carousel-item v-for="item in 4" :key="item">
            <h3>{{ item }}</h3>
        </el-carousel-item>
    </el-carousel>
</div>
```

实验证明在八张轮播图的情况下，指示器会太宽导致换行，所以应该修改指示器的默认样式

### 2. 修改样式

流程：现在开发者模式下，修改对应的样式，确认有效后，再改项目中的样式

首先：去掉style标签中的scoped，才能覆盖element的默认样式

其次：要给相应的类加上父标签，限制是这个组件修改的样式

```css
// 这样就成功修改了指示器的宽度
.hello .el-carousel__button {
  width: 10px;
}
```

```css
/* 轮播图容器指示器容器的类 */
  .el-carousel__indicators--horizontal {
    width: 2rem;
    max-width: 400px;
  }
  /* 包装指示器的块 */
  .el-carousel__indicator {
    width: 15px;
  }
  /* 轮播图指示器 */
  .el-carousel__button {
    width: 5px;
    height: 5px;
  }
```

可以将px转化成vw，上网搜是啥插件

### 3. el-container 布局容器

#### 3.1 代码

```html
<el-container style="height: 500px; border: 1px solid #eee">
  <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
    <el-menu :default-openeds="['1', '3']">
      <el-submenu index="1">
        <template slot="title"><i class="el-icon-message"></i>导航一</template>
        <el-menu-item-group>
          <template slot="title">分组一</template>
          <el-menu-item index="1-1">选项1</el-menu-item>
          <el-menu-item index="1-2">选项2</el-menu-item>
        </el-menu-item-group>
        <el-menu-item-group title="分组2">
          <el-menu-item index="1-3">选项3</el-menu-item>
        </el-menu-item-group>
        <el-submenu index="1-4">
          <template slot="title">选项4</template>
          <el-menu-item index="1-4-1">选项4-1</el-menu-item>
        </el-submenu>
      </el-submenu>
      <el-submenu index="2">
        <template slot="title"><i class="el-icon-menu"></i>导航二</template>
        <el-menu-item-group>
          <template slot="title">分组一</template>
          <el-menu-item index="2-1">选项1</el-menu-item>
          <el-menu-item index="2-2">选项2</el-menu-item>
        </el-menu-item-group>
        <el-menu-item-group title="分组2">
          <el-menu-item index="2-3">选项3</el-menu-item>
        </el-menu-item-group>
        <el-submenu index="2-4">
          <template slot="title">选项4</template>
          <el-menu-item index="2-4-1">选项4-1</el-menu-item>
        </el-submenu>
      </el-submenu>
      <el-submenu index="3">
        <template slot="title"><i class="el-icon-setting"></i>导航三</template>
        <el-menu-item-group>
          <template slot="title">分组一</template>
          <el-menu-item index="3-1">选项1</el-menu-item>
          <el-menu-item index="3-2">选项2</el-menu-item>
        </el-menu-item-group>
        <el-menu-item-group title="分组2">
          <el-menu-item index="3-3">选项3</el-menu-item>
        </el-menu-item-group>
        <el-submenu index="3-4">
          <template slot="title">选项4</template>
          <el-menu-item index="3-4-1">选项4-1</el-menu-item>
        </el-submenu>
      </el-submenu>
    </el-menu>
  </el-aside>
  
  <el-container>
    <el-header style="text-align: right; font-size: 12px">
      <el-dropdown>
        <i class="el-icon-setting" style="margin-right: 15px"></i>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item>查看</el-dropdown-item>
          <el-dropdown-item>新增</el-dropdown-item>
          <el-dropdown-item>删除</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
      <span>王小虎</span>
    </el-header>
    
    <el-main>
      <el-table :data="tableData">
        <el-table-column prop="date" label="日期" width="140">
        </el-table-column>
        <el-table-column prop="name" label="姓名" width="120">
        </el-table-column>
        <el-table-column prop="address" label="地址">
        </el-table-column>
      </el-table>
    </el-main>
  </el-container>
</el-container>

<style>
  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }
  
  .el-aside {
    color: #333;
  }
</style>

<script>
  export default {
    data() {
      const item = {
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      };
      return {
        tableData: Array(20).fill(item)
      }
    }
  };
</script>
```

#### 3.2 分析

```
el-container布局容器，属性只有方向和子元素的宽或高
```

```
el-container 
        el-aside
                el-menu
                        el-submenu （可以收起的下拉菜单）
                        		template 导航一 （el-submenu的标题）
                        		el-menu-item-group
                        				template 分组一
                        				el-menu-item 具体的导航项
                        				el-menu-item
		el-container
				el-header
						el-dropdown
                  el-main
                  		  el-table
                  		  		el-table-column
```

### 4. el-dropdown 下拉框

### 5. el-menu 菜单

### 6. el-upload上传图片



