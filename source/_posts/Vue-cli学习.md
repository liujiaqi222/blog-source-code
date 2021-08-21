---
title: Vue-cli学习_ElementUI_axios 拦截器_proxy代理
date: 2021-08-11 12:21:24
tags: [Vue,Vue-cli]
---



# Vue-cli

## 1.什么是 vue-cli

vue-cli（俗称：vue 脚手架）是 vue 官方提供的、快速生成 vue 工程化项目的工具。

特点：

① 开箱即用

② 基于 webpack

③ 功能丰富且易于扩展

④ 支持创建 vue2 和 vue3 的项目

vue-cli 的中文官网首页：https://cli.vuejs.org/zh/ 



## 2. 安装 vue-cli

vue-cli 是基于 Node.js 开发出来的工具，因此需要使用 npm 将它安装为全局可用的工具：  

```bash
npm install -g @vue/cli

# 查看vue-cli的版本，检查是否安装成功
vue --version
```

## 3. 创建项目

vue-cli 提供了创建项目的两种方式：  

```bash
# 基于命令行创建vue项目
vue create 项目名称
# OR基于可视化面板创建vue项目
vue ui
```

## 4.基于 vue ui 创建 vue 项目

步骤1：在终端下运行 vue ui 命令，自动在浏览器中打开创建项目的可视化面板：  

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111236849.png" alt="image-20210811123643781" style="zoom:50%;" />

步骤2：在详情页面填写项目名称：  

<img src="C:\Users\liujiaqi\AppData\Roaming\Typora\typora-user-images\image-20210811133059381.png" alt="image-20210811133059381" style="zoom:50%;" />

步骤3：在预设页面选择手动配置项目：  

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111331143.png" alt="image-20210811133142100" style="zoom:50%;" />

步骤4：在功能页面勾选需要安装的功能（Choose Vue Version、Babel、CSS 预处理器、**使用配置文件**）：  

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111332107.png" alt="image-20210811133258059" style="zoom:50%;" />

步骤5：在配置页面勾选 vue 的版本和需要的预处理器：  

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111333025.png" alt="image-20210811133339970" style="zoom:50%;" />

步骤6：将刚才所有的配置保存为预设（模板），方便下一次创建项目时直接复用之前的配置：  

![image-20210811133451758](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111334801.png)

步骤7：创建项目并自动安装依赖包：  

vue ui 的本质：通过可视化的面板**采集**到用户的配置信息后，在后台**基于命令行的方式**自动初始化项目。 

项目创建完成后，自动进入项目仪表盘：  

![image-20210811133810035](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111338126.png)

## 5. 基于命令行创建 vue 项目

步骤1：在终端下运行 vue create demo2 命令，基于交互式的命令行创建 vue 的项目：  通过上下箭头选择`manually select features`。

![image-20210811135039039](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111350085.png)

步骤2：选择要安装的功能：  

![image-20210811135314263](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111353298.png)

步骤3：使用上下箭头选择 vue 的版本，并使用回车键确认选择： 

![image-20210811135404359](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111354393.png) 

步骤4：使用上下箭头选择要使用的 css 预处理器，并使用回车键确认选择：  

步骤5：使用上下箭头选择如何存储插件的配置信息，并使用回车键确认选择：  

![image-20210811135441797](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111354834.png)

步骤6：是否将刚才的配置保存为预设：  

步骤7：选择如何安装项目中的依赖包：  

步骤8：开始创建项目并自动安装依赖包：  

![image-20210811135535468](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111355504.png)

步骤9：项目创建完成：  

![image-20210811135600817](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111356847.png)

## 6. 梳理 vue2 项目的基本结构

主要的文件：src -> App.vue，src -> main.js  

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111358164.png" alt="image-20210811135839127" style="zoom:50%;" />

## 7. 分析 main.js 中的主要代码  

```js
// 1. 导入Vue的构造函数
import Vue from 'vue'
// 2. 导入App根组件
import App from './App.vue'

//console面板的提示信息
Vue.config.productionTip = false

// 3. 创建vue实例对象
const app=new Vue({
  render: h => h(App), // 3.1使用render函数渲染App根组件
});

app.$mount('#app') // 3.2 把App根组件渲染到$mount 指定的el区域
```

## 8. 在 vue2 的项目中使用路由

在 vue2 的项目中，只能安装并使用 3.x 版本的 vue-router。

版本 3 和版本 4 的路由**最主要**的区别：**创建路由模块的方式不同**！  

### 8.1 回顾：4.x 版本的路由如何创建路由模块  

![image-20210811141623444](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111416517.png)

