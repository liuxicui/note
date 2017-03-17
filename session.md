# session
### Session 基本原理

当前最流行的做法就是，在每次有一个新的浏览器用户登录进来，服务器端都会为这个浏览器开辟一个小的内存区域（注意，这个区域可以认为是一个文件，但是这个文件是存在服务器端的），通常的术语就叫”又创建了一个新的 session“，那么在这个 session 之中，服务器就可以为这位特定用户（其实就是为特定的那个浏览器）保存任意信息到 session 之中了（例如，用户名，购物车中的商品）。但是同一时刻，连接到服务器的浏览器可能成千上万，那么服务器是如何区分不同用户的浏览器的呢？简单的答案就是，使用 cookie 。

具体来说，每个服务器端的 session 被创建的时候，都会有一个类似于文件名的东西，叫做 sessionId 。每个 session 在服务端被创建的时候，sessionId 都会自动的被发送到浏览器的 cookie 中，这样，每个浏览器就跟自己在服务器上的 session 有了绑定关系了。
装包
```
npm install express-session
```
代码如下：
```
const session = require('express-session');

app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: true
}))

app.post('/login', function(req, res){
  let username =  req.body.username;
  req.session.username = username ;
  res.send(req.session.username);
})
```

-浏览器从客户端发起请求，请求格式为
```
POST /login Content-Type: application/json {"username": "peter"}
```
-这样，服务器端用
```
req.body.username
```
这一个变量，就可以接收到 peter 这个字符串了。

-然后把 ”peter“ 这个字符串保持到 session 中，具体语句就是
```
req.session.username = username
```
-之后，服务器会自动的把 sessionId 返回给浏览器，存储到 cookie 之中
     注意：req.session 接口，内置 cookie 操作功能
-这样，浏览器每次就可以认领自己的 session 来拿到自己的用户名了。
### 使用 curl 测试
```
curl -v -X POST -H "Content-Type: application/json" -d '{"username": "peter"}' localhost:3005/login
```
上面信息重要是分两部分：请求和响应，具体来说：

-请求部分可以看到发出的请求是 POST /login Content-Type: application/json {"username": "peter"} 这个是符合 API 规范的，所以应该能够得到正确的返回
-响应的第一行 < HTTP/1.1 200 OK ，200 表示一切正常
-set-cookie 是我们要查看的重点，我们可以看到 req.session 接口可以正确的返回 sessionId 给浏览器

### 浏览器中的行为
首先要来写一些前端的 axios 代码：
```
let username = 'happypeter';
axios.post('/login', {username}).then(res => {
  console.log(res);
  })
```
FIXME：axios 默认不发送 cookie ？
```
var express = require('express')
var session = require('express-session')

var app = express()

app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: true
}))

app.get('/login', function(req, res){
  req.session.username = 'peter';
  res.send(req.session.username);
})

app.get('/foo', function (req, res, next) {
  res.send(req.session.username)
})

app.get('/bar', function (req, res, next) {
  res.send(req.session.username)
})
app.listen(3003, function(){
  console.log('running on part 3003');
})
```
