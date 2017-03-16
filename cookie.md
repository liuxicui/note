# Cookies

### 服务器端返回 cookie

Cookie 一般都是在服务器端设置的，通过 http 响应的头部返回给浏览器，浏览器拿到这些数据就可以保存到自己的 cookie 文件中。
服务器代码写成
```
response.setHeader('Set-Cookie', 'id=123')
```

### 后端代码加入
```
app.get('/',function (req,res) {
  res.setHeader('Set-Cookie','id=123');
  res.sendFile('index.html');
})
```
后端服务器起开，查看
```
curl -I localhost:3000
```
### 后续行为
后续的默认行为就是，每次浏览器再去访问同一个域名，都会在请求的头部中携带 cookie 信息的。
可以请求一下试试，在浏览器的 Network 标签下，看每次的请求 Headers ，都会看到 cookie 数据的。
```
app.get('/hello',function (req,res) {
  res.setHeader('Set-Cookie','id=123');
  res.sendFile('index.html');
})
```
测试：
```
curl -v --cookie "id=123" http://tiger.haoduoshipin.com/hello
```
后台可以看到
```
{"id":"123"}
```
