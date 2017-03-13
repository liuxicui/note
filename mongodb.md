# Mongodb

###安装

```
sudo apt-get install mongodb
```
### 安装新版本的操作

```
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
$ echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
$ sudo apt-get update
$ sudo apt-get install -y mongodb-org
```
###启动 mongodb

```
mkdir -p data/db
mongod --dbpath=./data/db
```
这样，mongodb 就启动成功了。

###现在要进行 Mongodb 数据库操作，我们就开启 Mongo Shell

```
mongo
```
## 具体操作数据库

第一步，创建一个数据库（digicity）
```
$ use digicity
switched to db digicity
```
查看数据库是否建立成功
```
show dbs
```
第二步，创建集合（users）
```
db.createCollection('users')
```
第三步，把一条数据，保存成一条文档（ Document ）
```
db.users.insert({username: 'liuxicui', sex: 'famale' })
```
第四步，列出一个集合中的所有文档：
```
db.users.find({})
```
###对数据记录进行增删改查
**增**
```
db.users.insert({username: 'cuicui', sex: 'famale'})
```

**改**
```
db.users.update({_id: ObjectId("584b62b830a2a2cbf4c4c3f6")}, {username: "liu", sex:"male"})
```
```
db.users.update({username:'cuicui'},{$set:{sex:'famale'}})
```
**查**
```
db.users.find({})
```
**删**
```
db.users.remove({_id:  ObjectId("584b5dbf30a2a2cbf4c4c3f5")})
```
删除集合中所有文档
```
db.users.remove({})
```
# 图形化的操作界面 mongo-express

(1)全局安装,**npm install -g mongo-express**
(2)先要找到　mongo-express 的配置文件,**npm list -g mongo-express**
(3)找到安装位置后，就可以进入安装文件夹，来修改配置文件了
```
cd /home/liuxicui/.nvm/versions/node/v7.1.0/lib
cd node_modules
cd mongo-express
cp config.default.js config.js
```
(4)把假配置文件 ，改名为真的　config.js , 也就是说是程序真正会读到的配置文件
```
mongo = {
  db:       'db',
  username: 'admin',
  password: 'pass',
  ...
  url:      'mongodb://localhost:27017/db',
};
```
改为：
```
mongo = {
  db:       'users',
  username: '',
  password: '',
  ...
  url:      'mongodb://localhost:27017/users',
};
```
(5)启动　mongo-express 需要开启一个新的命令行标签
```
mongo-express
```
(6)在浏览器上打开localhost:8081中打开，用户名：admin,密码：pass
