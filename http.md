# HTTP

##HTTP 请求的格式

###请求行

```
GET/POST [url] HTTP/[version]   GET / HTTP/1.1
```

###请求的头部 Headers

```
[header 名]：[header 值]
```

```
> Host: haoqicat.com
> User-Agent: curl/7.43.0
> Accept: */*
```

```
Server: nginx/1.4.6 (Ubuntu)
Date: Fri, 09 Dec 2016 09:23:59 GMT
Content-Type: text/html; charset=utf-8
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding
```

###总结一下

一个 HTTP 请求，第一会有一个请求行，然后会有几项 header ，最后，可能会有 payload 。这个就是任何一个 HTTP 请求的基本格式了。

##HTTP 响应的格式

###状态行

```
HTTP[版本号] [状态码] [状态信息]
```

```
HTTP/1.1 200 OK
```

简单介绍一下状态码 。

-20x 的状态码都代表某种成功状态。最常见的 200 ，它的意义，就正如它后面跟的状态信息 一样，代表一切 OK 。
-30x 的状态码，意味着资源已经被移动到其他地方了，但是响应中给出了应该跳转到哪里去找到这个资源。这个行为的 术语就叫做 redirect （重定向）。
-40x 的代码也都是代表一种客户端请求错误 。一个最常见的状态码 404 ，它的意 义也跟它后面紧跟的状态信息所说的 一样：Page Not Found （页面未找到）。
-50x 的状态吗也很常见。返回的如果是这一系列的状态码，就意味着服务器端在处理请求的时候出错 。50x 出现，对于开发者，一般意味着服务器端代码出了错误。

##GET 链接中嵌入参数传递到服务器端

###案例：传递用户 id 到服务器

现在我想请求一个具体的用户，通常会发这样的请求
```
GET /users/12345
```

###先给出后台代码
```
const express =  require('express');
const app = express();
const cors = require('cors');
app.use(cors());


app.get('/users/:id', function(req, res){
  console.log(req.params.id)
})

app.listen(3000, function(){
  console.log('running on port 3000...');
});
```
启动服务器

```
node index.js
```
###前台发出请求
```
curl -X GET localhost:3000/users/12345
```
这样，到后台终端 log 中，可以看到 12345 被打印出来了，表示后台以及手到了前台的参数。





end
