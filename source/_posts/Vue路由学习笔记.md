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

![image-20210802121213464](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210802121213.png)

### SPA

- 后端渲染（存在性能问题，假如用户频繁地提交表单，则会造成页面频繁刷新）。
- Ajax前端渲染（前端渲染提高性能，但是不支持浏览器的前进后退操作）。
- SPA（ Single Page Application）单页面应用程序：整个网站只有一个页面，内容的变化通过Ajax局部更新实现、同时支持浏览器地址栏的前进和后退操作。
- SPA实现原理之一：基于URL地址的hash（hash的变化会导致浏览器记录访问历史的变化、但是hash的变化不会触发新的URL请求）。
- 在实现SPA过程中，最核心的技术点就是**前端路由**。

### 前端路由

- 概念：根据不同的**用户事件**，显示不同的页面内容。
- 本质：**用户事件**与**事件处理函数**之间的对应关系。

![image-20210802122059780](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210802122059.png)

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

![动1](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210802135422.gif)

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

官网地址：https://router.vuejs.org/zh/

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

 ![动](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210803112827.gif)

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

![image-20210803120119722](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210803120119.png)

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

![动2](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210803123419.gif)

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

   ![image-20210803134627526](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210803134627.png)

3. `props`的值为函数类型

   形参route的值等于 `route.params`，即path中的动态参数。

   ![image-20210803214818700](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210803214818.png)

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

![image-20210803214603161](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210803214603.png)

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

![动23](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210804125047.gif)

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

