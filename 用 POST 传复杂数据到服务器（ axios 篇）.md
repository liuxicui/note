# 用 POST 传复杂数据到服务器（ axios 篇）

在 React 应用中，比较少会用到 form 直接提交数据的形式，而更多是采用更为灵活的一种方式， 那就通过 axios/fetch 这样的 http 客户端来发 POST 请求。
跟 form 提交形式不同，axios 提交的内容类型是
```
Content-Type: application/json
```
### 使用 curl 调试 api
```
curl -H "Content-Type:application/json" -X POST -d '{"username":"liuxicui"}' http://tiger.haoduoshipin.com/login
```
### 前台实现最简单的 axios 代码
```
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import axios from 'axios';

class App extends Component {
  componentWillMount(){
    let username = 'liuxicui';
    console.log({username});
    axios.post('http://tiger.haoduoshipin.com/login', {username}).then(res => {
      console.log(res);
    })
  }
  render(){
    return(
      <div>
          test API
      </div>
    )
  }
}

export default App;
```
### 引入一个 form 壳子

```
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import axios from 'axios';


class App extends Component {
  handleSubmit(e){
    e.preventDefault();
    let username = this.refs.username.value;
    axios.post('http://tiger.haoduoshipin.com/login', {username}).then(res => {
      this.setState({msg:res.data.msg});
      console.log(res.data.msg);
    })
  }
  render(){
    return(
      <div>
        <form onSubmit={this.handleSubmit.bind(this)}>
          <input ref='username' type="text" />
          <input type="submit" />
        </form>
      </div>
    )
  }
}

export default App;
```
refs 是 React 自带功能 ref 的中文意思是“指向”或者“指针”。
