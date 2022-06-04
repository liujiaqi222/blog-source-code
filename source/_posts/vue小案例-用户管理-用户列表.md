---
title: vue小案例_用户管理_用户列表
date: 2021-08-12 08:51:12
tags: [Vue,小案例]
---

# 用户管理小案例

项目起始文件：https://xiaobaijun.lanzoui.com/i7NBYshgffg

## 0. 实现步骤

1. 安装并配置 vue-router 4.x

2. 展示 Login.vue 登录组件

3. 模拟并实现登录功能

4. 通过路由渲染 Home.vue

5. 实现退出登录的功能

6. 全局控制路由的访问权限

7. 将左侧菜单改造为路由链接

8. 渲染用户管理页面的数据

9. 实现跳转到用户详情页的功能

10. 开启路由的 props 传参

11. 通过编程式导航实现后退功能  

## 1. 安装并配置 vue-router 4.x

1.运行如下的命令，安装 vue-router ：

```bash
npm install vue-router@next 
```

2.在 src 目录下新建 router.js 路由模块：

```js
import {createRouter,createWebHashHistory} from 'vue-router'

// 创建路由规则
const router = createRouter({
    history:createWebHashHistory(),
    routes:[],
})

// 向外导出router模块
export default router;
```

3.在 main.js 入口文件中导入并挂载路由对象：

```js
import { createApp } from 'vue'
import App from './App.vue'

//导入router模块
import router from './router.js';

// 创建 app 实例
const app = createApp(App);

// 全局挂载router
app.use(router);

// 挂载 app 实例
app.mount('#app')

```

## 2.展示 Login.vue 登录组件

1.在 router.js 模块中导入 Login.vue 组件：

```js
// 导入组件
import Login from './components/MyLogin.vue'
```

2.声明路由规则如下：

```js
const router = createRouter({
    history:createWebHashHistory(),
    routes:[
        // 路由重定向
        {path:'/',redirect:'/login'},
        {path:'/login',component:Login,name:'login'}
    ],
})
```

3.在 `App.vue` 组件中声明路由占位符：

```vue
<template>
  <div>
    <!-- 路由占位符 -->
    <router-view></router-view>
  </div>
</template>
```

## 3.模拟并实现登录功能

1.在 MyLogin.vue 组件中声明如下的 data 数据：

```js
export default {
  name: 'MyLogin',
  data(){
    return {
      username:'',
      password:''
    }
  }
}
```

2.为用户名和密码的文本框进行 **v-model 双向数据**绑定：

```vue
<!-- 登录名称 -->
<div class="form-group form-inline">
  <label for="username">登录名称</label>
  <input type="text" class="form-control ml-2" id="username" placeholder="请输入登录名称" autocomplete="off" v-model="username">
</div>
<!-- 登录密码 -->
<div class="form-group form-inline">
  <label for="password">登录密码</label>
  <input type="password" class="form-control ml-2" id="password" placeholder="请输入登录密码" v-model="password">
</div>
```

3.为 登录按钮 绑定点击事件处理函数：

```vue
<button type="button" class="btn btn-primary" @click='onLoginClick'>登录</button>
```

4.在 methods 中声明 onLoginClick 事件处理函数如下：

```js
onLoginClick(){
  // 判断用户名和密码是否正确
 if(this.username==='admin'&&this.password==='123456'){
    // 登录成功 跳转到后台主页
    this.$router.push('/home');
    // 模拟存储token
    return localStorage.setItem('token','Bearer xxx');
  }else{
    // 登录失败 清除token
    localStorage.removeItem('token');

  }
}
```

## 4. 通过路由渲染 Home.vue

1.在 router.js 中导入 Home.vue 组件：

```js
import Home from './components/MyHome.vue';
```

2.在 routes 路由规则的数组中，声明对应的路由规则：

```js
const router = createRouter({
    history:createWebHashHistory(),
    routes:[
        // 路由重定向
        {path:'/',redirect:'/login'},
        {path:'/login',component:Login,name:'login'},
        {path:'/home',component:Home}
    ],
})
```

3.渲染 Home.vue 组件的基本结构：

```vue
<template>
  <div class="home-container">
    <!-- 头部组件 -->
    <my-header></my-header>
    <!-- 主体区域 -->
    <div class="home-main-box">
      <!-- 左侧边栏区域 -->
      <my-aside></my-aside>
      <!-- 右侧内容主体区域 -->
      <div class="home-main-body"></div>
    </div>
  </div>
</template>
```

## 5.实现退出登录的功能

