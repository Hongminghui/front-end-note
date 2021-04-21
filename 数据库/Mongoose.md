## 1. mongoose的增删改查

### 1.1 增加

```
Model.create(doc(s), [callback])
- 用来创建一个或多个文档并添加到数据库中
- 参数：
 doc(s) 可以是一个文档对象，也可以是一个文档对象的数组
 callback 当操作完成以后调用的回调函数
```

```js
// 插入一个文档，第一个参数可以是对象，多个文档时，第一个参数是数组
stuModel.create([
  {
    name: '孙悟空',
    age: '586',
    gender: 'male',
    address: '花果山'
  },
  {
    name: '猪八戒',
    age: '566',
    gender: 'male',
    address: '高老庄'
  }], err => {
  if(!err) {
    console.log('写入成功')
  }
})

```

### 1.2 查询

#### 1.2.1 语法

```
Model.find(conditions, [projection], [options], [callback])
 - 查询所有符合条件的文档 总会返回一个数组
Model.findById(id, [projection], [options], [callback])
 - 根据文档的id属性查询文档
Model.findOne([conditions], [projection], [options], [callback])
 - 查询符合条件的第一个文档 总和返回一个具体的文档对象

 conditions 查询的条件
 projection 投影 需要获取到的字段
  - 两种方式
   {name:1,_id:0}
   "name -_id"
 options  查询选项（skip limit）
   {skip:3 , limit:1}
 callback 回调函数，查询结果会通过回调函数返回
    回调函数必须传，如果不传回调函数，压根不会查询
```

#### 1.2.2 案例

```js
// 基本查询，返回了名字为孙悟空的数组
stuModel.find({name: '孙悟空'}, (err, docs) => {
  console.log(docs)
})
```

全部查找，返回10条  

```js
// 分类列表的接口
router.get('/categories', async (req, res) => {
    const items = await Category.find().limit(10)
    res.send(items)
})
```



### 1.3 修改

#### 1.3.1 语法

```
Model.update(conditions, doc, [options], [callback])
 Model.updateMany(conditions, doc, [options], [callback])
 Model.updateOne(conditions, doc, [options], [callback])
 	- 用来修改一个或多个文档
 	- 参数：
 		conditions 查询条件
 		doc 修改后的对象
 		options 配置参数
 		callback 回调函数
 Model.replaceOne(conditions, doc, [options], [callback])
```

#### 1.3.2 案例

```js
// 修改一个
StuModel.updateOne({name:"唐僧"},{$set:{age:20}},function (err) {
	if(!err){
		console.log("修改成功");
	}
})
```

```js
// 修改多个
stuModel.updateMany({name:"孙悟空"},{$set:{age:20}},function (err) {
  if(!err){
    console.log("修改成功");
  }
})
```

```js
// 编辑文章的接口
router.put('/categories/:id', async (req, res) => {
    // 第一个参数是id，第二个参数是更新的内容
    const model = await Category.findByIdAndUpdate(req.params.id, req.body)
    res.send(model)
})
```

### 1.4 删除

#### 1.4.1 语法

```
 Model.remove(conditions, [callback])
 Model.deleteOne(conditions, [callback])
 Model.deleteMany(conditions, [callback])
 findByIdAndDelete(conditions)
```

#### 1.4.2 案例

```js
// 删除文章的接口
router.delete('/categories/:id', async (req, res) => {
    // 第一个参数是id
    await Category.findByIdAndDelete(req.params.id)
    res.send({
        success: true
    })
})
```

### 1.5 长度

```js
Model.count(conditions, [callback])
 	- 统计文档的数量的
 	- 性能比返回数组的长度更好
```

```js
StuModel.count({},function (err , count) {
	if(!err){
		console.log(count);
	}
});
```

## 2. 集合关联

存的是id，找的是文档

```js
// 定义Category的模型，数据库中会变成复数
const mongoose = require('mongoose')

const schema = new mongoose.Schema({
  name: { type: String },
  // 类型是mongoose里的id类型，ref：关联着Category这个模型，也就是它本身
  // 找当前分类的父级分类，就从Category里面找id和parent的type值相同的
  parent: { type: mongoose.SchemaTypes.ObjectId, ref: 'Category'}
})

module.exports = mongoose.model('Category', schema)
```

关联查询一：

```js
// 分类列表的接口
router.get('/categories', async (req, res) => {
    /* 关联查询 */
    // parent字段包含type和ref（关联字段），加上populate方法之后，
    // 就会在关联的集合（Category）中查询和parent的type值id相同的文档，
    // parent字段的值就会变成这个文档，没有parent字段的其他文档没有影响
    const items = await Category.find().populate('parent').limit(10)
    res.send(items)
})
```

关联查询二：

```js
// resource（文章）列表的接口
router.get('/', async (req, res) => {
    // 不是每个模型都有parent字段，所以需要判断
    const queryOptions = {}
    // 模型名称是Category，猜测会把populate('parent')代替setOptions()这一段
    if(req.Model.modelName === 'Category') {
        queryOptions.populate = 'parent'
    }
    const items = await req.Model.find().setOptions(queryOptions).limit(10)
    res.send(items)
})
```

