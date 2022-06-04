---
title: Http 存储 ajax
date: 2021-09-13 20:49:12
tags: [Http,Ajax,存储]
---

同样是之前的笔记，上传到自己的博客，用于复习。

# HTTP

## 前后端通信

前后端通信是在“请求”和“响应”中完成的。

![image-20210621140658177](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210621140706.png)

### 概念

前端指浏览器端。客户端指能和服务器通信的平台。后端一般指服务器端。

### 前后端通信方式

1.使用浏览器访问网页

在浏览器地址栏输入网址，按下回车

2.`html`的标签

浏览器在解析HTML标签的时候，遇到一些特殊的标签，会再次向服务器发送请求。如`link`、`img` 、`script` 、 `iframe`等。

![image-20210621142033666](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210621142034.png)

还有一些标签，浏览器解析的时候，不会向服务器发送请求，但是用户可以使用他们向服务器发送请求。如a、form标签。

3.`Ajax`和` Fetch`

## HTTP

### 概念

超文本：原先一个个单一的文本，通过超链接将其联系起来。由原先的单一的文本变成了可无限延伸扩展的超级文本、立体文本。

**超文本传输协议（HTTP，hyper text transfer protocol ）**是一个用于传输超媒体文档（例如 HTML）的[应用层](https://en.wikipedia.org/wiki/Application_Layer)协议。它是为 Web 浏览器与 Web 服务器之间的通信而设计的，但也可以用于其他目的。

### `HTTP`请求过程

![image-20210621143527702](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210621143529.png)



### `HTTP`报文

浏览器向服务器发送请求时，请求本身就是信息，叫请求报文。服务器向浏览器发送响应时传输的信息，叫响应报文。`http`报文指请求报文和响应报文。

#### HTTP报文格式

![image-20210621144321261](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210621144322.png)

请求报文=请求头(起始行+首部)+请求体 (get请求没有请求体)

响应报文=响应头(起始行+首部)+响应体

GET请求，没有请求体，数据通过请求头携带；POST请求，有请求体，数据通过请求体携带。

### HTTP方法

HTTP 方法是浏览器**发送请求**时采用的方法，与响应无关。HTTP方法用来定义对于资源采用什么样的操作，有各自的语义。

get请求用于获取数据，post用于创造数据，put用于更新数据，delete用于删除数据(增删改查)。**这些方法虽然有各自的语义，但并不是强制性的。**

`RESTful`接口设计是一种接口设计风格，充分利用HTTP方法的语义。

#### GET和POST的区别

![image-20210621151809943](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210621151810.png)

1.语义：

GET：获取数据；POST：创建数据

2.发送数据

GET通过地址在请求头中携带数据，能够携带的数据量和地址的长度有关系，一般最多就几KB。

POST既可以通过地址在请求头中携带数据，也可以通过请求体携带数据，理论上能携带数据的量是无限的 。

3.缓存

GET可以被缓存，POST一般不会被缓存。

4.安全性

GET和POST都不安全，发送密码或敏感信息时不要使用GET，主要是避免直接被他人窥屏或者通过历史记录找到你的密码。

### HTTP状态码

HTTP状态码是服务器对请求的处理结果，是服务器返回的。

100~199，消息：请求已经被接受，需要继续被处理。

200~299，成功

300~399，重定向 

> 301 moved permanently
>
> 302 move temporarily
>
> 304 not modified

400~499 请求错误

500~599 服务器错误

# 本地存储

## cookie

cookie全称为HTTP cookie，简称cookie，是浏览器存储数据的一种方式。cookie是本地存储，一般会自动随着浏览器发送请求时发送到服务器端。

cookie可以跟踪统计用户访问该网站的习惯，比如访问时间，访问哪些页面，页面停留时间。

可以在控制台->application->cookies的下拉菜单中看到本地存储的cookie信息。

![image-20210621164223885](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210621164225.png)

![image-20210621164622927](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210621164624.png)

### cookie用法

```js
//1.写入cookie
document.cookie="username=zs";
document.cookie="age=19"; 
// 并不会覆盖前面的cookie，除非name、domain、path三个都相同才能覆盖
// cookie 不能一起设置，需要一个一个的设置

//2.读取cookie
//读取的是一个由名值对构成的字符串，每个名值对之间由"; "(分号+空格)隔开
console.log(document.cookie);  //会输入所有的cookie
```

### cookie注意事项

1. 前后端都可以写入和获取 Cookie

2. Cookie有数量限制，每个域名下的cookie有数量限制。当超过单个域名限制之后，再设置 Cookie，浏览器就会清除以前设置的 Cookie。

3. Cookie有大小限制。每个cookie的存储容量很小，最多只有4KB左右。

### cookie属性

#### Name和Value

cookie名称(Name)和值(Value)是cookie最重要的两个属性，创建cookie时候必须要包含，其他的属性可以使用默认值。

cookie的名称或值如果包括非英文字母，写入时需要使用`encodeURIComponent()`来编码，读取时要使用`decodeURIComponent()`来解码。

```js
//使用模板字符串
document.cookie = `username=${encodeURIComponent('张三')}`;

console.log(encodeURIComponent('张三'));
//%E5%BC%A0%E4%B8%89
```

PS：`encodeURI`和`encodeURIComponent`两者都是是对统一资源标识符（URI）进行编码的方法，它使用1到4个转义序列来表示每个字符的**`UTF-8编码`**,基本功能都是把 URI 非法字符转化成合法字符，转化后形式类似「%*」,例如`encodeURI('博') => "%E5%8D%9A" `三个字节。

`encodeURI `和 `encodeURIComponent` 的主要区别在于需要转义的字符范围不一样。`encodeURIComponent`的转义范围会更大。

#### Expires

对于失效的cookie，会被浏览器自动清除。

如果没有设置失效(到期，expire)的时间，会使用默认值`Session`，这样的cookie称之为会话cookie。它存在于内存中，当会话结束后（浏览器关闭时，不是页面关闭），cookie会消失。

自定义失效时间，可以通过设置Expires或Max-Age来实现。

1. `Expires`的值Date()对象。
2. max-age 的值为数字，表示多少秒后过期。如果max-age的值是0或者负数，cookie会被删除，可以用这种方法删除cookie。

```js
//1.Expires的值Date()对象
document.cookie=`hobby=painting;expires=${new Date("2022-01-01")}`;
//2.max-age 的值为数字，表示多少秒后过期
//如果max-age的值是0或者负数，cookie会被删除，可以用这种方法删除cookie
document.cookie="weather=sunny;max-age=5";
```

#### Domain

Domain限制了（不同域名）访问cookie的范围，使用JavaScript只能读写当前域与父域的cookie，无法读写其他域的cookie。

```js
document.cookie='username=alex;domain=www.imooc.com';
```

假如在`www.imooc.com`当前域有cookie : name=18，则在父域`imooc.com`可以看到访问到该cookie，但是在域名为`m.imooc.com`的当前域是无法访问到前述cookie。

#### Path

path限制了同一域名下访问cookie的范围。默认的path为根目录`/`。 使用JavaScript只能读写当前路径和上一级目录的cookie，无法读写下一级路径的cookie。

```js
document.cookie='username=alex;path=/info';
```

**只有当name、domain、path三个都相同时，后面的cookie才能覆盖前面的。也即只有当name、domain、path都相同时，才是同一个cookie**

#### HttpOnly

**前端不能通过`JS`去设置一个 `HttpOnly`类型的 Cookie，这种类型的 Cookie只能是后端来设置。**设置了`HttpOnly`属性的Cookie不能通过JavaScript去访问。

#### Secure安全标志

secure限定了只有在使用了`https`而不是`http`的情况下cookie才能发送到服务器。

> Domain、Path、 Secure都要满足条件，还不能过期的 Cookie才能随着请求发送到服务器端。

## 封装cookie

### 1.新增cookie

写一个set函数，调用函数时传入name，value，max-age等值来创建一个cookie。

用到了对象的解构赋值，函数的默认值等前置知识点。

```js
const set=(name,value,{maxAge,domain,path,secure}={})=>{
    //为了输入非法的字符串，此时可以用encodeURIComponent进行编码
    let cookieText=`${encodeURIComponent(name)}=${encodeURIComponent(value)}`;
    if(typeof maxAge==="number"){
        cookieText+=`; max-age=${maxAge}`;
    }
    //判断是否传了domain
    if(domain){
        cookieText+=`; domain=${domain}`;
    }
    if(path){
        cookieText+=`; path=${path}`;
    }
    if(secure){
        cookieText+=`; secure`;
    }
    document.cookie=cookieText;
};
```

调用函数

```js
set('user','alex',{maxAge:5});
set('用户名','蒂娜');
```

### 2.获取cookie

通过cookie的name属性获取对应的value值。非常机智地用有格律的分隔符将cookie字符串分割为了数组。

`document.cookie`会获取类似如下的字符串：

```js
// cookie的格式为"username=alex; age=18; gender=male"
```

```js
const get=(name)=>{
    //预防传入的中文，先用encodeURI进行编码
    name=encodeURIComponent(name);
    // 把分号+空格作为分隔符，将cookie分割为数组
    const cookies= document.cookie.split('; ');
    // ["username=alex", "age=18", "gender=male"]
    //再将上面的得到的数组用等号分割为数组
    for(let item of cookies){
        let [cookieName,cookieValue]=item.split('=');
        if(cookieName===name){
            //解码
            return decodeURIComponent(cookieValue);
        }
    }
    return;
};
```

调用封装的set和get函数：

```js
set('user','alex',{maxAge:5});
set('用户名','蒂娜');
console.log(get('用户名')); //蒂娜
console.log(get('user')); //alex
```

### 3.删除指定cookie

一条cookie得要有name、domain、path三个属性值才能确定，因此要根据name、domain、path删除cookie。此时调用了前面封装好的set函数修改某个cookie的max-age为-1。

```js
const remove=(name,{domain,path}={})=>{
    //当max-age的值为0或负数的时候会删除cookie
    set(name,"",{domain,path,maxAge:-1});
};
```

### 4.小案例

点击英文或者中文按钮，使用前述的set函数生成cookie，并用`location.reload()`刷新界面，将生成的cookie发送到服务器端。

```html
<body>
    <button id="cn">中文</button>
    <button id="en">英文</button>
    <script type="module">
        import {set} from './3.cookie.js';
        // 使用封装好的cookie来实现网站语言切换
        const cnBtn=document.getElementById('cn');
        const enBtn=document.getElementById('en');
        cnBtn.addEventListener('click',()=>{
            // 新增一个cookie，保存7天
            set('language','cn',{maxAge:3600*24*7});
            //强制刷新页面,让生成的cookie发送当服务器端
            location.reload();
        },false);
        enBtn.addEventListener('click',()=>{
            set('language','en',{maxAge:3600*24*7});
            location.reload();
        },false);
    </script>
</body>
```

## localStorage

`localStorage`也是一种浏览器存储数据的方式（本地存储），它只是存储在本地，不会发送到服务器端。而cookie会随着浏览器向服务器发送请求时发送到服务器。`localStorage` 中的键值对总是以字符串的形式存储。

单个域名下的`localStorage`总大小有限制。

### localStorage用法



```js
//setItem()
localStorage.setItem('username','alex');
localStorage.setItem('age',18);
console.log(localStorage);
// {age: "18", username: "alex", length: 2}

//getItem()
// 获取不存在的返回null
console.log(localStorage.getItem('username'));//alex
console.log(localStorage.getItem('gender')); //null

//removeItem()
//删除不存在的key，不报错
localStorage.removeItem('username');
console.log(localStorage);

//clear()
//删除所有的
localStorage.clear();
```

### localStorage应用

使用`localStorage`实现用户名自动填充的功能。

HTML

```html
<form action="https://www.imooc.com" id="login" method="POST">
    <input type="text" name="username">
    <input type="password" name="password">
    <input type="submit" id="btn" value="登录">
</form>
```

JavaScript注意点：通过form表单的`dom.name`属性，获取表单内的标签。

```js
const loginForm=document.getElementById('login');
const btn=document.getElementById('btn');
btn.addEventListener('click',e=>{
    // 1.阻止它使用默认的提交，因为先要验证数据
    e.preventDefault();
    // 2.数据验证 code省略
    console.log(loginForm.username);
    // 3.把数据存在localStorage中
    //可以通过form表单的dom元素.name属性，获取表单内的标签
    // loginForm.username 得到的是<input type="text" name="username">
    // 再通过loginForm.username.value就可以获取用户输入的值
    localStorage.setItem('username',loginForm.username.value);
    // 4.手动提交表单
    loginForm.submit();
},false);
const username=localStorage.getItem('username');
// 如果从localStorage中取到了username的值，则填充表单
if(username){
    loginForm.username.value=username;
}
```

### localStorage注意事项

1. `localStorage`的存储期限

    `localStorage`是持久化本地存储，除非手动清除（比如通过JavaScript删除，或者清除浏览器缓存），否则数据是永远不会过期的。

   但是当会话结束（比如关闭浏览器）的时候， `sessionStorage`中的数据会被清空，它的方法和`localStorage`完全一样。

2. `localStorage`键和值的类型

   `localStorage`存储的键和值只能是**字符串类型**，**如果不是字符串类型，也会先转化成字符串类型再存进去。**

3. 不同域名下能否共用 `localStorage`

   不同的域名是不能共用 `localStorage`的。

4. `localStorage`的兼容性

   IE 7及以下版本不支持 `localStorage`， lE 8开始支持，可以在[Can I use](https://caniuse.com/)这个网站上查询。

   

# Ajax

## 基础内容

### 1.基本概念

Ajax是指Asynchronous JavaScript + XML（异步JavaScript和XML）。Ajax是浏览器与服务器之间的异步通信方式，使用Ajax可以在不重新加载整个页面的情况下，对页面的某部分进行更新。

Ajax中的异步：可以异步地向服务器发送请求，在等待响应的过程中，不会阻塞当前页面，浏览器可以做自己的事情。直到成功获取响应后，浏览器才开始处理响应数据。

XML（可扩展标记语言，Extensible Markup Language）是前后端数据通信时传输数据的—种格式。

尽管X在Ajax中代表XML，但由于[JSON](https://developer.mozilla.org/zh-CN/docs/Glossary/JSON)的许多优势，比如更加轻量以及作为Javascript的一部分，目前JSON的使用比XML更加普遍。JSON和XML都被用于在Ajax模型中打包信息。

Ajax需要服务器环境，非服务器环境下，很多浏览器无法正常使用 Ajax。非常简单的一种方式就是在vscode 中安装live server插件。

### 2. Ajax基本用法

Ajax想要实现浏览器和服务器之间的异步通信，需要依靠`XMLHttpRequest`，这是一个构造函数。

Note： 不论是`XMLHttpRequest`，还是Ajax，都没有和具体的某种数据格式绑定。

发送一个 HTTP 请求，需要创建一个 `XMLHttpRequest` 对象，打开一个 URL，最后发送请求。**当所有这些事务完成后，该对象将会包含一些诸如响应主体或 [HTTP status](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) 的有用信息。**

#### 1.创建xhr对象

```js
const xhr=new XMLHttpRequest();
```

#### 2.打开一个地址

 调用open并不会发送请求，只是做好了发送请求的准备准备工作。

```js
xhr.open('HTTP方法',`地址`,'是否使用异步发送请求');
```

####  3.发送请求 

send()的参数时通过请求体来携带的，而get方法没有请求体(因此send的参数为空或者null)，post方法有请求体。

```js
xhr.send("参数");
```

#### 4.监听事件，处理响应

当获取到响应后，会触发xhr对象的`readystatechange`事件，可以在该事件中对响应进行处理。

NOTE：为了兼容性，一般把第4步放在第1步的后面。

`readystatechange`可以监听`readyState`状态的变化，它的值从0~4，一共5个状。

> 0：未初始化。尚未调用open()
>
> 1：启动。已经调用open()，但尚未调用send()
>
> 2：发送。已经调用send()，但尚未接收到响应
>
> 3：接收。已经接收到部分响应数据
>
> 4：完成。已经接收到全部响应数据，而且已经可以在浏览器中使用了

```js
xhr.onreadystatechange=()=>{
    if(xhr.readState!==4)
    return;

    // 判断HTTP 状态码
    // 获取到响应后，响应的内容会自动填充到xhr对象的属性中
    // ps:http code为304表示not modified 使用缓存
    if((xhr.status>=200&&xhr.status<300)||xhr.status===304){
        console.log(xhr.responseText);
    }
};
```

`xhr.status`表示HTTP状态码，`xhr.statusText`表示状态码的说明，`xhr.reponseText`表示返回的响应主体。

### 3.使用Ajax完成前后端通信

```js
const url='https://www.imooc.com/api/http/search/suggest?words=js';
const xhr=new XMLHttpRequest();

xhr.onreadystatechange=()=>{
    if(xhr.readyState!==4) return;
   
    if((xhr.status>=200&&xhr.status<300)||xhr.status===304){
        console.log(xhr.status,xhr.statusText);
        console.log(xhr.responseText, typeof xhr.responseText);
    }
}

xhr.open('GET',url,true);

xhr.send(null);
```

输出的结果：

![image-20210622140552550](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210622140553.png)

#### 4. GET请求

GET请求不能通过请求体来携带数据，但是可以通过请求头来携带。如通过`?words=js&age=18&name=alex`这种问号+名值对的形式。

```js
 const url = 'https://www.imooc.com/api/http/search/suggest?words=js&age=18&name=alex';
```

![image-20210622142043924](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210622142044.png)

如果携带的数据是非英文字母的话，比如说汉字，就需要编码之后再发送给后端，不然会造成乱码问题可以使用 `encodeURIComponent()`编码。

```js
const url = `https://www.imooc.com/api/http/search/suggest?words=${encodeURIComponent('前端')}`;
```

### 5. POST请求

POST请求通过请求体来携带数据，同时也可以通过请求头携带。

![image-20210622143455252](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210622143456.png)

```js
// POST请求通过请求体来携带数据，同时也可以通过请求头携带。
const url = `https://www.imooc.com/api/http/search/suggest?words=${encodeURIComponent('前端')}`;
const xhr = new XMLHttpRequest();
xhr.onreadystatechange = () => {
    if (xhr.readyState !== 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.status, xhr.statusText);
        console.log(xhr.responseText, typeof xhr.responseText);
    }
}
xhr.open('POST', url, true);
// 通过请求体传输数据
xhr.send("username=alex&age=18");
```

## JSON

JSON是Ajax发送和接收数据的一种格式，全称是JavaScript Object Notation。

JSON有3种形式，每种形式的写法都和JavaScript中的数据类型很像，可以很轻松的和JavaScript中的数据类型互相转换。

### 3种形式

1.简单值类型

JSON的简单值形式对应着JavaScript中基础数据类型，如数字、字符串、布尔值、null。

> ①JSON中没有 undefined值
> ②JSON中的字符串必须使用双引号
> ③JSON中是不能注释的

2.对象形式

JSON的对象形式就对应着JavaScript中的对象。

> JSON中对象的属性名必须用双引号，属性值如果是字符串也必须用双引号。JSON中只要涉及到字符串，就必须使用双引号。不支持 undefined。

3.数组形式

JSON的数组形式对应着JavaScript中的数组。

> 注意事项数组中的字符串必须用双引号，JSON中只要涉及到字符串，就必须使用双引号。不支持 undefined。

### JSON常用方法

#### `JSON.parse()`

`JSON.parse()`可以将JSON格式的字符串解析成JavaScript中对应的值。注意，只能解析合法的JSON字符串，否则会报错。

```diff
const url="https://www.imooc.com/api/http/search/suggest?words=js";
const xhr=new XMLHttpRequest();
xhr.onreadystatechange=()=>{
    if(xhr.readyState!==4) return;
    if((xhr.status>=200&&xhr.status<300)||xhr.status===304){
+       let text=xhr.responseText;
        console.log(JSON.parse(text));
        //会输出JavaScript对应的数据类型
    }
}
xhr.open('GET','11.data.json',true);
xhr.send();
```

#### `JSON.stringify()`

`JSON.stringify()`可以将JavaScript中基本数据类型、对象、数组转换为JSON格式的字符串。

```js
const url="https://www.imooc.com/api/http/search/suggest?words=js";
const xhr=new XMLHttpRequest();
xhr.onreadystatechange=()=>{
    if(xhr.readyState!==4) return;
    if((xhr.status>=200&&xhr.status<300)||xhr.status===304){
        let text=xhr.responseText;
        console.log(JSON.parse(text));
    }
}
xhr.open('POST',url,true);
let text=JSON.stringify({"name":"alex","age":18});
xhr.send(text);
```



![image-20210622153303588](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210622153304.png)

### JSON应用

用`JSON.parse()`和`JSON.stringify`封装一个`localStorage`调用的函数。

```js
// 1.新添localStorage
const set=(key,value)=>{
    localStorage.setItem(key,JSON.stringify(value));
}

// 2.获取localStorage
const get=key=>{
    let value=JSON.parse(localStorage.getItem(key));
    return value;
}

// 3.删除指定localStorage
const remove=key=>{
    localStorage.removeItem(key);
}

// 4.删除全部的localStorage
const clear=()=>{
    localStorage.clear();
}
```

## 跨域

向—个域发送请求，如果要请求的域和当前域是不同域，就叫跨域不同域之间的请求，就是跨域请求。

```html
https(协议):// www.imooc.com(域名):443(端口号)/course/list(路径)
```

协议、域名、端口号三个部分只要有一个不同就是不同域。**与路径无关**。

浏览器一般会阻止跨域请求，阻止跨域请求，其实是浏览器的一种安全策略（同源策略）。

### 跨域解决方案

优先使用CORS跨域资源共享，如果浏览器不支持CORS的话，再使用 JSONP。

#### ①CORS跨域资源共享

`Access-Control-Allow-Origin: *`，表明允许所有的域名跨域请求它，*是通配符，没有任何限制。

`Access-Control-Allow-Origin: http://127.0.0.1:5500`，只允许特定的域名跨域请求。

![image-20210622165409671](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210622165410.png)

使用CORS跨域的过程：①浏览器发送跨域请求②后端在响应头中添加`Access-Control-Allow-Origin`的头信息③浏览器接收到响应④如果是同域下的请求，浏览器，不需要额外做什么，前后端可以进行通信。⑤如果是跨域请求，浏览器会从响应头中查找是否运行跨域访问，如果允许跨域，通信圆满完成，如果不允许跨域，就丢弃响应结果。

②JSONP

`script`标签跨域不会被浏览器阻止，`JSONP`主要就是利用`script`标签加载跨域文件。

首先服务器端要准备好JSONP的接口

```js
https://www.imooc.com/api/http/jsonp?callback=handleResponse
```

![image-20210622171826516](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210622171827.png)

上面链接对应着一个函数的调用，函数名`handleResponse`可以通过链接自定义。

手动加载JSONP的接口：

```html
// 先创建一个函数
<script>
    const handleResponse=data=>{
        console.log(data);
    };
</script>
// 调用上面的函数
<script src="https://www.imooc.com/api/http/jsonp?callback=handleResponse"></script>
```

动态加载：

```js
const handleResponse=data=>{
    console.log(data);
};
// 动态加载JSONP接口
const script=document.createElement('script');
script.src="https://www.imooc.com/api/http/jsonp?callback=handleResponse";
document.body.appendChild(script);
```

## XHR

下文所指的`XHR`指的 是构造函数`XMLHttpRequest`构造函数创建的对象。

```js
const xhr = new XMLHttpRequest();

```

### XHR的属性

1.`responseType和response`属性

`responseText`只能在没有设置`responseType`或者`responseType=""或者"text"`的时候才能使用。

```js
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';
const xhr = new XMLHttpRequest();
+ xhr.responseType="json";
xhr.onreadystatechange = () => {
    if (xhr.readyState !== 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.response, typeof xhr.response);
    }
}
xhr.open('GET', url, true);
xhr.send(null);
```

2.`timeout`属性

timeout 用来设置请求的超时时间(单位ms)，在`xhr.send()`之前添加timeout属性。

```js
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';
const xhr = new XMLHttpRequest();
xhr.responseType="json";
xhr.onreadystatechange = () => {
    if (xhr.readyState !== 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.response, typeof xhr.response);
    }
}
xhr.open('GET', url, true);
xhr.timeout=10000;
// 之后会学timeout事件
xhr.send(null);
```

3.`withCredentials`属性

`withCredentials`属性用来指定使用Ajax发送请求时是否携带cookie。使用Ajax发送请求时，默认情况下同域下会携带cookie，跨域时不会携带cookie。

```js
xhr.withCredentials=true;
```

最终能否成功跨域携带cookie，还得看服务器`Access-Control-Allow-Origin`的具体设置。

### XHR的方法

![image-20210622175213382](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210622175214.png)

1.`abort()`方法

`abort()`方法用来终止当前请求，一般配合abort事件一起使用。需要在调用send()发送请求后调用。

2.`setRequestHeader()`

`setRequestHeader()`可以用来设置请求头信息。这个方法需要放在open()后，send()调用前。

```js
setRequestHeader(头部字段的名称，头部字段的值)
```

```js
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';
const xhr = new XMLHttpRequest();
xhr.onreadystatechange = () => {
    if (xhr.readyState !== 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.status, xhr.statusText);
        console.log(xhr.responseText, typeof xhr.responseText);
    }
}
// 一般配合POST方法，而且send()有参数
xhr.open('POST', url, true);

// 请求头中的Content-Type字段用来告诉服务器，浏览器发送的数据是什么格式的 ,可以用来模拟表单提交
xhr.setRequestHeader('Content-Type',"application/x-www-form-urlencoded");

xhr.send('username=alex&age=18');
// xhr.abort();
```

JSON 的Content-Type为如下：

```js
xhr.setRequestHeader('Content-Type',"application/json")
```

Content-Type字段用来告诉服务器，浏览器发送的数据是什么格式的 ,可以用来模拟表单提交。

```html
  <form action="https://www.imooc.com/api/http/search/suggest?words=js" method="POST" id="login" >
<input type="text" name="username" placeholder="用户名">
<input type="password" name="password" placeholder="密码">
<input type="submit"  id="submit" value="登录">
</form>
<script>
    const url="https://www.imooc.com/api/http/search/suggest?words=js";
    // 1.使用ajax提交表单
    const login =document.getElementById('login');
    //console.log(login.username);
    const {username,password}=login;
    const btn=document.getElementById('submit');
    btn.addEventListener('click',e=>{
        e.preventDefault();
        // 表单数据验证 code省略

        // 发送ajax请求
        const xhr=new XMLHttpRequest();
        xhr.open('POST',url,true);
        // 这个非常关键
        xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
        xhr.send(`username=${username.value}&possword=${password.value}`);
        xhr.addEventListener('load',()=>{
            if((xhr.status>=200&&xhr.status<300||xhr.status===304)){
                console.log(xhr.response);  
            }
        },false)
    },false);
</script>
</body>
```





### XHR事件

![image-20210622183227814](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210622183234.png)

1.`load`事件

当响应数据**可用**的时候触发load事件。可以用load事件来替代`readstatechange`事件。

```js
xhr.onreadystatechange = () => {
    if (xhr.readyState !== 4) return;
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.responseText);
    }
}
//可以用下面的来替代
xhr.onload=()=>{
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.responseText);
    }
}
```

2.`error`事件

当**请求**发生错误的时候，会触发`error`事件。

```js
const url = 'https://www.imooc.pp'; // 网址错了
const xhr = new XMLHttpRequest();
xhr.onload = () => {
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.response);
    }
}
xhr.addEventListener('error',()=>++ {console.log('error'),false})
xhr.open('GET', url, true);
xhr.send(null);
```

3.`abort`事件

当调用abort()函数终止请求的时候触发。

```js
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';
const xhr = new XMLHttpRequest();
xhr.onload = () => {
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.response);
    }
}
xhr.addEventListener('abort',()=>{console.log("abort");})
xhr.open('GET', url, true);
xhr.send(null);
xhr.abort();
```

4.`timeout`事件

```js
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';
const xhr = new XMLHttpRequest();
xhr.onload = () => {
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
        console.log(xhr.response);
    }
}

xhr.addEventListener('timeout',()=>{console.log('timeout');},false);
xhr.open('GET', url, true);
xhr.timeout=10;
xhr.send(null);
```

## 进阶内容

FormData

通过HTML的表单元素创建FormData对象。

```js
// 用FormData组装数据 通过for of遍历可以看到其中的内容
const data=new FormData(某个form元素);
// 还可以通过append()方法添加其他数据
data.append('age',18);

// 有了FormData后就不需要自己添加content-type了
//xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');

// send的参数可以直接写FormData 创建的对象
xhr.send(data);
```



```html
<form action="https://www.imooc.com/api/http/search/suggest?words=js" method="POST" id="login" >
<input type="text" name="username" placeholder="用户名">
<input type="password" name="password" placeholder="密码">
<input type="submit"  id="submit" value="登录">
</form>
<script>
    const url="https://www.imooc.com/api/http/search/suggest?words=js";
    // 1.使用ajax提交表单
    const login =document.getElementById('login');
    //console.log(login.username);
    const {username,password}=login;
    const btn=document.getElementById('submit');
    btn.addEventListener('click',e=>{
        e.preventDefault();
        // 表单数据验证 code省略
        const xhr=new XMLHttpRequest();
        xhr.open('POST',url,true);
        
       
        // 组装数据 FormData 通过for of遍历可以看到其中的内容
       const data=new FormData(login);
       // 可以省略这些`username=${username.value}&possword=${password.value}`
       
        // 有了FormData后就不需要自己添加content-type了
        //xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
       
       xhr.send(data);
        xhr.addEventListener('load',()=>{
            if((xhr.status>=200&&xhr.status<300||xhr.status===304)){
                console.log(xhr.response);  
            }
        },false)
    },false);
</script>
```

### 封装ajax