1.在 MyHeader.vue 组件中，为 退出登录 按钮绑定 click 事件处理函数：

```vue
<button type="button" class="btn btn-light" @click="onLogout">退出登录</button>
```

2.在 methods 中声明如下的事件处理函数：

```js
methods: {
  onLogout(){
      //跳转到登录页
    this.$router.push('/login');
      // 清空token
    localStorage.removeItem('token');
  }
},
```

## 6.全局控制路由的访问权限

在 router.js 模块中，通过 router 路由实例对象，全局挂载路由导航守卫：  

```js
// 导航守卫
router.beforeEach((to,from,next)=>{
    // 如果用户访问的是登录页面 直接放行
    if(to.path==='/login') return next();
    // 获取token值
    const token=localStorage.getItem('token');
    // 如果token 不存在 则跳转到登录页面
    if(!token) return next('/login');
    // 存在token 则直接放行
    next();
})
```

## 7. 将左侧菜单改造为路由链接

1.打开 `MyAside.vue` 组件，把 li 内部的纯文本升级改造为 `<router-link>` 组件：

```vue
<ul class="user-select-none menu">
  <li class="menu-item">
    <router-link to='/home/users'>用户管理</router-link>
  </li>
  <li class="menu-item">
    <router-link to='/home/rights'>权限管理</router-link>
  </li>
  <li class="menu-item">
    <router-link to='/home/goods'>商品管理</router-link>
  </li>
  <li class="menu-item">
    <router-link to='/home/orders'>订单管理</router-link>
  </li>
  <li class="menu-item">
    <router-link to='/home/settings'>系统设置</router-link>
  </li>
</ul>
```

2.打开 Home.vue 组件，在 右侧内容主体区域 中声明子路由的占位符：

```vue
<template>
  <div class="home-container">
    <!-- 头部组件 -->
    <my-header></my-header>
    <!-- 主体区域 -->
    <div class="home-main-box">
      <!-- 左侧边栏区域 -->
      <my-aside>
      </my-aside>
      <!-- 右侧内容主体区域 -->
      <div class="home-main-body">
        <!-- 子路由的占位符 -->
        <router-view></router-view>
      </div>
    </div>
  </div>
</template>
```

3.在 router.js 中导入左侧菜单对应的组件：

```js
import Users from './components/menus/MyUsers.vue'
import Rights from './components/menus/MyRights.vue'
import Goods from './components/menus/MyGoods.vue'
import Orders from './components/menus/MyOrders.vue'
import Settings from './components/menus/MySettings.vue'
```

4.通过 children 属性，为 home 规则定义子路由规则如下：

```js
// 创建路由规则
const router = createRouter({
    history: createWebHashHistory(),
    routes: [
        // 路由重定向
        { path: '/', redirect: '/login' },
        { path: '/login', component: Login, name: 'login' },
        {
            path: '/home', component: Home, name: 'home', 
            // 当用户访问/home时，重定向至/home/users,
            redirect:'/home/users',
            children: [
                { path: 'users', component: Users },
                { path: 'rights', component: Rights },
                { path: 'goods', component: Goods },
                { path: 'orders', component: Orders },
                { path: 'settings', component: Settings },
            ]
        }
    ],
})
```

## 8.渲染用户管理页面的数据

1.在 MyUsers.vue 组件中，通过 v-for 指令循环渲染用户列表的数据：

```vue
<tbody>
  <tr v-for="(item,index) in userlist" :key='item.id'>
    <td>{{index+1}}</td>
    <td>{{item.name}}</td>
    <td>{{item.age}}</td>
    <td>{{item.position}}</td>
    <td>详情</td>
  </tr>
</tbody>
```

## 9.实现跳转到用户详情页的功能

1.在 MyUsers.vue 组件中，渲染详情页的路由链接如下：

```vue
<td><router-link :to="'/home/users/'+item.id">详情</router-link></td>
```

2.在 router.js 中导入用户详情页组件：

```js
import UserDetail from './components/user/MyUserDetail.vue'
```

3.在 home 规则的 children 节点下，声明 用户详情页 的路由规则：

```js
{
    path: '/home', component: Home, name: 'home',
    // 当用户访问/home时，重定向至/home/users,
    redirect: '/home/users',
    children: [
        { path: 'users', component: Users },
        { path: 'rights', component: Rights },
        { path: 'goods', component: Goods },
        { path: 'orders', component: Orders },
        { path: 'settings', component: Settings },
        { path: 'users/:id', component: UserDetail }
    ]
}
```

## 10.开启路由的 props 传参