### 8.2 学习：3.x 版本的路由如何创建路由模块

步骤1：在 vue2 的项目中安装 3.x 版本的路由：  

```bash
npm install vue-router
```

步骤2：在 src -> components 目录下，创建需要使用路由切换的组件：  

![image-20210811142250768](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111422815.png)

步骤3：在 src 目录下创建 router -> index.js 路由模块：  

```js
// 1.导入vue2的构造函数
import Vue from 'vue';  
// 2.导入3.x路由的构造函数
import VueRouter from 'vue-router';

// 3.导入需要使用路由切换的两个组件
// 由vue-cli创建的项目，src目录的别名为@
import Home from '@/components/Home.vue';
import Movie from '../components/Movie.vue';

// 4.调用Vue.use()函数，把路由配置为Vue的插件
Vue.use(VueRouter);

// 5. 创建路由对象
const router= new VueRouter({
    routes:[
        {path:'/',redirect:'/home'},
        {path:'/home',component:Home},
        {path:'/movie',component:movie},
    ]
})

// 6.向外共享路由对象
export default router;
```

步骤4：在 main.js 中导入路由模块，并通过 router 属性进行挂载：  

# 组件库

## 1.什么是 vue 组件库  

在实际开发中，前端开发者可以把自己封装的 .vue 组件整理、打包、并发布为 npm 的包，从而供其他人下载和使用。这种可以直接下载并在项目中使用的现成组件，就叫做 vue 组件库。  

## 2. vue 组件库和 bootstrap 的区别

二者之间存在本质的区别：

- bootstrap 只提供了纯粹的原材料（ css 样式、HTML 结构以及 JS 特效），需要由开发者做进一步的组装和改造。
- vue 组件库是遵循 vue 语法、高度定制的现成组件，开箱即用  。

## 3. 最常用的 vue 组件库

① PC 端

- Element UI（https://element.eleme.cn/#/zh-CN）

- View UI（http://v1.iviewui.com/）

② 移动端

- Mint UI（http://mint-ui.github.io/#!/zh-cn）

- Vant（https://vant-contrib.gitee.io/vant/#/zh-CN/）  

## 4. Element UI

Element UI 是饿了么前端团队开源的一套 PC 端 vue 组件库。支持在 vue2 和 vue3 的项目中使用：

- vue2 的项目使用旧版的 Element UI（https://element.eleme.cn/#/zh-CN）

- vue3 的项目使用新版的 Element Plus（https://element-plus.gitee.io/#/zh-CN）  

### 4.1 在 vue2 的项目中安装 element-ui

运行如下的终端命令：  

```bash
npm install element-ui
```

### 4.2 引入 element-ui

开发者可以一次性完整引入所有的 element-ui 组件，或是根据需求，只按需引入用到的 element-ui 组件：

- 完整引入：操作简单，但是会额外引入一些用不到的组件，导致项目体积过大
-  按需引入：操作相对复杂一些，但是只会引入用到的组件，能起到优化项目体积的目的  

### 4.3 完整引入

在 main.js 中写入以下内容：  

```js
import Vue from 'vue';
//1.完整引入element-ui 的组件
import ElementUI from 'element-ui';
// 2.导入element-ui的样式
import 'element-ui/lib/theme-chalk/index.css';
import App from './App.vue';

// 3.把element-ui注册为vue的插件（注册后便可以在每一个组件中直接使用element ul的组件）
Vue.use(ElementUI);

new Vue({
  el: '#app',
  render: h => h(App)
});
```

### 4.4 按需引入

借助 `babel-plugin-component`，我们可以只引入需要的组件，以达到减小项目体积的目的。

步骤1，安装 babel-plugin-component：  

```bash
npm install babel-plugin-component -D
```

步骤2，修改根目录下的 babel.config.js 配置文件，新增 plugins 节点如下：  

```js
module.exports = {
  presets: [
    '@vue/cli-plugin-babel/preset'
  ],
  "plugins": [
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]
  ]
}
```

步骤3，如果你只希望引入部分组件，比如 Button，那么需要在 main.js 中写入以下内容：  

```js
import Vue from 'vue';
import { Button, Select } from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import App from './App.vue';

// 全局注册
Vue.component(Button.name, Button);
Vue.component(Select.name, Select);
/* 或写为
 * Vue.use(Button)
 * Vue.use(Select)
 */

new Vue({
  el: '#app',
  render: h => h(App)
});
```

### 4.5 把组件的导入和注册封装为独立的模块

在 src 目录下新建 element-ui/index.js 模块，并声明如下的代码：  

```js
import { Button, Select } from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import Vue from 'vue';

Vue.use(Button);
Vue.use(Select);
```

