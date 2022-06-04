---
title: axios学习
date: 2021-08-01 11:56:47
tags: axios

---

笔记基于axios中文网：https://axios-http.com/zh/docs/intro

github地址：https://github.com/axios/axios

Axios 是一个基于 *[promise](https://javascript.info/promise-basics)* 网络请求库，作用于[`node.js`](https://nodejs.org/) 和浏览器中。在服务端它使用原生 node.js `http` 模块, 而在客户端 (浏览端) 则使用 XMLHttpRequests。

它具有一下特征：

- 从浏览器创建 [XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- 从 node.js 创建 [http](http://nodejs.org/api/http.html) 请求
- 支持 [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) API
- 拦截请求和响应
- 转换请求和响应数据
- 取消请求
- 自动转换JSON数据
- 客户端支持防御[XSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery)

## 用法举例

发起多个并发请求

```js
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

Promise.all([getUserAccount(), getUserPermissions()])
  .then(function (results) {
    const acct = results[0];
    const perm = results[1];
  });
```

支持async/await语法，如发送多个异步请求，第一个请求的结果作为第二个请求的参数。

```js
axios.defaults.baseURL='http://localhost'
async function queryData(){
    let info=await axios.get('async1');
    let res=await axios.get('async2?info='+info.data);
    return res;
}
// async 函数的返回值是promise对象！
queryData().then(response=>{
    console.log(response.data);//hello async1
});
```

> 接口配置

```js
app.get('/async1',(req,res)=>{
    res.send('async1');
})
app.get('/async2',(req,res)=>{
    // 如果请求地址存在参数
    if(req.query){
        return res.send('hello '+req.query.info)
    }
    res.send('hello');
})
```







## 常用API

### GET请求

可以通过url或者params选项传递参数。

- 通过url传递参数

  ```js
  axios.get('http://localhost/axios?id=12').then(response=>{
      console.log(response.data);
  });
  axios.get('http://localhost/axios/45').then(response=>{
      console.log(response.data);
  });
  ```

  上述请求分别调用第一个和第二接口

  ```js
  app.get('/axios',(req,res)=>{
      res.send('axios get '+req.query.id);
  })
  app.get('/axios/:id',(req,res)=>{
      res.send('restful get '+req.params.id);
  })
  ```

- 通过params传递参数

  ```js
  axios.get('http://localhost/axios',{
      params:{id:67}
  }).then(response=>{
      console.log(response.data);
  })
  ```
	
  此时会调用第一个接口。
  
  最后的结果为：
  
  ![image-20210801150053066](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210801150100.png)



### DELETE传递参数

参数传递与GET方式类似，支持通过url或者axios的params传递参数。

以下示例通过axios的params进行传参：

```js
axios.delete('http://localhost/axios',{
    params:{
        name:'jiaqi'
    }
}).then(response=>{
    console.log(response.data);
})
```

> 接口

```js
app.delete('/axios',(req,res)=>{
    res.send('axios delete name: '+req.query.name)
})
```

### POST 请求

- 通过选项传递参数（默认传递的是json格式的数据）

  ```js
  axios.post('axios',{
      uname:'tom',
      pwd:123
  }).then(response=>{
      console.log(response.data);
  })
  ```

  > 后台接口

  ```js
  const bodyParser = require('body-parser')
  app.use(bodyParser.urlencoded({extended:false}));
  app.use(bodyParser.json());
  
  //需要使用bodyParser来解析post传递的参数
  
  app.post('/axios',(req,res)=>{
      res.send('axios post '+req.body.name+'---'+req.body.pwd);
  })
  ```

- 传递表单类型的数据

  通过[URLSearchParams](https://developer.mozilla.org/zh-CN/docs/Web/API/URLSearchParams)传递参数（application/x-www-urlencoded）

  > **`URLSearchParams`** 接口定义了一些实用的方法来处理 URL 的查询字符串。

  ```js
  var params=new URLSearchParams();
  params.append('name','zhangsan');
  params.append('pwd','12345');
  axios.post('http://localhost/axios',params).then(response=>{
      console.log(response.data);
  })
  ```

  使用的接口与前面的一样，故不赘述。

- 除了URLSearchParams，还可以通过其他方式编码，具体详见https://axios-http.com/zh/docs/urlencoded。

  ![image-20210801153632594](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210801153639.png)

### PUT请求

参数传递与POST请求相似。

```js
// PUT 请求
axios.put('http://localhost/axios/494',{
    name:'王五',
    pwd:903
}).then(response=>{
    console.log(response.data);
})
```

> 接口

```js
app.put('/axios/:id',(req,res)=>{
    res.send('axios put '+req.params.id+'-----'+req.body.name+'---'+req.body.pwd);
})
```



## 响应结构

```js
{
  // `data` 由服务器提供的响应
  data: {},

  // `status` 来自服务器响应的 HTTP 状态码
  status: 200,

  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: 'OK',

  // `headers` 是服务器响应头
  // 所有的 header 名称都是小写，而且可以使用方括号语法访问
  // 例如: `response.headers['content-type']`
  headers: {},

  // `config` 是 `axios` 请求的配置信息
  config: {},

  // `request` 是生成此响应的请求
  // 在node.js中它是最后一个ClientRequest实例 (in redirects)，
  // 在浏览器中则是 XMLHttpRequest 实例
  request: {}
}

```

当使用 then 时，将接收如下响应:

```js
axios.get('/user/12345')
  .then(function (response) {
    console.log(response.data);
    console.log(response.status);
    console.log(response.statusText);
    console.log(response.headers);
    console.log(response.config);
  });
```

## 默认配置

可以指定默认配置，它将作用于每个请求。

### 全局 axios 默认值

```js
//配置公共的请求头
axios.defaults.baseURL = 'https://api.example.com';
// 配置 超时时间
axios.defaults.timeout = 2500;
// 配置公共的请求头
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
// 配置公共的 post 的 Content-Type
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
// 设置请求头
axios.defaults.headers['mytoken']='alsjffsfjslkk';

```

使用举例


```js
// 设置请求头信息
// 需要服务器允许传递mytoken这个header
axios.defaults.headers['mytoken']='hello';
// 设置基准url地址
axios.defaults.baseURL='http://localhost/';
axios.get('/axios-json').then(response=>{
    console.log(response.data.name);
})
```

关于请求头的设置，服务器端需要允许设置某个header才行。必须要在服务器中允许名为mytoken的请求头，`    res.header("Access-Control-Allow-Headers", "mytoken");`

此时的服务器设置如下：

```js
//设置跨域请求
app.all('*', function (req, res, next) {
    //设置请求头
    //允许所有来源访问
    res.header('Access-Control-Allow-Origin', '*')
    
    res.header("Access-Control-Allow-Headers", " Origin, X-Requested-With, Content-Type, Accept");
    res.header("Access-Control-Allow-Headers", "mytoken");
    //允许访问的方式
    res.header('Access-Control-Allow-Methods', 'PUT,POST,GET,DELETE,OPTIONS')
    //修改程序信息与版本
    res.header('X-Powered-By', ' 3.2.1')
    //内容类型：如果是post请求必须指定这个属性
    res.header('Content-Type', 'application/json;charset=utf-8')
    next()
});
```

## axios拦截器

在请求或响应被 then 或 catch 处理前拦截它们。一定要记得把config或者response返回出去。

### 请求拦截器

在请求发出前的设置的一些信息。

![image-20210801163129933](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210801163129.png)

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });
```

使用举例：

```js
axios.interceptors.request.use(function(config){
    // 比如可以根据不同的url，进行不同的配置
    console.log(config.url);
    // 在请求发出前做些什么
    config.headers.mytoken='hello';
    return config;
},function(err){
    console.log(err);
});

axios.get('http://localhost/axios-json').then(function(response){
    console.log(response.data);
})
```

### 响应拦截器

在获取数据之前，对数据进行加工处理。

![image-20210801163941747](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210801163941.png)

```js
// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

使用举例：

```js
axios.interceptors.response.use(function(response){
    console.log(response);
    return response.data;
},function (error){
    return Promise.reject(error);
});
axios.get('http://localhost/axios-json').then(function (response) {
    console.log(response);
})
```

### 移除拦截器

```js
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