1.在 router.js 模块中，为 用户详情页 的路由规则开启 props 传参：

```js
{
    path: '/home', component: Home, name: 'home',
    // 当用户访问/home时，重定向至/home/users,
    redirect: '/home/users',
    children: [
        { path: 'users', component: Users },
        { path: 'rights', component: Rights },
        { path: 'goods', component: Goods },
        { path: 'orders', component: Orders },
        { path: 'settings', component: Settings },
        { path: 'users/:id', component: UserDetail,props:true }
    ]
}
```

2.在 MyUserDetail.vue 组件中声明 props 参数：

```vue
export default {
name: 'MyUserDetail',
props: ['id'],
}
```

3.在 MyUserDetail.vue 组件的结构中直接使用路由参数：

```vue
<template>
  <div>
    <button type="button" class="btn btn-light btn-sm">后退</button>
    <h4 class="text-center">用户详情---{{id}}</h4>
  </div>
</template>
```

11.通过编程式导航实现后退功能

1.在 MyUserDetail.vue 组件中，为后退按钮绑定点击事件处理函数：

```vue
<template>
  <div>
    <button type="button" class="btn btn-light btn-sm" @click="goBack">后退</button>
    <h4 class="text-center">用户详情---{{id}}</h4>
  </div>
</template>
```

2.在 methods 中声明 goBack 事件处理函数如下：

```vue
<script>
export default {
  name: 'MyUserDetail',
  props:['id'],
  methods: {
    goBack(){
      this.$router.go(-1);
    }
  },
}
</script>
```

# 用户列表小案例

- vue-cli 创建 vue2 项目

- element ui 组件库

- axios 拦截器

- proxy 跨域接口代理

- vuer-router 路由  

## 0. 实现步骤

1. 初始化项目
2.  渲染用户列表组件
3.  基于全局过滤器处理时间格式
4.  实现添加用户的操作
5.  实现删除用户的操作
6.  通过路由跳转到详情页  

## 1. 初始化项目

### 1.1 梳理项目结构

1.基于 vue-cli 运行如下的命令，新建 vue2.x 的项目：

```bash
vue create code-users
```

2.重置 App.vue 组件中的代码：

3.删除 components 目录下的 HelloWorld.vue 组件。

### 1.2 修改项目的开发调试配置

1.在项目根目录中新建 vue.config.js 配置文件。

2.在 vue.config.js 配置文件中，通过 devServer 节点添加如下的配置项：

```js
module.exports = {
devServer: {
// 修改 dev 期间的端口号
port: 3000,
// 自动打开浏览器
open: true
}
}
```

### 1.3 初始化路由

1.运行如下的命令，在 vue2.x 的项目中安装 vue-router：

```bash
npm install vue-router -S
```

2.在 src 目录下新建 router/index.js 路由模块：

```js
// 路由模块
import Vue from 'vue'
import VueRouter from 'Vue-router'

// 安装路由插件
Vue.use(VueRouter);

// 创建路由实例对象
const router = new VueRouter({
    routes:[]// 路由规则
})；

// 向外共享路由实例对象
export default router;
```

3.在 main.js 模块中导入并挂载路由模块：

```js
import Vue from 'vue'
import App from './App.vue'
// 导入路由模块
import router from './router/index.js'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  router
}).$mount('#app')

```

### 1.4 使用路由渲染 UserList 组件

1.在 components 目录下新建 UserList.vue 组件如下：

```vue
<template>
  <div>
      <h3>UserLIst 组件</h3>
  </div>
</template>

<script>
export default {
    name:'UserList'
}
</script>

<style lang="less" scoped>
</style>
```

2.在 router/index.js 路由模块中新增如下的路由规则：

```js
// 导入要被渲染的组件
import UserList from '@/components/UserList.vue'

// 声明路由规则
const router=new VueRouter({
    routes:[
        {path:'/',redirect:'/users'},
        {path:'/users',component:UserList}
    ],
})
```

3.在 App.vue 中声明 router-view 路由占位符：

```vue
<template>
  <div>
    <h1>App 根组件</h1>
    <router-view></router-view>
  </div>
</template>
```

## 2. 渲染用户列表组件

### 2.1 安装并配置 axios

1.运行如下的命令，在项目中安装 axios ：

```bash
npm install axios -S
```

2.在 main.js 中导入并配置 axios ：

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router/index.js'

import axios from 'axios';

//全局配置axios
Vue.prototype.$http=axios;
//配置根路径
axios.defaults.baseURL='https://www.escook.cn';

