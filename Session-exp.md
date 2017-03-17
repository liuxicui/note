# Session例子

## 创建一个项目session，**npm init -y** 生成package.json，然后创建index.js文件，代码如下：

```
const express = require('express');
const app = express();



app.get('/',function(req,res){
  res.sendFile('index.html',{root:'public'});
})//发送的是静态文件

app.listen(3000,function () {
    console.log('running on port 3000...');
})
```

## 创建public/index.html文件，代码如下：

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Session</title>
</head>
<body>
  index.html
</body>
</html>
```

## 打开package.json,修改启动项，代码如下：
```
{
  "name": "session",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "nodemon index.js" //修改的是这一行
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.15.2"
  }
}

```

## 启动项目
```
npm start
```
在浏览器中输入http://localhost:3000/index.html网址，出现
```
Cannot GET /index.html
```
解决方法，在index.js中加入
```
app.use(express.static('public'));
```
把public文件夹中架设成为一个静态（static）服务器，也就是public文件夹中的html页面不走API，直接在浏览器中以链接的方式访问。

## 创建public/login.html文件，代码如下：

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Login</title>
</head>
<body>
  <form method="post" action="/login">
    <label for="username">用户名：</label>
    <input type="text" name="username"/>
    <input type="submit" />
  </form>
</body>
</html>
```

## 在index.html中加入：

```
<a href="/login.html">login</a>
```
## 在index.js中，代码如下：

```
const express = require('express');
const app = express();
app.use(express.static('public'));

const bodyParser = require('body-parser');
app.use(bodyParser.urlencoded({extended:false}));

app.get('/',function(req,res){
  res.sendFile('index.html',{root:'public'});
})//发送的是静态文件
app.post('/login',function(req,res){
  console.log(req.body);
})//要用req.body,需要把HTTP请求体里的文本转换成对象

app.listen(3000,function () {
    console.log('running on port 3000...');
})

```
## 登录，代码改为：
```
app.post('/login',function(req,res){
  let username = req.body.username;
  //User.find({username:username}) 如果在数据库中能够找到，密码正确才算登录成功。
  if(true){
    res.redirect('/');//页面重定向，具体效果前端页面会自动跳转到指定页面，这是自动跳转到首页。
  }
  console.log(req.body);
})//要用req.body,需要把HTTP请求体里的文本转换成对象
```
## 引入session
```
const session = require('express-session');
app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: true
}))
//加密用的，记住就行。

app.post('/login',function(req,res){
  let username = req.body.username;
  //User.find({username:username}) 如果在数据库中能够找到，密码正确才算登录成功。
  req.session.username = username ;
  if(true){
    res.redirect('/');//页面重定向，具体效果前端页面会自动跳转到指定页面，这是自动跳转到首页。
  }
  console.log(req.body);
})//要用req.body,需要把HTTP请求体里的文本转换成对象
```

## 输出username的值
```
app.get('/hello',function(req,res){
  res.send(req.session.username)
})//访问localhost:3000/hello页面，出现username的值。
```

访问localhost：3000/hello

## 去掉
```
app.use(express.static('public'));
```
## 添加
```
app.get('/login',function(req,res){
  let currentUser = req.session.username;
  res.sendFile('login.html',{root:'public'});
})
```
## 添加一句
```
app.get('/login',function(req,res){
  let currentUser = req.session.username;
  res.sendFile('login.html',{root:'public'});
})//发送的是静态文件
```
## 装包
```
npm i --save pug
```
## index.js修改为：
```
const express = require('express');
const app = express();

const bodyParser = require('body-parser');
app.use(bodyParser.urlencoded({extended:false}));

const pug = require('pug');
app.set('view engine','pug');

const session = require('express-session');
app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: true
}))
//加密用的，记住就行。

app.get('/',function(req,res){
  console.log('home page',req.session.username);
  let currentUser = req.session.username;
  res.render('index',{currentUser});
})

app.get('/hello',function(req,res){
  res.send(req.session.username)
})//访问localhost:3000/hello页面，出现username的值。

app.get('/login',function(req,res){
  res.sendFile('login.html',{root:'public'});
})

app.get('/logout',function(req,res){
  req.session.destroy();
  res.redirect('/');
})

app.post('/login',function(req,res){
  let username = req.body.username;
  //User.find({username:username}) 如果在数据库中能够找到，密码正确才算登录成功。
  req.session.username = username ;
  if(true){
    res.redirect('/');//页面重定向，具体效果前端页面会自动跳转到指定页面，这是自动跳转到首页。
  }
  console.log(req.body);
})//要用req.body,需要把HTTP请求体里的文本转换成对象

app.listen(3000,function () {
    console.log('running on port 3000...');
})

```
## 删掉public/index.html，创建views/index.pug
```
html
  body
      h3  当前用户名：
        if  currentUser
          span=currentUser
          a(href='/logout') 退出
        else
          a(href='/login')  登录
          //嵌套标签缩进为一个Tab
```








end
