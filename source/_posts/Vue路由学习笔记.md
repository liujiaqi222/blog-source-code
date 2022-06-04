---
title: Vue路由学习笔记
date: 2021-08-02 12:03:51
tags: [Vue,Vue路由]
---

笔记基于：https://router.vuejs.org/zh/ 和黑马视频

# 路由的基本概念与原理

## 路由

### 后端路由

- 概念：根据不同的用户URL请求，返回不同的内容
- 本质：URL**请求地址**与**服务器资源**之问的对应关系

![image-20210802121213464](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210802121213.png)

### SPA

- 后端渲染（存在性能问题，假如用户频繁地提交表单，则会造成页面频繁刷新）。
- Ajax前端渲染（前端渲染提高性能，但是不支持浏览器的前进后退操作）。
- SPA（ Single Page Application）单页面应用程序：整个网站只有一个页面，内容的变化通过Ajax局部更新实现、同时支持浏览器地址栏的前进和后退操作。
- SPA实现原理之一：基于URL地址的hash（hash的变化会导致浏览器记录访问历史的变化、但是hash的变化不会触发新的URL请求）。
- 在实现SPA过程中，最核心的技术点就是**前端路由**。

### 前端路由

- 概念：根据不同的**用户事件**，显示不同的页面内容。
- 本质：**用户事件**与**事件处理函数**之间的对应关系。

![image-20210802122059780](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210802122059.png)

### 前端路由的工作方式

① 用户点击了页面上的路由链接

② 导致了 URL 地址栏中的 Hash 值发生了变化

③ 前端路由监听了到 Hash 地址的变化

④ 前端路由把当前 Hash 地址对应的组件渲染都浏览器中  

结论：前端路由，指的是 **Hash 地址**与**组件**之间的对应关系！  

## 实现简单的前端路由

基于URL中的hash实现（点击菜单的时候改变URL的hash，根据hash的变化控制组件的切换）

```js
// 监听window的onhashchange事件，根据最新的hash值，切换要显示的组件名称
window.onhashchange=function(){
    //通过location.hash 获取到最新的hash值
}
```

实现的效果：

根据`location.hash`的值切换页面显示的内容

![动1](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210802135422.gif)

```html
<div id="app">
    <!-- 切换组件的超链接 -->
    <a href="#/zhuye">主页</a>
    <a href="#/keji">科技</a>
    <a href="#/caijing">财经</a>
    <a href="#/yule">娱乐</a>
    <!-- 根据is属性的指定的组件名称，把对应的组件渲染到component标签所在的位置 -->
    <!-- 可以把component标签看做为组件的占位符 -->
    <component :is="comName"></component>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            comName: 'zhuye'
        },
        // 注册私有组件
        components: {
            zhuye: {
                template: `<h1>主页信息</h1>`,
            },
            keji: {
                template: `<h1>科技信息</h1>`
            },
            caijing: {
                template: `<h1>财经信息</h1>`
            },
            yule: {
                template: `<h1>娱乐信息</h1>`
            }
        }
    });
    window.onhashchange = function () {
        // 通过location.hash 获取到最新的hash值
        // 根据hash值切换组件
        console.log(location.hash);
        let list = ['zhuye', 'keji', 'caijing', 'yule'];
        list.some(item => {
            if (location.hash.includes(item)) {
                vm.comName = item;
                return true;
            }
        })
    }
</script>
```

### 补充知识点