new Vue({
  render: h => h(App),
  router,
}).$mount('#app')
```

### 2.2 请求用户列表的数据

1.在 UserList.vue 组件中声明如下的 data 数据节点：

```js
data(){
  return {
    userlist:[],
  }
},
```

2.在 created 生命周期函数中预调用 getUserList 方法：

```js
created(){
  this.getUserList();
}
```

3.在 methods 中声明 getUserList 方法：

```js
methods: {
  async getUserList(){
    const {data}=await this.$http.get('/api/users');
    if(data.status!==0) return console.log('获取用户信息失败');
    this.userlist=data.data;
  }
},
```

### 2.3 解决跨域请求限制

> 由于 API 接口服务器并没有开启 CORS 跨域资源共享，因此终端会提示如下的错误：
> Access to XMLHttpRequest at ' https://www.escook.cn/api/users ' from origin ' http://l
> ocalhost:3000 ' has been blocked by CORS policy: No 'Access-Control-Allow-Origin'
> header is present on the requested resource.  

解决方案：
通过 vue.config.js 中的 devServer.proxy 即可在开发环境下将 API 请求代理到 API 服务器。

```js
module.exports={
    devServer:{
        // 修改dev期间的端口号
        port:3000,
        open:true, // 自动打开浏览器
        proxy:'https://www.escook.cn'
    }
}
```

同时，在 main.js 入口文件中，需要把 axios 的根路径改造为开发服务器的根路径：  

```js
//配置根路径
axios.defaults.baseURL='http://localhost:3000/';
//全局配置axios
Vue.prototype.$http=axios;
```

> 注意：devServer.proxy 提供的代理功能，仅在开发调试阶段生效。项目上线发布时，依旧需要
> API 接口服务器开启 CORS 跨域资源共享  

修改vue.config.js的配置后，需要重启打包的服务器。

### 2.4 安装并配置 element-ui

1.运行如下的命令，在项目中安装 element-ui 组件库：

```bash
npm i element-ui -S
```

2.在 main.js 中配置 element-ui：

```js
import Vue from 'vue';
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import App from './App.vue';

Vue.use(ElementUI);

new Vue({
  el: '#app',
  render: h => h(App)
});
```

3.在UserList.vue 中使用https://element.eleme.cn/#/zh-CN/component/table Element-ui的表格样式，并配置好数据。

当`el-table`元素中注入`data`对象数组后，在`el-table-column`中用`prop`属性来对应对象中的键名即可填入数据，用`label`属性来定义表格的列名。可以使用`width`属性来定义列宽。

![image-20210811192005052](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108111920170.png)

```vue
<template>
  <div>
    <!-- 用户的表格 -->
    <el-table :data="userlist" style="width: 100%">
      <el-table-column prop="id" label="#" width="180"> </el-table-column>
      <el-table-column prop="name" label="姓名" width="180"> </el-table-column>
      <el-table-column prop="age" label="年龄"> </el-table-column>
      <el-table-column prop="position" label="头衔"> </el-table-column>
      <el-table-column prop="addtime" label="创建时间"> </el-table-column>
    </el-table>
  </div>
</template>
```

![image-20210811193033787](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108111930881.png)

接着为表格添加一些[样式](https://element.eleme.cn/#/zh-CN/component/table#dai-ban-ma-wen-biao-ge)，如添加stripe属性。默认情况下，Table 组件是不具有竖直方向的边框的，如果需要，可以使用`border`属性，它接受一个`Boolean`，设置为`true`即可启用。

![image-20210811192753470](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108111927544.png)

如果想要第一列渲染中id，可以使用`type="index"`。

```vue
<el-table-column type="index" width="50" label="#" > </el-table-column>
```

### 2.5 自定义时间的格式

`el-table-column` 组件内部提供了一个默认插槽，可以获取到 row, column, $index这些数据。在UserList.vue中，通过接收插槽的数据，来自定义时间格式。

`scope.row`会返回当前行的数据。

```vue
<el-table-column label="创建时间">
  <template v-slot:default="scope">{{ scope.row.addtime }}</template>
</el-table-column>
```

之后使用一个过滤器，对时间进行格式化

```vue
<el-table-column label="创建时间">
  <template v-slot:default="scope">{{ scope.row.addtime|dateFormat }}</template>
</el-table-column>
```

在main.js中声明一个全局的过滤器：

```js
Vue.filter('dateFormat',(value)=>{
  const d=new Date(value);
  const y=d.getFullYear();
  const mm=d.getMonth()+1;
  const date=d.getDate();
  const h=d.getHours();
  const m=d.getMinutes();
  const s=d.getSeconds();
  return `${y}-${padZero(mm)}-${padZero(date)} ${padZero(h)}:${padZero(m)}:${padZero(s)}`;
})

