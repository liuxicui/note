# mongoose

网址：mongoose(http://mongoosejs.com/)

###装包

```
npm install --save mongoose
```
打开 express-hello 的 index.js 文件，导入 mongoose
```
const mongoose = require('mongoose');
```

###连接数据库

```
mongoose.connect('mongodb://localhost:27017/数据库名');
// 执行此行代码之前，要保证 mongodb 数据库已经运行了，而且运行在 27017 端口
```
然后添加下面代码
```
let db = mongoose.connection;
db.on('error', console.log);
db.once('open', function() {
  console.log('success!')
});
```
如果连接数据库成功，就打印 success 。once('open') 的意思就是 “一旦打开成功” 。如果数据库没有启动，是看不到 success 的。

###创建数据 model

创建一个专门的　models/user.js ，里面代码的作用是，实现一个对象跟一个 数据集合之间映射关系。
```
let mongoose = require('mongoose');
let Schema = mongoose.Schema;

const UserSchema = new Schema(
  {
    username: { type: String },
    sex: { type: String }
  }
);
module.exports = mongoose.model('User', UserSchema);
// `User` 会自动对应数据库中的　users 这个集合
// 如果这里是　Apple 那么就会对应　apples 集合
// 如果这里是　Person 那么就会对应　people 集合
```
创建了一个　Schema （概要），Schema 就是用来描述这个集合中每个文档 的数据结构。

### 保存一个文档

```
let User = require('./moduls/user');
```
把　db.once 部分的代码，改成下面这个样子
```
let db = mongoose.connection;
db.on('error', console.log);
db.once('open', function() {
  let user = new User({username:'malaoer',sex:'male'});
  user.save(function (err) {
    if(err) console.log(err);
  })
  console.log('success!')
});

```
find() 接口是一个异步函数，所以它的返回值　users 只能 在回调函数中使用。.exec 字面意思就是执行，我们把回调函数穿给它做参数。