1. `location.hash`

   [`Location`](https://developer.mozilla.org/zh-CN/docs/Web/API/Location) 接口的 **`hash`** 属性返回一个 [`USVString`](https://developer.mozilla.org/zh-CN/docs/Web/API/USVString)，其中会包含URL标识中的 `'#'` 和 后面URL片段标识符。

   ```html
   <a id="myAnchor" href="/en-US/docs/Location.href#Examples">Examples</a>
   <script>
     var anchor = document.getElementById("myAnchor");
     console.log(anchor.hash); // 返回'#Examples'
   </script>
   ```

2. 关于url对象，[在这篇里](https://jiaqicoder.com/2021/08/02/%E7%BD%91%E7%BB%9C%E8%AF%B7%E6%B1%82%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/#URL%E5%AF%B9%E8%B1%A1)写了很多。



# VUE Router

Vue Router 是 [Vue.js (opens new window)](http://cn.vuejs.org/)官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。包含的功能有：

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于 Vue.js 过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的 CSS class 的链接
- HTML5 历史模式或 hash 模式，在 IE9 中自动降级
- 自定义的滚动条行为

vue-router 目前有 3.x 的版本和 4.x 的版本。其中：

- vue-router 3.x 只能结合 vue2 进行使用
-  vue-router 4.x 只能结合 vue3 进行使用
- vue-router 3.x 的官方文档地址：https://router.vuejs.org/zh/
- vue-router 4.x 的官方文档地址：https://next.router.vuejs.org/  

## 使用步骤

1. 引入相关的库文件

   ```js
   // 先导入vue再导入vue router
   <script src="/path/to/vue.js"></script>
   <script src="/path/to/vue-router.js"></script>
   ```

2. 添加路由链接

   ```html
   
   <div id="app">
       <!-- router-link 是vue中提供的标签，默认会被渲染为 a 标签 -->
       <!-- to 属性默认会被渲染为 href属性 -->
       <!-- to 属性的值默认会被渲染为 # 开头的 hash 地址 -->
       <router-link to='/user'>User</router-link>
       <router-link to='/register'>Register</router-link>
   </div>
   ```

3. 添加路由填充位

   ```html
   <!-- 路由填充位（也叫路由占位符） -->
   <!-- 通过路由规则匹配到的组件，会被渲染到router-view所在的位置 -->
   <router-view></router-view>
   ```

4. 定义路由组件

   ```js
   const User={
       template:`<h1>user 组件</h1>`
   }
   const Register={
       template:`<h1>register</h1>`
   }
   ```

5. 创建路由实例并配置路由规则

   ```js
   // 创建路由实例对象
   const router = new VueRouter({
       // routes是路由规则数组
       routes:[
           // 每一个路由规则都是一个配置对象，其中至少包括 path 和 component 两个属性：
           // path 表示当前路由规则匹配到的hash地址
           // component 表示当前路由规则要展示的组件
           {path:'/user',component:User},
           {path:'/register',component:Register},
       ]
   })
   ```

6. 把路由挂载到Vue根实例中

   ```js
   new Vue({
       el: "#app",
       data: {},
       // 挂载路由实例对象
       router
   })
   ```

最终代码：

   ```html
   <!DOCTYPE html>
   <html lang="en">
   
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
       <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
       <title>Document</title>
   </head>
   
   <body>
       <div id="app">
           <!-- router-link 是vue中提供的标签，默认会被渲染为 a 标签 -->
           <!-- to 属性默认会被渲染为 href属性 -->
           <!-- to 属性的值默认会被渲染为 # 开头的 hash 地址 -->
           <router-link to='/user'>User</router-link>
           <router-link to='/register'>Register</router-link>
           <!-- 路由填充位（也叫路由占位符） -->
           <!-- 通过路由规则匹配到的组件，会被渲染到router-view所在的位置 -->
           <router-view></router-view>
   
       </div>
   
       <script>
           const User = {
               template: `<h1>user 组件</h1>`
           }
           const Register = {
               template: `<h1>register</h1>`
           }
   
           // 创建路由实例对象
           const router = new VueRouter({
               // routes是路由规则数组
               routes: [
                   // 每一个路由规则都是一个配置对象，其中至少包括 path 和 component 两个属性：
                   // path 表示当前路由规则匹配到的hash地址
                   // component 表示当前路由规则要展示的组件
                   { path: '/user', component: User },
                   { path: '/register', component: Register },
               ]
           })
           new Vue({
               el: "#app",
               data: {},
               // 挂载路由实例对象
               router
           })
   
       </script>
   </body>
   
   </html>
   ```

 ![动](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210803112827.gif)

## 路由重定向

路由重定向：用户在访问地址A的时候，强制用户跳转到地址C，从而展示特定的组件页面。

通过路由规则的 redirect属性，指定个新的路由地址，可以很方便地设置路由的重定向。

基于先前的代码，实现当用户打开页面时，页面就跳转到'/user'。

```js
const router = new VueRouter({
    // routes是路由规则数组
    routes: [
        // path 表示需要被重定向的原地址， redirect表示将要被重定向的新地址
        {path:'/',redirect:'/user'},
        { path: '/user', component: User },
        { path: '/register', component: Register },
    ]
})
```

## 嵌套路由

### 嵌套路由功能分析

- 点击父级路由链接显示模板内容
- 模板内容中又有子级路由链接
- 点击子级路由链接显示子级模板内容

![image-20210803120119722](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210803120119.png)

### 具体实现

1.父路由组件模板

```html
<div id="app">
    <router-link to='/user'>用户</router-link>
    <router-link to='/register'>注册</router-link>
    <!-- 路由填充位 -->
    <router-view></router-view>
</div>
```

2.子路由模板

- 子路由链接
- 子路由填充位置

```js
const Register = {
    template: `
    <div>
        <h1>登录</h1>
        <hr/>
	<!--子路由链接-->
        <router-link to='/register/tab1'>tab1</router-link>
        <router-link to='/register/tab2'>tab2</router-link>
        <!-- 子路由填充位置 -->
        <router-view></router-view>
    </div>
    `
};
```

3.父路由通过children属性配置子级路由，children**数组**表示子路由规则。

```js
const router = new VueRouter({
    // 定义路由规则
    routes: [
        // 重定向 当用户打开页面时，定位到user组件
        { path: '/', redirect: '/user' },
        { path: '/user', component: User },
        {
            path: '/register',
            component: Register,
            // 通过children属性，为/register添加子路由规则
            children:[
                {path:'/register/tab1',component:Tab1},
                {path:'/register/tab2',component:Tab2}
            ]
        }
    ]
})
```

![动2](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210803123419.gif)

全部代码见：http://jsrun.net/PU8Kp/edit

## 动态路由匹配

通过动态路由参数的模式进行路由匹配

在路由规则中，要配置以冒号开头的动态参数

```js
const router = new VueRouter({
    // routes是路由规则数组
    routes: [
        // 动态路径参数，以冒号开头
        { path: '/user/:id', component: User }

    ]
})
```

### 直接通过params获取参数

在路由组件中，可以通过`$route.params`获取路由参数。

```js
const User = {
    // 路由组件中通过$route.params获取路由参数
    template: `<h1>user 组件---{{$route.params.id}}</h1>`
}
```

### 通过props传参

`$route`与对应路由形成高度耦合，不够灵活。所以，可以使用 `props`将组件和路由解耦。

1. 路由规则中`props`的值为布尔值

   ```js
   const router = new VueRouter({
               // routes是路由规则数组
               routes: [
                   // 如果props设置为true，route.params将会被设置为组件的属性
                   { path: '/user/:id', component: User,props:true},
               ]
           })
   ```

   ```js
   const User = {
       props:['id'], 
       // 使用 props 接收路由参数
       // 当然也可以继续使用$route.params.id
       template: `<h1>user 组件---{{id}}--{{$route.params.id}}</h1>`
   }
   ```

2. 路由规则中`props`的值是对象类型

   如果 props是一个对象，它会被按原样设置为组件属性，此时路径中的id已经不能访问了。（如果props设置为true，`route.params`才会被设置为组件的属性）

   ```js
   // 创建路由实例对象
   const router = new VueRouter({
       // routes是路由规则数组
       routes: [
           // 如果 props是一个对象，它会被按原样设置为组件属性
           { path: '/user/:id', component: User,props:{uname:'lisi',age:20}},
       ]
   })
   ```

   ```js
   const User = {
       props: ['id', 'uname', 'age'],
       // 此时的id并没有传值，需要使用$route.params.id才行
       template: `<h1>user 组件---Id：{{id}}--id：{{$route.params.id}}--{{uname}}--{{age}}</h1>`
   }
   ```

   最终效果：

   ![image-20210803134627526](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210803134627.png)

3. `props`的值为函数类型

   形参route的值等于 `route.params`，即path中的动态参数。

   ![image-20210803214818700](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210803214818.png)

   ```js
   const User = {
       props: ['id', 'uname', 'age'],
       template: `<h1>user 组件---Id：{{id}}--id：{{$route.params.id}}--{{uname}}--{{age}}</h1>`
   }
   ```

   ```js
   // 创建路由实例对象
   const router = new VueRouter({
       // routes是路由规则数组
       routes: [
           // 如果 props是一个对象，它会被按原样设置为组件属性
           {
               path: '/user/:id',
               component: User,
               props: (route) => { return {uname:'zhangsan',age:20,id:route.params.id} }
           },
       ]
   })
   ```

![image-20210803214603161](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210803214603.png)

## 命名路由

为了更方便的表示路由的路径，可以给路由规则起一个别名，即为“命名路由”。

注意：在to前面需要加上冒号:

```js
const router = new VueRouter({
    // routes是路由规则数组
    routes: [
        {
            // 命名路由
            name:'user',
            path: '/user/:id',
            component: User,
        },
    ]
})
```

```html
<div id="app">
    <router-link :to="{name:'user',params:{id:123}}">User1</router-link>
    <!-- 就相当于 -->
    <router-link to="/user/123">User2</router-link>
    <router-view></router-view>
</div>
```

## 编程式导航

声明式导航：通过点击链接实现导航的方式，叫做声明式导航
例如:普通网页中的`<a> </a>`链接或`vue`中的`<router-link> </router-link>`

编程式导航:通过调用JavaScript形式的API实现导航的方式，叫做编程式导航
例如:普通网页中的`location.href`。

`vue`中常见的编程式导航：

- `this.$route.push('hash地址')`

  **注意：在 `Vue` 实例内部，你可以通过 `$router` 访问路由实例。因此你可以调用 `this.$router.push`。**

  想要导航到不同的 URL，则使用 `router.push` 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

  **注意：如果提供了 `path`，`params` 会被忽略，上述例子中的 `query` 并不属于这种情况。**

  ```js
  // 字符串
  router.push('/home')
  
  // 对象
  router.push({ path: '/home' })
  
  // 命名的路由
  router.push({ name: 'user', params: { userId: '123' }})
  
  // 带查询参数，变成 /register?plan=private
  router.push({ path: 'register', query: { plan: 'private' }})
  ```

- `this.$router.go(n)`

  这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 `window.history.go(n)`。

```js
const User = {
    props: ['id', 'uname', 'age'],
    template: `<div>
        <h1>user组件-- 用户id为：{{id}}--姓名：{{uname}}--年龄为:{{age}}</h1>
        <button @click='goRegister'>跳转到register页面</button>
        </div>`,
    methods: {
        goRegister(){
            //跳转到注册页面
            this.$router.push('/register');
        }
    },
}
const Register = {
    template: `<div>
        <h1>register</h1>
        <button @click='goback'>回退</button>
        </div>`,
    methods: {
        goback(){
            this.$router.go(-1);
        }
    },
}

// 创建路由实例对象
const router = new VueRouter({
    // routes是路由规则数组
    routes: [
        { path: '/user/:id', component: User, props: route => ({ id: route.params.id, uname: 'jiaqicoder', age: 22 }) },
        { path: '/register', component: Register },
    ]
})
```

# Vue-Router小案例

根据项目的整体布局划分好组件结构，通过路由导航控制组件的显示。

1.抽离并渲染 App根组件

2.将左侧菜单改造为路由链接

3.创建左侧菜 单对应的路由组件

4.在右侧主体区 域添加路由占位符

5.添加子路由规则

6.通过路由重定向默认渲染用户组件

7.渲染用户列表数据

8.编程式导航跳转到用户详情页

9.实现后退功能

素材代码：

http://jsrun.net/t98Kp/edit

最终效果：

![动23](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210804125047.gif)

实现的代码：(省略了css)

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <title>基于vue-router的案例</title>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>

<body>
  <!-- 被vue实例所控制的区域 -->
  <div id="app">
    <!-- 路由占位符 -->
    <router-view></router-view>
  </div>
  <table>
    <tr>
      <th>id</th>
      <th>name</th>
      <th>age</th>
    </tr>
  </table>
  <script>
    // 定义app根组件
    const App = {
      template: `
          <div>
      <!-- 头部区域 -->
      <header class="header">后台管理系统</header>
      <!-- 中间主体区域 -->
      <div class="main">
        <!-- 左侧菜单栏 -->
        <div class="content left">
          <ul>
            <li><router-link to='/users'>用户管理</router-link></li>
            <li><router-link to='/rights'>权限管理</router-link></li>
            <li><router-link to='/goods'>商品管理</router-link></li>
            <li><router-link to='/orders'>订单管理</router-link></li>
            <li><router-link to='/settings'>系统设置</router-link></li>
          </ul>
        </div>
        <!-- 右侧内容区域 -->
        <div class="content right"><div class="main-content"><router-view></router-view>
</div></div>
      </div>
      <!-- 尾部区域 -->
      <footer class="footer">版权信息</footer>
    </div>
          `,
    };
    const UserInfo={
      template:`<div>
        <h5>用户详情页--id:{{id}}---{{$route.params.id}}</h5>
        <button @click='goBack'>后退</button>
      </div>`,
      props:['id'],
      methods: {
        goBack(){
          this.$router.go(-1);
        }
      },
    }
    
    const Users = {
      template: `
      <div>
        <h3>用户管理</h3>
        <table>
          <tr>
              <th>id</th>
              <th>name</th>
              <th>age</th>
              <th>操作</th>
          </tr>
          <tr v-for='item in userlist ':key="item.id">
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
            <td>{{item.age}}</td>
            <td>
              <a href='javascript:;' @click='goDetail(item.id)'>详情</a>
            </td>
          </tr>
        </table>
      </div>`,
      methods: {
        goDetail(id){
          this.$router.push('/userinfo/'+id)
        }
      },
      data() {
        return {
          userlist: [
            { id: 1, name: '张三', age: 30 },
            { id: 2, name: '张四', age: 25 },
            { id: 3, name: '张五', age: 47 },
            { id: 4, name: '张六', age: 87 }
          ]
        }
      }
    };
    const Rights = {
      template: `<div><h3>权限管理</h3></div>`,
    };
    const Goods = {
      template: `<div><h3>商品管理</h3></div>`,
    };
    const Orders = {
      template: `<div><h3>订单管理</h3></div>`,
    };
    const Settings = {
      template: `<div><h3>系统设置</h3></div>`,
    };
    //  创建路由对象
    const router = new VueRouter({
      routes: [{
        path: '/', component: App,
        redirect: '/users',
        children: [
          { path: '/users', component: Users },
          { path: '/userinfo/:id', component: UserInfo ,props:true},
          { path: '/rights', component: Rights },
          { path: '/goods', component: Goods },
          { path: '/orders', component: Orders },
          { path: '/settings', component: Settings },
        ]
      },
      ],

    })

    const vm = new Vue({
      el: '#app',
      router
    });
  </script>
</body>

</html>
```

案例思路：
1).先将素材文件夹中的11.基于vue-router的案例.html复制到我们自己的文件夹中。
看一下这个文件中的代码编写了一些什么内容，
这个页面已经把后台管理页面的基本布局实现了
2).在页面中引入vue，vue-router
3).创建Vue实例对象，准备开始编写代码实现功能
4).希望是通过组件的形式展示页面的主体内容，而不是写死页面结构，所以我们可以定义一个根组件：

```js
//只需要把原本页面中的html代码设置为组件中的模板内容即可
const app = {
    template:`<div>
        <!-- 头部区域 -->
        <header class="header">传智后台管理系统</header>
        <!-- 中间主体区域 -->
        <div class="main">
          <!-- 左侧菜单栏 -->
          <div class="content left">
            <ul>
              <li>用户管理</li>
              <li>权限管理</li>
              <li>商品管理</li>
              <li>订单管理</li>
              <li>系统设置</li>
            </ul>
          </div>
          <!-- 右侧内容区域 -->
          <div class="content right">
            <div class="main-content">添加用户表单</div>
          </div>
        </div>
        <!-- 尾部区域 -->
        <footer class="footer">版权信息</footer>
      </div>`
  }
```
5).当我们访问页面的时候，默认需要展示刚刚创建的app根组件，我们可以
创建一个路由对象来完成这个事情,然后将路由挂载到Vue实例对象中即可
```js
const myRouter = new VueRouter({
    routes:[
        {path:"/",component:app}
    ]
})

const vm = new Vue({
    el:"#app",
    data:{},
    methods:{},
    router:myRouter
})
```
补充：到此为止，基本的js代码都处理完毕了，我们还需要设置一个路由占位符
```js
<body>
  <div id="app">
    <router-view></router-view>
  </div>
</body>
```
6).此时我们打开页面应该就可以得到一个VueRouter路由出来的根组件了
我们需要在这个根组件中继续路由实现其他的功能子组件
先让我们更改根组件中的模板：更改左侧li为子级路由链接，并在右侧内容区域添加子级组件占位符
```js
const app = {
    template:`<div>
        ........
        <div class="main">
          <!-- 左侧菜单栏 -->
          <div class="content left">
            <ul>
              <!-- 注意：我们把所有li都修改为了路由链接 -->
              <li><router-link to="/users">用户管理</router-link></li>
              <li><router-link to="/accesses">权限管理</router-link></li>
              <li><router-link to="/goods">商品管理</router-link></li>
              <li><router-link to="/orders">订单管理</router-link></li>
              <li><router-link to="/systems">系统设置</router-link></li>
            </ul>
          </div>
          <!-- 右侧内容区域 -->
          <div class="content right">
            <div class="main-content">
                <!-- 在 -->
                <router-view></router-view> 
            </div>
          </div>
        </div>
        .......
      </div>`
  }
```
然后，我们要为子级路由创建并设置需要显示的子级组件
```js
//建议创建的组件首字母大写，和其他内容区分
const Users = {template:`<div>
    <h3>用户管理</h3>
</div>`}
const Access = {template:`<div>
    <h3>权限管理</h3>
</div>`}
const Goods = {template:`<div>
    <h3>商品管理</h3>
</div>`}
const Orders = {template:`<div>
    <h3>订单管理</h3>
</div>`}
const Systems = {template:`<div>
    <h3>系统管理</h3>
</div>`}

//添加子组件的路由规则
const myRouter = new VueRouter({
    routes:[
        {path:"/",component:app , children:[
            { path:"/users",component:Users },
            { path:"/accesses",component:Access },
            { path:"/goods",component:Goods },
            { path:"/orders",component:Orders },
            { path:"/systems",component:Systems },
        ]}
    ]
})

const vm = new Vue({
    el:"#app",
    data:{},
    methods:{},
    router:myRouter
})
```

7).展示用户信息列表：
    A.为Users组件添加私有数据,并在模板中循环展示私有数据

```js
const Users = {
  data() {
    return {
      userList: [
        { id: 1, name: "zs", age: 18 },
        { id: 2, name: "ls", age: 19 },
        { id: 3, name: "wang", age: 20 },
        { id: 4, name: "jack", age: 21 },
      ]
    }
  },
  template: `<div>
    <h3>用户管理</h3>
    <table>
        <thead>
            <tr>
                <th>编号</th>
                <th>姓名</th>
                <th>年龄</th>
                <th>操作</th>
            </tr>
        </thead>
        <tbody>
            <tr :key="item.id" v-for="item in userList">
                <td>{{item.id}}</td>
                <td>{{item.name}}</td>
                <td>{{item.age}}</td>
                <td><a href="javascript:;">详情</a></td>
            </tr>
        </tbody>
    </table>
</div>`}
```

8.当用户列表展示完毕之后，我们可以点击列表中的详情来显示用户详情信息，首先我们需要创建一个组件，用来展示详情信息
```js
const UserInfo = {
    props:["id"],
    template:`<div>
      <h5>用户详情</h5>
      <p>查看 {{id}} 号用户信息</p>
      <button @click="goBack">返回用户详情页</button>
    </div> `,
    methods:{
      goBack(){
        //当用户点击按钮，后退一页
        this.$router.go(-1);
      }
    }
  }
```
然后我们需要设置这个组件的路由规则
```js
const myRouter = new VueRouter({
    routes:[
        {path:"/",component:app , children:[
            { path:"/users",component:Users },
            //添加一个/userinfo的路由规则
            { path:"/userinfo/:id",component:UserInfo,props:true},
            { path:"/accesses",component:Access },
            { path:"/goods",component:Goods },
            { path:"/orders",component:Orders },
            { path:"/systems",component:Systems },
        ]}
    ]
})

const vm = new Vue({
    el:"#app",
    data:{},
    methods:{},
    router:myRouter
})
```
再接着给用户列表中的详情a链接添加事件
```js
const Users = {
    data(){
        return {
            userList:[
                {id:1,name:"zs",age:18},
                {id:2,name:"ls",age:19},
                {id:3,name:"wang",age:20},
                {id:4,name:"jack",age:21},
            ]
        }
    },
    template:`<div>
        <h3>用户管理</h3>
        <table>
            <thead>
                <tr>
                    <th>编号</th>
                    <th>姓名</th>
                    <th>年龄</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                <tr :key="item.id" v-for="item in userList">
                    <td>{{item.id}}</td>
                    <td>{{item.name}}</td>
                    <td>{{item.age}}</td>
                    <td><a href="javascript:;" @click="goDetail(item.id)">详情</a></td>
                </tr>
            </tbody>
        </table>
    </div>`,
    methods:{
        goDetail(id){
            this.$router.push("/userinfo/"+id);
        }
    }
}
```

# Vue router 4

## 使用步骤

① 在项目中安装 vue-router

② 定义路由组件

③ 声明路由链接和占位符

④ 创建路由模块

⑤ 导入并挂载路由模块  

### 1.在项目中安装vue-router

在 vue3 的项目中，只能安装并使用 vue-router 4.x。安装的命令如下：  

```bash
npm install vue-router@next 
```

### 2 定义路由组件

举个栗子：在项目中定义 MyHome.vue、MyMovie.vue、MyAbout.vue 三个组件，将来要使用 vue-router 来控制它们的展示与切换。![image-20210810153352360](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108101533469.png)

### 3.  声明路由链接和占位符

可以使用 `<router-link>` 标签来声明**路由链接**，并使用 `<router-view>` 标签来声明**路由占位符**。示例代码如下：  

```vue
<template>
  <div>
    <h1>App 根组件</h1>
    <!-- 声明路由链接 -->
    <!-- 不需要加#，vue会帮我加 route-link会渲染成a标签 -->
    <router-link to="/home">首页</router-link>
    <router-link to="/movie">电影</router-link>
    <router-link to="/about">关于</router-link>
    <!-- 声明路由占位符 -->
    <router-view></router-view>
  </div>
</template>
```

### 4 创建路由模块

在项目中创建 router.js 路由模块，在其中按照如下 4 个步骤创建并得到路由的实例对象：
① 从 vue-router 中按需导入两个方法

```js
// 从vue-router中按需导入两个方法

// createRouter方法用于创建路由的实例对象
// createWebHashHistory 用于指定路由的工作模式（hash模式）

import { createRouter,createWebHashHistory } from "vue-router";
```

② 导入需要使用路由控制的组件

```js
import Home from "./MyHome.vue";
import About from "./MyAbout.vue";
import Movie from "./MyMovie.vue";
```

③ 创建路由实例对象 

```js
// 创建路由实例对象
const router=createRouter({
    // 通过history属性指定路由的工作模式
    history:createWebHashHistory(),
    routes:[
        {path:'/home',component:Home},
        {path:'/about',component:About},
        {path:'/movie',component:Movie},
    ]
});
```

④ 向外共享路由实例对象

```js
export default router;
```

⑤ 在 main.js 中导入并挂载路由模块  

```js
import { createApp } from 'vue'
import App from './components/02.start/App.vue'
import './index.css'
// 1.导入路由模块
import router from './components/02.start/router.js'

const app=createApp(App);

// 2.使用路由模块
// app.use()方法用于挂载路由模块
app.use(router);

app.mount('#app');
```

## 路由重定向

路由重定向指的是：用户在访问地址 A 的时候，强制用户跳转到地址 C ，从而展示特定的组件页面。
通过路由规则的 redirect 属性，指定一个新的路由地址，可以很方便地设置路由的重定向：  

```js
const router=createRouter({
    // 通过history属性指定路由的工作模式
    history:createWebHashHistory(),
    routes:[
        // 访问根路径时，将页面重定向到Home页面
        {path:'/',redirect:Home},
        {path:'/home',component:Home},
        {path:'/about',component:About},
        {path:'/movie',component:Movie},
    ]
});
```

## 路由高亮

可以通过如下的两种方式，将激活的路由链接进行高亮显示：

① 使用默认的高亮 class 类

② 自定义路由高亮的 class 类  

### 1.自定义路由高亮的 class 类

被激活的路由链接，**默认**会自动应用一个叫做 `router-link-active` 的类名。开发者可以使用此类名选择器，为**激活的路由链接**设置高亮的样式：  

![image-20210810162022542](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108101620610.png)

```css
/*可以在index.css 为router-link-active设置样式*/
.router-link-active{
  background-color: red;
  color:white;
  font-weight: bold;
}
```

### 2.自定义路由高亮的 class 类

在创建路由的实例对象时，开发者可以基于 `linkActiveClass` 属性，自定义路由链接被激活时所应用的类名：  

```js
// 创建路由实例对象
const router=createRouter({
    // 通过history属性指定路由的工作模式
    history:createWebHashHistory(),
    linkActiveClass:'active-router',
    routes:[
        // 访问根路径时，将页面重定向到Home页面
        {path:'/',redirect:Home},
        {path:'/home',component:Home},
        {path:'/about',component:About},
        {path:'/movie',component:Movie},
    ]
});
```

## 嵌套路由  

通过路由实现组件的嵌套展示，叫做嵌套路由。

![image-20210810163106972](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108101631034.png)

① 声明子路由链接和子路由占位符

② 在父路由规则中，通过 children 属性嵌套声明子路由规则  

### 1. 声明子路由链接和子路由占位符

在 About.vue 组件中，声明 tab1 和 tab2 的子路由链接以及子路由占位符。示例代码如下：  

```vue
<template>
  <div>
    <h3>MyAbout组件</h3>
    <hr />
    <!-- 声明子路由链接 -->
    <router-link to="/about/tab1">tab1</router-link> &nbsp;
    <router-link to="/about/tab2">tab2</router-link> &nbsp;
    <router-link to="/about/tab3">tab3</router-link>
    <!-- 声明子路由占位符 -->
    <router-view></router-view>
  </div>
</template>
```

### 2.通过 children 属性声明子路由规则

在 router.js 路由模块中，**导入需要的组件**，并使用 `children` 属性**声明子路由规则**。示例代码如下：

```js
import Tab1 from './Tab1.vue';
import Tab2 from './Tab2.vue';

// 创建路由实例对象
const router = createRouter({
    // 通过history属性指定路由的工作模式
    history: createWebHashHistory(),
    routes: [
        // 访问根路径时，将页面重定向到Home页面
        { path: '/', redirect: '/home' },
        { path: '/home', component: Home },
        {
            path: '/about',
            component: About,
            // 通过children属性嵌套子级路由规则
            children: [
                {path:'tab1',component:Tab1},
                {path:'tab2',component:Tab2},
            ]
        },
        { path: '/movie', component: Movie },
    ]
});
```

children属性下的path，要么写完整路径`/about/tab1`，要么直接写`tab1`。

**注意，以 `/` 开头的嵌套路径将被视为根路径。这允许你利用组件嵌套，而不必使用嵌套的 URL。**

## 动态路由匹配

### 1.动态路由匹配概念

动态路由指的是：把 Hash 地址中可变的部分定义为参数项，从而提高路由规则的复用性。在 vue-router 中使用英文的冒号（:）来定义路由的参数项。示例代码如下： 

```vue
<router-link to="/movie/1">电影1</router-link> 
<router-link to="/movie/2">电影2</router-link> 
<router-link to="/movie/3">电影3</router-link> 
```

```js
//路由中的动态参数 以: 声明，冒号后面的是自定的参数名称
{path:'/movie/:id',component:Moive}

//就将以下的三个规则合并成了一个，提高复用性
{path:'/movie/1',component:Moive}
{path:'/movie/2',component:Moive}
{path:'/movie/3',component:Moive}
```

### 2.$route.params 参数对象

**通过动态路由匹配的方式渲染出来的组件**中，可以使用 `$route.params` 对象访问到**动态匹配的参数值**。  

```vue
<template>
  <div>MyMoive组件 {{$route.params.id}}</div>
</template>

<script>
export default {
  name:'MyMovie'
}
</script>
```

### 3.使用 props 接收路由参数

为了简化路由参数的获取形式，vue-router 允许在**路由规则**中开启 `props` 传参。示例代码如下：  

```vue
//1.在定义路由规则时，声明props:true
// 即可在movie组件中，以props形式接收被路由规则匹配的参数
{ path: '/movie/:id', component: Movie,props:true }

<template>
  <!-- 3.直接使用props中接收的参数 -->
  <div>MyMoive组件 {{ id }}</div>
</template>

<script>
export default {
  // 2. 使用props接收路由规则匹配到的参数
  props: ["id"],
};
</script>
```

#### 路由规则中`props`的值是对象类型

如果 props是一个对象，它会被按原样设置为组件属性，此时路径中的id已经不能访问了。（如果props设置为true，`route.params`才会被设置为组件的属性）

```js
// 创建路由实例对象
const router = new VueRouter({
    // routes是路由规则数组
    routes: [
        // 如果 props是一个对象，它会被按原样设置为组件属性
        { path: '/user/:id', component: User,props:{uname:'lisi',age:20}},
    ]
})
```

```js
const User = {
    props: ['id', 'uname', 'age'],
    // 此时的id并没有传值，需要使用$route.params.id才行
    template: `<h1>user 组件---Id：{{id}}--id：{{$route.params.id}}--{{uname}}--{{age}}</h1>`
}
```

最终效果：

![image-20210803134627526](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210803134627.png)

#### `props`的值为函数类型

形参route的值等于 `route.params`，即path中的动态参数。

![image-20210803214818700](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210803214818.png)

```js
const User = {
    props: ['id', 'uname', 'age'],
    template: `<h1>user 组件---Id：{{id}}--id：{{$route.params.id}}--{{uname}}--{{age}}</h1>`
}
```

```js
// 创建路由实例对象
const router = new VueRouter({
    // routes是路由规则数组
    routes: [
        // 如果 props是一个对象，它会被按原样设置为组件属性
        {
            path: '/user/:id',
            component: User,
            props: (route) => { return {uname:'zhangsan',age:20,id:route.params.id} }
        },
    ]
})
```

![image-20210803214603161](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210803214603.png)



## 编程式导航

通过调用 API 实现导航的方式，叫做编程式导航。与之对应的，通过点击链接实现导航的方式，叫做声明式导航。例如：

- 普通网页中点击 `<a>` 链接、vue 项目中点击 `<router-link>` 都属于声明式导航
- 普通网页中调用 `location.href` 跳转到新页面的方式，属于编程式导航  

### 1.vue-router 中的编程式导航 API

vue-router 提供了许多编程式导航的 API，其中最常用的两个 API 分别是：

① this.$router.push('hash 地址')

- 跳转到指定 Hash 地址，从而展示对应的组件

② this.$router.go(数值 n)

- 实现导航历史的前进、后退  

### 2 $router.push

调用 `this.$router.push()` 方法，可以跳转到指定的 hash 地址，从而展示对应的组件页面。示例代码如下：  

```vue
<template>
  <div>
    <h3>MyHOME 组件</h3>
    <button @click="gotoMovie(3)">Go to movie</button>
  </div>
</template>

<script>
export default {
  methods: {
    gotoMovie(id){
      // 跳转到 /movie/3
      this.$router.push(`/movie/${id}`);
    }
  },
}
</script>
```

3.$router.go

调用 `this.$router.go()` 方法，可以在浏览历史中进行前进和后退。示例代码如下：  

```vue
<template>
  <!-- 3.直接使用props中接收的参数 -->
  <div>
    <h3>MyMoive组件---- {{ id }}</h3>
    <button @click='goBack'>回退</button>
  </div>

</template>

<script>
export default {
  // 2. 使用props接收路由规则匹配到的参数
  props: ["id"],
  methods: {
    goBack(){
      this.$router.go(-1);
    }
  },
};
</script>
```

## 命名路由  

**通过 name 属性为路由规则定义名称的方式，叫做命名路由。**示例代码如下：

注意：命名路由的 name 值不能重复，必须保证唯一性！  

### 6.1 使用命名路由实现声明式导航

为 `<router-link>` 标签动态绑定 to 属性的值，并通过 **name 属性**指定要跳转到的路由规则。期间还可以用params 属性指定跳转期间要携带的路由参数。示例代码 如下：  

```vue
// 在router.js中给路由命名为mov
{name:'mov' ,path: '/movie/:id', component: Movie,props:true },

//============
<template>
  <div>
    <h3>MyHOME 组件</h3>
    <button @click="gotoMovie(3)">Go to movie</button>
    <router-link :to="{name:'mov',params:{id:3}}">Go to movie</router-link>
  </div>
</template>

```

### 6.2 使用命名路由实现编程式导航

调用 `push` 函数期间指定一个**配置对象**，name 是要跳转到的路由规则、params 是携带的路由参数：  

```vue
<template>
  <div>
    <h3>MyHOME 组件</h3>
    <router-link :to="{name:'mov',params:{id:2}}">Go to movie</router-link>
  </div>
</template>

<script>
export default {
  methods: {
    gotoMovie(id){
      // 跳转到 /movie/1
      this.$router.push({name:'mov',params:{id}});
    }
  },
}
</script>
```

## 导航守卫

**导航守卫**可以控制**路由的访问权限**。示意图如下：  

![image-20210810192715528](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108101927665.png)

### 1.声明全局导航守卫

**全局导航守卫**会拦截**每个路由规则**，从而对每个路由进行**访问权限的控制**。可以按照如下的方式定义全局导航守卫：  

```js
// 创建路由实例对象
const router=createRouter({...});
// 调用路由实例对象的beforeEach函数，声明“全局前置守卫”
// fn 必须是一个函数，每次拦截到路由的请求，必须调用fn进去处理
// 因此fn叫做“守卫访问”

router.beforeEach(()=>{
    console.log('ok');
});
```

### 2.守卫方法的 3 个形参

**全局导航守卫**的守卫方法中接收 3 个形参，格式为：

```js
// 创建路由实例对象
const router=createRouter({...});
                           
// 全局前置守卫
router.beforeEach((to,from,next)=>{
    // to 目标路由对象
    // from 当前导航正要离开的路由对象
    // next 是一个函数，表示放行
    console.log(to,from);
    //
})
```

打印to和from的结果：

![image-20210810195445380](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108101954448.png)

注意：
① 在守卫方法中**如果不声明 next 形参，则默认允许用户访问每一个路由**！

② **在守卫方法中如果声明了 next 形参，则必须调用 next() 函数，否则不允许用户访问任何一个路由！**  

### 3.next 函数的 3 种调用方式

参考示意图，分析 next 函数的 3 种调用方式最终导致的结果：

![image-20210810201559508](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108102015581.png)

- 直接放行：next();
- **强制其停留在当前页面**：next(false);
- **强制其跳转到其他页面如**：next('/login')  

```js
const router=createRouter({...});

// 全局导航守卫
router.beforeEach((to,from,next)=>{
    // to为将要访问的页面
    // from从哪个页面来的
    
    if(to.path==='/main'){
        // 如果用户要访问后台页面

        // next(false)强制用户停留在当前页面
        next(false);
    }else{
        // 用户访问的不是后台页面
        // next() 直接放行
        next();
    }
});
```

```js
const router=createRouter({...});

// 全局导航守卫
router.beforeEach((to,from,next)=>{
    // to为将要访问的页面
    // from从哪个页面来的
    
    if(to.path==='/main'){
        // 如果用户要访问后台页面
        // 跳转到登录页面
        next('/login');
    }else{
        // 用户访问的不是后台页面
        // next() 直接放行
        next();
    }
});
```

### 4.结合 token 控制后台主页的访问权限  

```js
router.beforeEach((to,from,next)=>{
// 获取本地存储的token值
const tokenStr=localStorage.getItem('token');
    if(to.path==='/main'&&!tokenStr){
        // 如果用户要访问后台页面且不存在token时
        // 跳转到登录页面
        next('/login');
    }else{
        // 用户访问的不是后台页面
        // next() 直接放行
        next();
    }
});
```



## 5.实现页面标题

首先用到了'meta'属性，然后在导航守卫中给将meta的值传给标题。

<img src="https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108311140471.png" alt="image-20210831114031423" style="zoom:50%;" />

```js
import { createRouter, createWebHashHistory  } from 'vue-router';
import Home from '../views/Home.vue';
import Blogs from '../views/Blogs.vue';


const routes = [
    { path: '/', component: Home,meta:{title:'Home'}},
    { path: '/home', name: 'Home', component: Home,meta:{title:'Home'} },
    { path: '/blogs', name: 'Blogs', component: Blogs,meta:{title:'Blogs'} },

];

const router = createRouter({
    history: createWebHashHistory (),
    routes
});

router.beforeEach((to,from,next)=>{
    document.title = `${to.meta.title} | FireBlogs`;
    next();
})

export default router;
```