//声明一个时间不足2位则补0的函数
function padZero(n){
  return n>9? n:'0'+n;
}
```

### 2.6 增加操作栏

首先，在UserList.vue中增加一列，label名为操作，然后在默认插槽中添加2个a标签。

```vue
<el-table-column label='操作' width="120">
  <template>
    <div>
      <a href="#">详情</a> &nbsp;
      <a href="#">删除</a>
    </div>
  </template>
</el-table-column>
```

### 2.7 添加新用户的弹出框

在UserList.vue中，添加一个按钮。

```vue
<!-- 添加按钮 -->
<!-- primary 表示 是蓝色的按钮 -->
<el-button type="primary" @click="dialogVisible = true">添加新用户</el-button>
```

此时，希望列表和按钮有一定的间距。而element组件的标签名就是类名，因此可以这样写：

```css
.el-table{
  margin-top: 15px;
}
```

![image-20210811210439793](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108112104982.png)

当点击添加新用户的时候，希望弹出一个填充信息的对话框。在element-ui中，找到对应的对话框组件https://element.eleme.cn/#/zh-CN/component/dialog。

需要设置`dialogVisible`属性，它接收`Boolean`，当为`true`时显示 Dialog。

```vue
<!-- 添加按钮 -->
<!-- primary 表示 是蓝色的按钮 -->
<el-button type="primary" @click="dialogVisible = true">添加新用户</el-button>
<!-- 添加用户的对话框 -->
<el-dialog
  title="提示"
  :visible.sync="dialogVisible"
  width="50%">
  <span>这是一段信息</span>
  <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
  </span>
</el-dialog>
```

在data中，要添加dialogVisible为false这个数据。

```js
data() {
  return {
    userlist: [],
    // 控制对话框的显示与隐藏
    dialogVisible: false
  };
},
```

将弹出框的提示内容改为表单：

```vue
<!-- 添加用户的表单 表单中填充的数据会保存在form中 -->
<el-form ref="form" :model="form"></el-form>
```

同时，在data中声明一个form的对象

```js
data() {
  return {
    userlist: [],
    // 控制对话框的显示与隐藏
    dialogVisible: false,
    // 要采集用户信息的对象
    form:{
      name:'',
      age:'',
      postion:'',
    }
  };
},
```

接着给表单，增加文本输入框。

```vue
<el-form :model="form" label-width="80px">
  <!-- 采集用户的姓名 -->
  <el-form-item label="用户姓名">
    <el-input v-model="form.name"></el-input>
  </el-form-item>
  <el-form-item label="年龄">
    <el-input v-model="form.age"></el-input>
  </el-form-item>
  <el-form-item label="头衔">
    <el-input v-model="form.postion"></el-input>
  </el-form-item>
</el-form>
```

![image-20210811212639641](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108112126735.png)

### 2.8 实现表单验证

为UserList.vue中的表单，添加表单验证，具体可见在https://element.eleme.cn/#/zh-CN/component/form#biao-dan-yan-zheng。

> Form 组件提供了表单验证的功能，只需要通过 `rules` 属性传入约定的验证规则，并将 Form-Item 的 `prop` 属性设置为需校验的字段名即可。

首先在data中，声明表单的验证规则：

```js
//表单的验证规则对象
formRules: {
  name: [
    // messge为错误提示信息，trigger:blur 表示失去焦点时触发验证
    { required: true, message: "姓名为必填项", trigger: "blur" },
    { min: 2, max: 15, message: "长度在 2 到 15 个字符", trigger: "blur" },
  ],
  age: [
    { required: true, message: "年龄为必填项", trigger: "blur" },
  ],
  postion:[
    { required: true, message: "头衔为必填项", trigger: "blur" },
    { min: 1, max: 10, message: "长度在 1 到 10个字符", trigger: "blur" },
  ]
},
```

接着在表单中，用prop接收验证规则。

```vue
<el-form :model="form" label-width="80px" :rules="formRules">
  <!-- 采集用户的姓名 -->
  <el-form-item label="用户姓名" prop="name">
    <el-input v-model="form.name"></el-input>
  </el-form-item>
  <el-form-item label="年龄" prop='age'>
    <el-input v-model.number="form.age"></el-input>
  </el-form-item>
  <el-form-item label="头衔" prop='postion'>
    <el-input v-model="form.postion"></el-input>
  </el-form-item>