```js
//main.js
import './element-ui/';
```

# axios拦截器

## 1. 回顾：在 vue3 的项目中全局配置 axios  

```js
import { createApp } from 'vue'
import App from './components/06.network/App.vue'
import './index.css'
// 1.导入axios
import axios from 'axios';

const app=createApp(App);
//2.配置请求根路径
axios.defaults.baseURL='http://www.escook.cn';

// 3.全局挂载axios
app.config.globalProperties.$http=axios;

app.mount('#app');
```

## 2. 在 vue2 的项目中全局配置 axios

需要在 main.js 入口文件中，通过 Vue 构造函数的 prototype 原型对象全局配置 axios：  

```js
import Vue from 'Vue';
import App from './App.vue';
//1.导入axios
import axios from 'axios';

//2.配置请求路径
axios.defaults.baseURL='https://www.escook.cn';

//3.通过vue构造函数的原型对象，全局配置axios
Vue.prototype.$http=axios

new Vue({
    render:h=>h(App)
}).$mount('#app');

```

## 3. 什么是拦截器

拦截器（英文：Interceptors）会在每次发起 ajax 请求或得到响应的时候自动被触发。

![image-20210811163040844](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111630897.png)

应用场景：

① Token 身份认证

② Loading 效果

## 4. 配置请求拦截器

通过 `axios.interceptors.request.use(成功的回调, 失败的回调)` 可以配置请求拦截器。示例代码如下：

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    // 一定要把config return 出去
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });
```

注意：失败的回调函数可以被省略！  

### 4.1 请求拦截器 – Token 认证  

```js
import axios from 'axios';

axios.defaults.baseURL='https://www.escook.cn';

//配置请求拦截器
axios.interceptors.request.use(config => {
    //为当前请求配置token认证字段
    config.headers.Authorization='Bearer xxx';
    // 一定要把config return出去
    return config;
})

Vue.prototype.$http=axios;
```

### 4.2 请求拦截器 – 展示 Loading 效果

借助于 element ui 提供的 Loading 效果组件 （https://element.eleme.cn/#/zh-CN/component/loading） 可以方便的实现 Loading 效果的展示：  

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

不过，当加载完成后，loading的效果并没有被关闭，因此需要使用后面的响应拦截器。

## 5. 配置响应拦截器

通过 `axios.interceptors.response.use(成功的回调, 失败的回调)` 可以配置响应拦截器。示例代码如下：

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

### 5.1 响应拦截器 – 关闭 Loading 效果

调用 Loading 实例提供的 close() 方法即可关闭 Loading 效果，示例代码如下：  

```js
// 响应拦截器
axios.interceptors.response.use(response=>{
    // 调用loading实例的close 方法即可关闭loading效果
    loadingInstance.close();
    return response;
})
```

# proxy 跨域代理

## 1. 回顾：接口的跨域问题  

vue 项目运行的地址：http://localhost:8080/

API 接口运行的地址：https://www.escook.cn/api/users  

由于当前的 API 接口没有开启 CORS 跨域资源共享，因此默认情况下，上面的接口无法请求成功！  

![image-20210811170621432](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111706491.png)

## 2. 通过代理解决接口的跨域问题

通过 vue-cli 创建的项目在遇到接口跨域问题时，可以通过代理的方式来解决：

![image-20210811170744136](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108111707217.png)

① 把 axios 的请求根路径设置**为 vue 项目的运行地址**（接口请求不再跨域）

② vue 项目发现请求的接口不存在，把请求转交给 proxy 代理

③ 代理把请求根路径替换为 devServer.proxy 属性的值，发起真正的数据请求

④ 代理把请求到的数据，转发给 axios  

## 3. 在项目中配置 proxy 代理

步骤1，在 main.js 入口文件中，把 axios 的请求根路径改造为当前 web 项目的根路径：  

```js
//配置请求路径
// axios.defaults.baseURL='https://www.escook.cn';
axios.defaults.baseURL='http://localhost:8080/';
```

步骤2，在项目**根目录**下创建 vue.config.js 的配置文件（是项目根目录，不是src目录下），并声明如下的配置：

```js
module.exports={
    devServer:{
        //当前项目在开发调试阶段会将
        // 任何未知请求配置（没有匹配到静态文件的请求）
        // 代理到 https://www.escook.cn
        proxy:'https://www.escook.cn',
    }
}
```



注意：

① devServer.proxy 提供的代理功能，仅在开发调试阶段生效

② 项目上线发布时，依旧需要 API 接口服务器开启 CORS 跨域资源共享  