</el-form>
```

### 2.9 自定义验证规则

对于年龄大小的验证，element-ui并没有提供相应的验证规则。因此，可以[自定义验证规则](https://element.eleme.cn/#/zh-CN/component/form#zi-ding-yi-xiao-yan-gui-ze)。在data中声明一个验证年龄的函数，接着通过规则中的validator属性调用它。

```js
data() {
  // 声明校验年龄的函数
  let checkAge=(rule,value,callback)=>{
    console.log(rule);
    // 判断年龄是否为整数
    if(!Number.isInteger(value)){
      return callback(new Error('年龄必须为整数'));
    }
    if(value>100||value<1) return callback(new Error('年龄必须在1~100之间!'));
    // 直接调用callback函数 代表验证通过
    callback();
  };
  return {
    //表单的验证规则对象
    formRules: {
      age: [
        { required: true, message: "年龄为必填项", trigger: "blur" },
        {validator: checkAge, trigger: 'blur' }
        ],
    },
    // 声明校验年龄的函数
  };
},
```

### 2.10 清空表单

在关闭表单后，希望重置表单的内容，并且重置校验的结果。以免第二次打开，还是上一次的结果。

<img src="https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108112208120.png" alt="image-20210811220826024" style="zoom:50%;" />

element-ui的弹出框组件，有如下几种事件。可以通过监听关闭事件，来实现清空表单的功能。

| 事件名称 | 说明                        | 回调参数 |
| :------- | :-------------------------- | :------- |
| open     | Dialog 打开的回调           | —        |
| opened   | Dialog 打开动画结束时的回调 | —        |
| close    | Dialog 关闭的回调           | —        |
| closed   | Dialog 关闭动画结束时的回调 | —        |

```vue
<!-- @close='onDialogClose'监听对话框的关闭对话框 -->
<el-dialog title="添加新用户" :visible.sync="dialogVisible" width="50%" @close='onDialogClose'>
</el-dialog>
```

接着在methods函数中，声明`onDialogClose`函数，用于清空表单。

我的想法很简单，直接将双向绑定的表单数据清空。

```js
methods: {
  // 监听对话框关闭的事件
  onDialogClose(){
    this.form.name='',
    this.form.age='',
    this.form.postion='',
  }
},
```

老师的想法更加高级，通过使用ref属性，为element-ui的表单组件添加引用名称，接着使用$refs可以调用[组件清空表单的方法](https://element.eleme.cn/#/zh-CN/component/form#form-methods)。

| 方法名        | 说明                                                         | 参数                                                         |
| :------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| validate      | 对整个表单进行校验的方法，参数为一个回调函数。该回调函数会在校验结束后被调用，并传入两个参数：是否校验成功和未通过校验的字段。若不传入回调函数，则会返回一个 promise | Function(callback: Function(boolean, object))                |
| validateField | 对部分表单字段进行校验的方法                                 | Function(props: array \| string, callback: Function(errorMessage: string)) |
| resetFields   | 对整个表单进行重置，将所有字段值重置为初始值并移除校验结果   | —                                                            |
| clearValidate | 移除表单项的校验结果。传入待移除的表单项的 prop 属性或者 prop 组成的数组，如不传则移除整个表单的校验结果 | Function(props: array \| string)                             |

```vue
<el-form :model="form" label-width="80px" :rules="formRules" ref='myaddForm'>
</el-form>
```

```js
methods: {
  // 监听对话框关闭的事件
  onDialogClose() {
      //拿到Form表单的引用，调用resetFields
    this.$refs.myaddForm.resetFields();
    console.log("ok");
  },
},
```

### 2.11 处理确认按钮

当点击了确认按钮后，需要对表单的数据进行预校验，当表单验证通过后，再发送ajax请求。

<img src="https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108120933989.png" alt="image-20210812093321898" style="zoom:33%;" />

```vue
<span slot="footer" class="dialog-footer">
  <el-button @click="dialogVisible = false">取 消</el-button>
  <el-button type="primary" @click="onAddNewUser"
    >确 定</el-button
  >
</span>
```

新声明一个`onAddNewUser`的的函数，处理点击确认按钮的事件。Element-ui的表单组件有一个validate方法，用于对整个表单进行校验。

| 方法名   | 说明                                                         | 参数                                          |
| :------- | :----------------------------------------------------------- | :-------------------------------------------- |
| validate | 对整个表单进行校验的方法，参数为一个回调函数。该回调函数会在校验结束后被调用，并传入两个参数：是否校验成功和未通过校验的字段。若不传入回调函数，则会返回一个 promise | Function(callback: Function(boolean, object)) |

```js
onAddNewUser() {
  this.$refs.myaddForm.validate (valid) => {
      //验证不通过直接返回
    if (!valid) return;
    // 需要执行添加业务的处理 发送ajax请求
  });
```

## 3. 项目用到的api

### 3.1 请求根路径

https://www.escook.cn/

### 3.2 获取用户列表

请求方式：

- GET  

请求地址：

- /api/users

请求参数：

- 无  

### 3.3 添加用户

请求方式：

- POST

请求地址：

- /api/users

请求参数：

- name 用户姓名（1 - 15 个字符之间）

- age 用户年龄（1 - 100 之间）

- position 职位（1 - 10 个字符之间）

请求结果：

- status 的值等于 0 表示成功  

### 3.4 删除用户

请求方式：

- delete

请求地址：

- /api/users/:id

请求参数：

- id 要删除的用户的Id（URL参数）

请求结果：

- status 的值等于 0 表示成功  

### 3.5 获取用户信息

请求方式：

- GET

请求地址：

- /api/users/:id

请求参数：

- id 要查询的用户的Id（URL参数）

请求结果：

- status 的值等于 0 表示成功  

## 4. 发送ajax请求

### 4.1 发送添加用户的请求

在UserList.vue中，当点击了确认添加用户后，需要发送ajax请求到服务器。

```js
onAddNewUser() {
  this.$refs.myaddForm.validate(async (valid) => {
    if (!valid) return;
    // 需要执行添加业务的处理 发送ajax请求
    // 注意async的位置
    let { data } = await this.$http.post("/api/users", {
      name: this.form.name,
      age: this.form.age,
      position: this.form.position,
    });
    if (data.status !== 0) return alert("添加失败");
      console.log('添加成功');
    // 添加成功后，关闭对话框
    this.dialogVisible = false;
    // 重新发起ajax请求，刷新列表
    this.getUserList();
  });
},
```

### 4.2 优化信息提示

当出错或成功后，不应该直接在控制台输出或者弹出alert对话框。应该将信息输出到页面来提示用户。

> 如果是完整引入element，则Element 为 Vue.prototype 添加了全局方法 $message。因此在 vue instance 中可以采用本页面中的方式调用 `Message`。

```js
onAddNewUser() {
  this.$refs.myaddForm.validate(async (valid) => {
    if (!valid) return;
    // 需要执行添加业务的处理 发送ajax请求
    // 注意async的位置
    let { data } = await this.$http.post("/api/users", {
      name: this.form.name,
      age: this.form.age,
      position: this.form.position,
    });
    if (data.status !== 0) return this.$message.error('添加用户失败！');
    // 提示添加成功
    this.$message.success('添加成功！');
    
    // 添加成功后，关闭对话框
    this.dialogVisible = false;
    // 重新发起ajax请求，刷新列表
    this.getUserList();
  });
}
```

### 4.3 确认删除弹出框

在UserList.vue中，当点击了删除后，应该弹出对话框提示用户是否删除。使用element ui的弹出框组件，https://element.eleme.cn/#/zh-CN/component/message-box#que-ren-xiao-xi。

![image-20210812103806599](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108121038660.png)

> 调用`$confirm`方法即可打开消息提示，它模拟了系统的 `confirm`。Message Box 组件也拥有极高的定制性，我们可以传入`options`作为第三个参数，它是一个字面量对象。`type`字段表明消息类型，可以为`success`，`error`，`info`和`warning`，无效的设置将会被忽略。注意，第二个参数`title`必须定义为`String`类型，如果是`Object`，会被理解为`options`。在这里我们用了 Promise 来处理后续响应。

```vue
<el-table-column label="操作" width="120">
  <template>
    <div>
      <a href="#">详情</a> &nbsp;
      <a href="#" @click.prevent="onRemove">删除</a>
    </div>
  </template>
</el-table-column>
```

```js
onRemove() {
  this.$confirm("此操作将永久删除该用户, 是否继续?", "提示", {
    confirmButtonText: "确定",
    cancelButtonText: "取消",
    type: "warning",
  })
    .then(() => {
      this.$message.success('删除成功');
    })
    .catch(() => {
      this.$message.info('已取消删除')
    });
},
```

### 4.4 实现删除功能

为了拿到用户的id，应该将默认插槽改为作用域插槽。

```vue
<el-table-column label="操作" width="120">
  <!-- v-slot='{row}' -->
  <!-- #default='{row}' -->
  <template v-slot:default="{ row }">
    <div>
      <a href="#">详情</a> &nbsp;
      <a href="#" @click.prevent="onRemove(row.id)">删除</a>
    </div>
  </template>
</el-table-column>
```

接着当用户点击确认的时候，发起ajax请求，删除数据。

```js
// 点击了删除的链接
onRemove(id) {
  this.$confirm("此操作将永久删除该用户, 是否继续?", "提示", {
    confirmButtonText: "确定",
    cancelButtonText: "取消",
    type: "warning",
  })
    // 点击确认按钮
    .then(async () => {
      let { data } = await this.$http.delete("/api/users/" + id);
      if (data.status !== 0) return this.$message.error("删除用户失败！");
      this.$message.success("删除成功");
      // 重新发起ajax请求，刷新列表数据
      this.getUserList();
    })
    // 点击取消按钮
    .catch(() => {
      this.$message.info("已取消删除");
    });
},
```

### 4.5 声明式导航跳转到用户详情页

将详情的a链接改为`<router-link>`。

```vue
<template v-slot:default="{ row }">
  <div>
    <router-link :to="'/users/'+row.id">详情</router-link> &nbsp;
    <a href="#" @click.prevent="onRemove(row.id)">删除</a>
  </div>
</template>
```

新建并初始化`UserDetails.vue` 组件。

```vue
<template>
  <div>
      <h3>UserDetail 组件</h3>
  </div>
</template>

<script>
export default {
    name:'UserDetail'
}
</script>

<style scoped>

</style>
```

在`router/index.js` 中添加对应的路由规则：

```js
// 导入要被渲染的组件
// 在vue-cli中 @代表src目录
import UserList from '../components/UserList.vue';
import UserDetail from '@/components/UserDetail.vue';


// 声明路由规则
const router=new VueRouter({
    routes:[
        {path:'/',redirect:'/users'},
        {path:'/users',component:UserList},
        {path:'/users/:id',component:UserDetail}
    ],
})
```

### 4.6 查询用户信息

在`router/index.js`，开启props传参。

```js
const router=new VueRouter({
    routes:[
        {path:'/',redirect:'/users'},
        {path:'/users',component:UserList},
        {path:'/users/:id',component:UserDetail,props:true}
    ],
})
```

在`UserDetails.vue` 中接收路由传递的id，并在created生命周期函数中发起ajax请求，查询用户信息，最后将查询到的信息赋值给`userInfo` 。

```js
export default {
  name: "UserDetail",
  data() {
    return {
      userInfo: {},
    };
  },
  // 接收路由传递的id
  props: ["id"],
  // 发起ajax请求，查询用户信息
  created() {
    this.getUserInfo();
  },
  methods: {
    async getUserInfo() {
      let { data } = await this.$http.get("api/users/" + this.id);
      if (data.status !== 0) return this.$message.error("获取用户信息失败");
      this.userInfo = data.data;
      console.log(this.userInfo);
    },
  },
};
```

### 4.7 展示查询的信息

使用element-ui的[卡片组件](https://element.eleme.cn/#/zh-CN/component/card#ji-chu-yong-fa)，展示查询到的用户信息。

```vue
<template>
  <div>
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <span>用户详情</span>
        <el-button style="float: right; padding: 3px 0" type="text"
          >返回</el-button
        >
      </div>
      <div  class="text item">
       <p>姓名：{{userInfo.name}}</p>
       <p>年龄：{{userInfo.age}}</p>
       <p>头衔：{{userInfo.position}}</p>
      </div>
    </el-card>
  </div>
</template>
```

为返回按钮添加点击事件的监听

```vue
<el-button style="float: right; padding: 3px 0" type="text" @click='goBack'>
    返回
</el-button>
```

通过编程式导航，实现后退功能。

```vue
goBack(){
    this.$router.go(-1);
}
```

## 5.实现loading效果

使用element-ui的[loading组件](https://element.eleme.cn/#/zh-CN/component/loading#zheng-ye-jia-zai)，实现加载效果。

```js
//1. 按需导入Loading效果组件
import {Loading} from 'element-ui'

//2. 声明变量，用来存储loading组件的实例对象
let loadingInstance = null;

// 配置请求的拦截器
axios.interceptors.request.use(config=>{
    // 3.调用loading组件的servie()方法，创建Loading组件的实例，并全屏展示loading效果
    		loadingInstance=Loading.service({fullsreen:true});
 return config;
})
```

```js
// 响应拦截器
axios.interceptors.response.use(response=>{
    // 调用loading实例的close 方法即可关闭loading效果
    loadingInstance.close();
    return response;
})
```



