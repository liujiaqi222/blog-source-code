---
title: Vue小案例|TodoList_购物车_table案例
date: 2021-08-10 09:19:08
tags: [Vue, 小案例]

---

# todo-list小案例

案例效果

![image-20210806172116797](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108061721870.png)

用到的知识点

① vite 创建项目

② 组件的封装与注册

③ props

④ 样式绑定

⑤ 计算属性

⑥ 自定义事件

⑦ 组件上的 v-model  

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108061950527.png" alt="image-20210806195021439" style="zoom:50%;" />

## 1.使用 vite 初始化项目

在终端运行以下的命令，初始化 vite 项目：

```bahs
npm init vite-app todos
```

 使用 vscode 打开项目，并安装依赖项：

```bash
npm install
```

安装 less 语法相关的依赖项：

```bash
npm i less -D  
```

## 2. 梳理项目结构

- 重置 index.css 中的全局样式如下：

  ```css
  :root {
  font-size: 12px;
  }
  body {
  padding: 8px;
  }
  ```

- 重置 App.vue 组件的代码结构如下  :

  ```vue
  <template>
    <div>
      <h1>App 根组件</h1>
    </div>
  </template>
  
  <script>
  export default {
    name: "MyApp",
    data() {
      return {
        // 任务列表数据
        todolist: [
          { id: 1, task: "周一早晨9点开会", done: false },
          { id: 2, task: "周一晚上8点聚餐", done: false },
          { id: 3, task: "准备周三上午的演讲稿", done: true },
        ],
      };
    },
  };
  </script>
  
  <style lang="less" scoped>
    
  </style>
  ```

- 删除 components 目录下的 HelloWorld.vue 组件  

- 在终端运行'npm run dev'命令，把项目运行起来  

## 3.封装 todo-list 组件

### 3.1 创建并注册 TodoList 组件

1. 在 src/components/todo-list/ 目录下新建 `TodoList.vue` 组件： 

   ```vue
   <template>
       <div>TodoList 组件</div>
   </template>
   
   <script>
   export default {
       name:'TodoList',
   }
   </script>
   
   <style lang="less" scoped>
       
   </style>
   ```

2. 在 `App.vue `组件中导入并注册 `TodoList.vue`组件：

   ```vue
   <script>
     import TodoList from './components/todo-list/TodoList.vue';
   export default {
     name: "MyApp",
     components:{
       TodoList
     },
   };
   </script>
   ```

   

3. 在 App.vue 的 template 模板结构中使用注册的 TodoList 组件：

   ```vue
   <template>
     <div>
       <h1>App 根组件</h1>
       <todo-list></todo-list>
     </div>
   </template>
   ```

### 3.2 基于 bootstrap 渲染列表组件  

1. 将 资料 目录下的 css 文件夹拷贝到 src/assets/ 静态资源目录中。

2. 在 main.js 入口文件中，导入 src/assets/css/bootstrap.css 样式表：  

   ```js
   import { createApp } from 'vue'
   import App from './App.vue'
   import './index.css'
   import './assets/css/bootstrap.css'
   
   createApp(App).mount('#app')
   
   ```

3. 根据 bootstrap 提供的列表组（https://v4.bootcss.com/docs/components/list-group/#with-badges）和复选框（https://v4.bootcss.com/docs/components/forms/#checkboxes-and-radios-1）渲染列表组件的基本效果：

   ```vue
   <template>
     <ul class="list-group">
       <!-- 列表组 -->
       <li class="list-group-item d-flex justify-content-between align-items-center">
         <!-- 复选框 -->
         <div class="custom-control custom-checkbox">
           <input type="checkbox" class="custom-control-input" id="customCheck1" />
           <label class="custom-control-label" for="customCheck1">Check this custom checkbox
           </label>
         </div>
         <!-- 徽标 -->
         <span class="badge badge-success badge-pill">完成</span>
         <span class="badge badge-warning badge-pill">未完成</span>
       </li>
     </ul>
   </template>
   
   <script>
   export default {
     name: "TodoList",
   };
   </script>
   
   <style lang="less" scoped>
   </style>
   ```

### 3.3 为 TodoList 声明 props 属性  

1. 为了接受外界传递过来的列表数据，需要在 TodoList 组件中声明如下的 props 属性：

   ```vue
   <script>
   export default {
     name: "TodoList",
     props:{
       //   列表数据
         list:{
             type:Array,
             required:true,
             default:[]
         }
     }
   };
   </script>
   ```

2. 在 App 组件中通过 list 属性，将数据传递到 TodoList 组件之中：  

   ```vue
   <template>
     <div>
       <h1>App 根组件</h1>
       <todo-list :list='todolist'></todo-list>
     </div>
   </template>
   ```

### 3.4 渲染列表的 DOM 结构

1. 通过 v-for 指令，循环渲染列表的 DOM 结构：

   ```vue
   <template>
     <ul class="list-group">
       <!-- 列表组 -->
       <li class="list-group-item d-flex justify-content-between align-items-center" :key='item.id' v-for='item in list'>
         <!-- 复选框 -->
         <div class="custom-control custom-checkbox">
           <input type="checkbox" class="custom-control-input" :id="item.id" />
           <label class="custom-control-label" :for="item.id">{{item.task}}</label>
         </div>
         <!-- 徽标 -->
           <span class="badge badge-success badge-pill" v-if='item.done'>完成</span>
           <span class="badge badge-warning badge-pill" v-else>未完成</span>
       </li>
     </ul>
   </template>
   ```

2. 通过 v-if 和 v-else 指令，按需渲染 badge 效果：

   ```vue
   <span class="badge badge-success badge-pill" v-if='item.done'>完成</span>
   <span class="badge badge-warning badge-pill" v-else>未完成</span>
   ```

3. 通过 v-model 指令，双向绑定任务的完成状态：

   因为父组件提供的todolist数据是一个非常复杂的数组，因此使用proprs传递数据时传递的是**引用**。在子组件循环该数组时，使用v-model对input进行绑定时，修改的会是原数据。

   ```vue
   <!-- 复选框 -->
   <input type="checkbox" class="custom-control-input" :id="item.id"
   v-model="item.done" />
   <!-- 注意：App 父组件通过 props 传递过来的 list 是“引用类型”的数， -->
   <!-- 这里 v-model 双向绑定的结果是：用户的操作修改的是 App 组件中数据的状态 -->
   ```

4. 通过 v-bind 属性绑定，动态切换元素的 class 类名：  

   ```vue
   <style lang="less" scoped>
   // 为列表设置高度
   .list-group{
       max-width: 600px;
       margin: 50px auto;
   }
   
   // 删除效果
   .delete{
       text-decoration: line-through;
       color:gray;
       font-style: italic;
   }
   </style>
   ```

   ```vue
   <label class="custom-control-label" :for="item.id" :class='item.done?"delete":""'>{{item.task}}</label>
   ```

## 4. 封装 todo-input 组件

### 4.1 创建并注册 TodoInput 组件  

1. 在 src/components/todo-input/ 目录下新建 TodoInput.vue 组件：

   ```vue
   <template>
       <div>Todo Input 组件</div>
   </template>
   
   <script>
   export default {
       name:'TodoInput'
   }
   </script>
   
   <style lang='less' scoped>
   
   </style>
   ```

2. 在 App.vue 组件中导入并注册 TodoInput.vue 组件：

   ```vue
   <script>
   import TodoList from "./components/todo-list/TodoList.vue";
   import TodoInput from './components/todo-input/todoinput.vue'
   export default {
     name: "MyApp",
     components: {
       TodoList,TodoInput
     },
   };
   </script>
   ```

   

3. 在 App.vue 的 template 模板结构中使用注册的 TodoInput 组件：

   ```vue
   <template>
     <div>
       <h1>App 根组件</h1>
       <todo-input></todo-input>
       <hr>
       <todo-list :list="todolist"></todo-list>
     </div>
   </template>
   ```

### 4.2 基于 bootstrap 渲染组件结构  

1. 根据 bootstrap 提供的 inline-forms（https://v4.bootcss.com/docs/components/forms/#inline-forms）渲染 TodoInput 组件的基本结构。

   

2. 在 TodoInput 组件中渲染如下的 DOM 结构：

   ```vue
   <template>
   
       <form class="form-inline">
           <div class="input-group mb-2 mr-sm-2">
               <div class="input-group-prepend">
                   <div class="input-group-text">任务</div>
               </div>
               <input type="text" class="form-control" placeholder="请输入任务..." style="width:320px">
           </div>
           <button type="submit" class="btn btn-primary mb-2">添加</button>
       </form>
   
   </template>
   
   <script>
   export default {
       name:'TodoInput'
   }
   </script>
   
   <style lang='less' scoped>
       form{
           margin: 20px auto 0;
           width: 550px;
           display: flex;
           justify-content: center;
       }
       
   </style>
   ```

### 4.3 通过自定义事件向外传递数据

![image-20210806200940579](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108062009652.png)

1. 在 TodoList 组件的 data 中声明如下的数据：

   ```vue
   data(){
       return{
           // 新任务名称
           taskname:''
       }
   }
   ```

2. 为 input 输入框进行 v-model 的双向数据绑定：

   ```vue
   <input type="text" class="form-control" placeholder="请输入任务..." style="width:320px" v-model.trim="taskname">
   
   ```

3. 监听 form 表单的 submit 事件，阻止默认提交行为并指定事件处理函数：

   ```vue
    <form class="form-inline" @submit.prevent="onFormSubmit"></form>
   ```

4. 在 methods 中声明 onFormSubmit 事件处理函数如下：

   ```vue
   methods: {
       // 表单提交处理函数
       onFormSubmit(){
           // 1.判断任务名称是否为空
           if(!this.taskname) return alert('待办事项不能为空！');
           // 2.触发自定义的add事件
   
           // 3.清空文本框
       }
   },
   ```

5. 声明自定义事件如下：

   ```vue
   export default {
       name:'TodoInput',
       // 声明自定义事件
       emits:['add'],
   }
   ```

   

6. 进一步完善 onFormSubmit 事件处理函数如下：

   ```vue
   methods: {
       // 表单提交处理函数
       onFormSubmit(){
           // 1.判断任务名称是否为空
           if(!this.taskname) return alert('待办事项不能为空！');
           // 2.触发自定义的add事件
           this.$emit('add',this.taskname);
           // 3.清空文本框
           this.taskname='';
       }
   },
   ```

### 4.4 实现添加任务的功能  

1. 在 App.vue 组件中监听 TodoInput 组件自定义的 add 事件：

   ```vue
   <todo-input @add='onAddNewTask'></todo-input>
   ```

2. 在 App.vue 组件的 methods 中声明 onAddNewTask 事件处理函数如下：

   ```vue
   methods: {
     onAddNewTask(task) {
       //向任务列表中新增任务信息
       this.todolist.push({
         id:this.todolist.length+1,
         task:task,
         done:false
       })
     },
   },
   ```

## 5. 封装 todo-button 组件  

### 5.1 创建并注册 TodoButton 组件

1. 在 src/components/todo-button/ 目录下新建 TodoButton.vue 组件：

2. 在 App.vue 组件中导入并注册 TodoButton.vue 组件：

   ```vue
   <script>
   import TodoList from "./components/todo-list/TodoList.vue";
   import TodoInput from "./components/todo-input/todoinput.vue";
   import TodoButton from './components/todo-button/TodoButton.vue'
   export default {
     name: "MyApp",
     components: {
       TodoList,
       TodoInput,
       TodoButton
     },
   };
   </script>
   ```

### 5.2 基于 bootstrap 渲染组件结构

1. 根据 bootstrap 提供的 Button group（https://v4.bootcss.com/docs/components/button-group/）渲染 Todobutton 组件的基本结构。

2. 在 TodoButton 组件中渲染如下的 DOM 结构：  


   ```vue
<template>
  <div class="mt-3 btn-container">
    <div class="btn-group" role="group" aria-label="Basic example">
      <button type="button" class="btn btn-primary">全部</button>
      <button type="button" class="btn btn-secondary">已经完成</button>
      <button type="button" class="btn btn-secondary">未完成</button>
    </div>
  </div>
</template>
   ```

3.  并通过 button-container 类名美化组件的样式：


   ```vue
<style lang="less" scoped>
.btn-container{
    max-width: 450px;
    margin:0px auto;
    
    text-align: center;
}
</style>
   ```


### 5.3 给按钮添加点击事件

1. 在 TodoButton 声明active变量，用于记录当前激活的按钮，默认为0。

   ```vue
   <script>
   export default {
     name: "TodoButton",
     data(){
       return{
         active:0,
       }
     },
   
   };
   </script>
   ```

2. 通过 动态绑定 class 类名 的方式控制按钮的激活状态：

   ```vue
   <template>
     <div class="mt-3 btn-container">
       <div class="btn-group" role="group" aria-label="Basic example">
         <button type="button" class="btn" :class="active==0?'btn-primary':'btn-secondary'">全部</button>
         <button type="button" class="btn" :class="active==1?'btn-primary':'btn-secondary'">已经完成</button>
         <button type="button" class="btn" :class="active==2?'btn-primary':'btn-secondary'">未完成</button>
       </div>
     </div>
   </template>
   ```

3. 监听按钮的点击事件，并传递按钮的索引。

   ```vue
   <button type="button" class="btn" :class="active==0?'btn-primary':'btn-secondary'" @click='onBtnClick(0)'>
     全部</button>
   <button type="button" class="btn" :class="active==1?'btn-primary':'btn-secondary'" @click='onBtnClick(1)'>
       已经完成</button>
   <button type="button" class="btn" :class="active==2?'btn-primary':'btn-secondary'" @click='onBtnClick(2)'>
       未完成</button>
   ```

4. 当点击按钮时，把按钮的索引赋值给active，并将按钮的索引通过自定义事件传递给出去。

   ```vue
   <script>
   export default {
     name: "TodoButton",
     emits:['button-change'],
     methods: {
       onBtnClick(value){
         this.active=value;
           //把按钮的索引值传递出去
         this.$emit('button-change',value);
       }
     },
   };
   </script>
   ```

### 5.4 通过计算属性动态切换列表的数据  

1. App.vue接收子组件传递的按钮激活索引。

   声明一个激活按钮索引名的变量，默认值 为0。

   ```vue
   data() {
     return {
       // 任务列表数据
       todolist: [
         { id: 1, task: "周一早晨9点开会", done: false },
         { id: 2, task: "周一晚上8点聚餐", done: false },
         { id: 3, task: "准备周三上午的演讲稿", done: true },
       ],
       btn_index:0
     };
   },
   ```

   然后通过监听自定义事件，把子组件传递的索引值赋值给btn_index。

   ```vue
   <todo-button @button-change="list_change"></todo-button>
   ```

   ```vue
   methods: {
     list_change(value) {
       // 接收此时的按钮索引
       this.btn_index=value;
     },
   },
   ```

2. 根据btn_index值，此时可以根据当前激活按钮的索引，动态计算出要显
   示的列表数据并返回即可，因此在 App 根组件中声明如下的计算属性 。

   ```vue
   computed:{
     tasks(){
       switch(this.btn_index){
         case 0: return this.todolist;
         case 1:return this.todolist.filter(item=>item.done);
         case 2: return this.todolist.filter(item=>!item.done);
       }
     }
   }
   ```


   3. 在 App 根组件的 DOM 结构中，将 TodoList 组件的 :list="todolist" 修改为：

      ```vue
      <todo-list :list="tasks" class="mt-2"></todo-list>
      ```

# 购物车小案例

## 0.实现步骤

① 初始化项目基本结构

② 封装 EsHeader 组件

③ 基于 axios 请求商品列表数据

④ 封装 EsFooter 组件

⑤ 封装 EsGoods 组件

⑥ 封装 EsCounter 组件  

## 1. 初始化项目结构

在终端运行以下的命令，初始化 vite 项目：

```bahs
npm init vite-app code-cart
cd code-cart
```

 使用 vscode 打开项目，并安装依赖项：

```bash
npm install
```

安装 less 语法相关的依赖项：

```bash
npm i less -D  
```

初始化 index.css 全局样式如下：

```css
:root {
font-size: 12px;
}
```

清理项目结构：

- 把 bootstrap 相关的文件放入 src/assets 目录下

- 在 main.js 中导入 bootstrap.css

- 清空 App.vue 组件

- 删除 components 目录下的 HelloWorld.vue 组件  

## 2. 封装 es-header 组件

### 2.1 创建并注册 EsHeader 组件

1.在 src/components/es-header/ 目录下新建 EsHeader.vue 组件：

2.在 App.vue 组件中导入并注册 EsHeader.vue 组件：

3.在 App.vue 的 template 模板结构中使用 EsHeader 组件：

### 2.2 封装 es-header 组件

0.封装需求

- 允许用户自定义 title 标题内容

- 允许用户自定义 color 文字颜色
- 允许用户自定义 bgcolor 背景颜色
- 允许用户自定义 fsize 字体大小
- es-header 组件必须固定定位到页面顶部的位置，高度为 45px，文本居中，z-index 为 999  

1.在 es-header 组件中封装以下的 props 属性：

```vue
<script>
export default {
  name: "EsHeader",
  props: {
    title: {
      // 标题内容
      type: String,
      default: "es-header",
    },
    bgcolor: {
      // 背景颜色
      type: String,
      default: "#007BFF",
    },
    color: {
      // 文字颜色
      type: String,
      default: "#ffffff",
    },
    fsize: {
      // 文字大小
      type: Number,
      default: 12,
    },
  },
};
</script>
```

2.渲染标题内容，并动态为 DOM 元素绑定行内的 style 样式对象：

```vue
<template>
  <div
    :style="{ color: color, backgroundColor: bgcolor, fontSize: fsize + 'px' }">
    {{ title }}
  </div>
</template>
```

3.为 DOM 节点添加 header-container 类名，进一步美化 es-header 组件的样式：

```vue
<template>
    <div class='header-container'  :style='{backgroundColor:bgcolor,color:color,fontSize:fsize+"px"}' >
        {{title}}
    </div>
</template>


<style lang="less" scoped>
    .header-container{
        position: fixed;
        top:0;
        left:0;
        width: 100%;
        height: 45px;
        text-align: center;
        line-height: 45px;
        z-index: 999;
    }
</style>
```

4.在 App 根组件中使用 es-header 组件时，通过 title 属性 指定 标题内容 ：

```vue
<template>
  <div>
    <h1>App 根组件</h1>
    <es-header title="购物车案例"></es-header>
  </div>
</template>
```

## 3. 基于 axios 请求商品列表数据

### 3.1 全局配置 axios

1.运行如下的命令安装 axios :

```bash
npm i axios
```

2.在 main.js 入口文件中导入并全局配置 axios：

```js
import { createApp } from 'vue'
import App from './App.vue'
import './assets/css/bootstrap.css';
import './index.css'

// 导入axios
import axios from 'axios';

const app=createApp(App);

// 配置请求的根路径
axios.defaults.baseURL='https://www.escook.cn';
// 将axios挂载为全局的$http 自定义属性
app.config.globalProperties.$http=axios;

app.mount('#app');
```

### 3.2 请求商品列表数据

1.在 App.vue 根组件中声明如下的 data 数据：

```js
data() {
	return {
	// 商品列表的数据
	goodslist: [],
	}
}
```

2.在 App.vue 根组件的 created 生命周期函数中，预调用 获取商品列表数据 的
methods 方法：  

```js
// 组件实例创建完毕之后的生命周期函数
created(){
  // 调用methods中的getGoodsList方法，请求商品列表数据
  this.getGoodsList();
}
```

3.在 Ap.vue 根组件的 methods 节点中，声明刚才预调用的 getGoodsList 方法：

```js
methods: {
  // 获取商品列表数据的方法
  async getGoodsList() {
    let res = await this.$http.get("/api/cart");
    if(res.status!==200) return alert('数据请求失败');
    this.goodslist=res.data;
  },
},
```

## 4. 封装 es-footer 组件

### 4.1 创建并注册 EsFooter 组件

1.在 src/components/es-footer/ 目录下新建 EsFooter.vue 组件：

```vue
<template>
    <div>footer</div>
</template>

<script>
export default {
    name:"EsFooter"
}
</script>
```

2.在 App.vue 组件中导入并注册 EsFooter.vue 组件：

```vue
<script>
import EsHeader from "./components/es-header/EsHeader.vue";
import EsFooter from './components/es-footer/EsFooter.vue';
export default {
  name: "MyApp",
  components: {
    EsHeader,EsFooter
  },
};
</script>
```

3.在 App.vue 的 template 模板结构中使用 EsFooter 组件：

```vue
<template>
  <div class="app-container">
    <h1>App 根组件</h1>
    <es-header title="购物车案例"></es-header>
    <es-footer></es-footer>
  </div>
</template>
```

### 4.2 封装 es-footer 组件

#### 4.2.0 封装需求

1.es-footer 组件必须固定定位到 页面底部 的位置，高度为 50px，内容两端贴边对齐，z-index 为 999

2.允许用户自定义 `amount` 总价格（单位是元），并在渲染时 保留两位小数

3.允许用户自定义 `total` 总数量，并渲染到 结算按钮 中；如果要结算的商品数量为0，则禁用结算按钮

4.允许用户自定义 `isfull` 全选按钮的选中状态

5.允许用户通过 `自定义事件` 的形式，监听全选按钮 选中状态的变化 ，并获取到 最新的选中状态

#### 4.2.1 渲染组件的基础布局

1.将 EsFooter.vue 组件在页面底部进行固定定位：

2.根据 bootstrap 提供的 Checkboxes https://v4.bootcss.com/docs/components/forms/#checkboxes 渲染左侧的 全选 按钮：  

```vue
<template>
  <div class="footer-container">
    <!-- 全选区域 -->
    <div class="custom-control custom-checkbox">
      <input type="checkbox" class="custom-control-input" id="fullCheck" />
      <label class="custom-control-label" for="fullCheck">
          全选</label>
    </div>
  </div>
</template>

<script>
export default {
  name: "EsFooter",
};
</script>

<style lang="less" scoped>
.footer-container {
  position: fixed;
  bottom: 0;
  left: 0;
  box-sizing: border-box;
  width: 100%;
  height: 50px;
  background-color: #fff;
  border-top: 1px solid #efefef;
  display: flex;
  justify-content: space-between;
  padding: 10px;
}
</style>
```

并在全局样式表 index.css 中覆盖 全选 按钮的圆角样式：

```css
.custom-checkbox .custom-control-label::before{
  border-radius: 50%;
}
```

3.渲染合计对应的价格区域：

```vue
<template>
  <div class="footer-container">
    <!-- 全选区域 -->
    <div class="custom-control custom-checkbox">
      <input type="checkbox" class="custom-control-input" id="fullCheck" />
      <label class="custom-control-label" for="fullCheck">
          全选</label>
    </div>
    <!-- 合计区域 -->
    <div>
        <span>合计：</span>
        <span class="amount">￥0.00</span>
    </div>
  </div>
    
</template>
```

并在当前组件的 <style> 节点中美化总价格的样式：

```vue
.amount{
    font-weight: bold;
    color:red;
}
```

4.根据 bootstrap 提供的 Buttons https://v4.bootcss.com/docs/components/buttons/#examples 渲染结算按钮：  

```vue
<template>
  <div class="footer-container">
    <!-- 全选区域 -->
    <div class="custom-control custom-checkbox">
      <input type="checkbox" class="custom-control-input" id="fullCheck" />
      <label class="custom-control-label" for="fullCheck"> 全选</label>
    </div>
    <!-- 合计区域 -->
    <div>
      <span>合计：</span>
      <span class="amount">￥0.00</span>
    </div>
    <!-- 结算按钮 -->
    <button type="button" class="btn btn-primary">结算(0)</button>
  </div>
</template>

<script>
export default {
  name: "EsFooter",
};
</script>

<style lang="less" scoped>
.footer-container {
  position: fixed;
  bottom: 0;
  left: 0;
  box-sizing: border-box;
  width: 100%;
  height: 50px;
  background-color: #fff;
  border-top: 1px solid #efefef;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
}
.amount {
  font-weight: bold;
  color: red;
}
button{
    min-width: 90px;
    height: 38px;
    border-radius: 19px;
}
</style>
```

#### 4.2.2 封装自定义属性 amount

> amount 是已勾选商品的总价格

1.在 EsFooter.vue 组件的 props 节点中，声明如下的自定义属性：  

```vue
props:{
//   已勾选商品的总价格
    amount:{
        type:Number,
        default:0,
    },
}
```

2.在 EsFooter.vue 组件的 DOM 结构中渲染 amount 的值：

```vue
<!-- 合计区域 -->
<div>
    <span>合计：</span>
    <span class="amount">￥{{amount.toFixed(2)}}</span>
</div>
```

#### 4.2.3 封装自定义属性 total

> total 为已勾选商品的总数量

1.在 EsFooter.vue 组件的 props 节点中，声明如下的自定义属性：

```vue
<script>
export default {
  name: "EsFooter",
props:{

	// 已经勾选商品的总数量
    total:{
        type:Number,
        default:0,
    }
}
};
</script>
```

2.在 EsFooter.vue 组件的 DOM 结构中渲染 total 的值：

```vue
 <button type="button" class="btn btn-primary">结算{{total}}</button>
```

3.动态控制结算按钮的禁用状态：

```vue
<button type="button" class="btn btn-primary" :disabled='total===0'>结算{{total}}</button>
```

#### 4.2.4 封装自定义属性 isfull

> isfull 是全选按钮的选中状态，true 表示选中，false 表示未选中

1.在 EsFooter.vue 组件的 props 节点中，声明如下的自定义属性：

```vue
props: {
//   已勾选商品的总价格
amount: {
    type: Number,
    default: 0,
},
//   已经勾选商品的总数量
total: {
    type: Number,
    default: 0,
},
//  全选按钮是否选中
isfull:{
    type:Boolean,
    default:false,
}
},
```

2.为复选框动态绑定 ckecked 属性的值：  

```vue
<!-- 全选区域 -->
<div class="custom-control custom-checkbox">
    <input type="checkbox" class="custom-control-input" id="fullCheck"  checked='isfull'/>
    <label class="custom-control-label" for="fullCheck"> 全选</label>
</div>
```

#### 4.2.5 封装自定义事件 fullChange

> 通过自定义事件 fullChange，把最新的选中状态传递给组件的使用者

1.监听复选框选中状态变化的 change 事件：

```vue
<div class="custom-control custom-checkbox">
    <input
    type="checkbox"
    class="custom-control-input"
    id="fullCheck"
    checked="isfull"
    @change='onCheckBoxChange'
    />
    <label class="custom-control-label" for="fullCheck"> 全选</label>
</div>
```

2.在 methods 中声明 onCheckBoxChange ，并通过事件对象 e 获取到最新的选中状态：

```vue
methods: {
//   监听checkbox选中状态变化的事件
    onCheckBoxChange(e){
        this.$emit('fullChange',e.target.checked)
    }
},
```

3.在 emits 中声明自定义事件：  

```vue
  emits:['fullChange'],
```

4.在 onCheckBoxChange 事件处理函数中，通过 $emit() 触发自定义事件，把最新的选中状态传递给当前组件的使用者：  

```vue
methods: {
//   监听checkbox选中状态变化的事件
    onCheckBoxChange(e){
        this.$emit('fullChange',e.target.checked)
    }
},
```

5.在 App.vue 根组件中测试 EsFooter.vue 组件：

```vue
<template>
  <div class="app-container">
    <h1>App 根组件</h1>
    <es-header title="购物车案例"></es-header>
    <es-footer @fullChange="onFullStateChange"></es-footer>
  </div>
</template>
```

并在 methods 中声明 onFullStateChange 处理函数，通过形参获取到全选按钮最新的选中状态：  

```js
methods: {
  // 监听选中状态
  onFullStateChange(isFull){
    console.log(isFull);
  }
},
};
```

## 5. 封装 es-goods 组件

1.在 src/components/es-goods/ 目录下新建 EsGoods.vue 组件：  

2.在 App.vue 组件中导入并注册 EsGoods.vue 组件：

3.在 App.vue 的 template 模板结构中使用 EsGoods 组件：

### 5.2 封装 es-goods 组件

#### 5.2.0 封装需求

1.实现 EsGoods 组件的基础布局

2.封装组件的 6 个自定义属性（id, thumb，title，price，count，checked）

3.封装组件的自定义事件 stateChange ，允许外界监听组件选中状态的变化  

#### 5.2.1 渲染组件的基础布局

1.渲染 EsGoods 组件的基础 DOM 结构：

```vue
<template>
  <div class="goods-container">
    <!-- 左侧图片区域 -->
    <div class="left">
      <!-- 商品的缩略图 -->
      <img src="" alt="商品图片" class="thumb" />
    </div>
    <!-- 右侧信息区域 -->
    <div class="right">
      <!-- 商品名称 -->
      <div class="top">xxxx</div>
      <div class="bottom">
        <!-- 商品价格 -->
        <div class="price">￥0.00</div>
        <!-- 商品数量 -->
        <div class="count">数量</div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "EsGoods",
};
</script>
```

2.美化组件的布局样式：

```vue
<style lang="less">
.goods-container {
  display: flex;
  padding: 10px;
  /* 左侧图片的样式 */
  .left {
    margin-right: 10px;
    /* 商品图片 */
    .thumb {
      display: block;
      width: 100px;
      height: 100px;
      background-color: #efefef;
    }
  }
  /* 右侧商品名称、单价、数量的样式 */
  .right {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .top {
      font-weight: bold;
    }
    .bottom {
      display: flex;
      justify-content: space-between;
      align-items: center;
      .price {
        color: red;
        font-weight: bold;
      }
    }
  }
}
</style>
```

3.在商品缩略图之外包裹复选框( https://v4.bootcss.com/docs/components/forms/#checkboxes )效果：  

```vue
<!-- 左侧图片区域 -->
<div class="left">
    <div class="custom-control custom-checkbox">
    <input type="checkbox" class="custom-control-input" id="customCheck1" />
    <label class="custom-control-label" for="customCheck1">
        <!-- 商品的缩略图 -->
        <img src="" alt="商品图片" class="thumb" />
    </label>
    </div>
</div>
```

4.覆盖复选框的默认样式：

```vue
.custom-checkbox .custom-control-label::before,
.custom-checkbox .custom-control-label::after
{
    top:3.4rem;
}
```

5.在 App.vue 组件中循环渲染 EsGoods.vue 组件：

```vue
<es-goods v-for='item in goodslist' :key='item.id'></es-goods>
```

6.为 EsGoods.vue 添加顶边框：

通过+ 选择器，排除第一项，为其他兄弟元素添加+选择器

```less
<style lang="less">
.goods-container {
    // 最终生成的选择器为 .goods-container + .goods-container
  + .goods-container {
    border-top: 1px solid #efefef;
  }
 // 其他样式省略
}

</style>
```

#### 5.2.2 封装自定义属性 id

> id 是每件商品的唯一标识  

1.在 EsGoods.vue 组件的 props 节点中，声明如下的自定义属性：

```js
export default {
  name: "EsGoods",
  props:{
    //   唯一的key值
    id:{
        type:[String,Number],
        required:true
    }
  }
};
```

2.在渲染复选框时动态绑定 input 的 id 属性和 label 的 for 属性值：

```vue
<div class="custom-control custom-checkbox">
<input type="checkbox" class="custom-control-input" :id="id" />
<label class="custom-control-label" :for="id">
    <!-- 商品的缩略图 -->
    <img src="" alt="商品图片" class="thumb" />
</label>
</div>
```

3.在 App.vue 中使用 EsGoods.vue 组件时，动态绑定 id 属性的值：

```vue
<template>
  <div class="app-container">
    <es-header title="购物车案例"></es-header>
    <es-goods v-for='item in goodslist' :key='item.id' :id='item.id'></es-goods>
    <es-footer @fullChange="onFullStateChange"></es-footer>
  </div>
</template>
```

#### 5.2.3 封装其它属性

> 除了 id 属性之外，EsGoods 组件还需要封装：
> 缩略图（thumb）、商品名称（title）、单价（price）、数量（count）、勾选状态（checked）这 5 个属性  

1.在 EsGoods.vue 组件的 props 节点中，声明如下的自定义属性：

```vue
<script>
export default {
  name: "EsGoods",
  props: {
    //   唯一的key值
    id: {
      type: [String, Number],
      required: true,
    },
    //商品缩略图
    thumb:{
        type:String,
        required:true,
    },
    // 商品名称
    title:{
        type:String,
        required:true,
    },
    // 单价
    price:{
        type:Number,
        required:true,
    },
    // 商品数量
    count:{
        type:Number,
        required:true,
    },
    // 勾选状态
    checked:{
        type:Boolean,
        required:true,
    }
  },
};
</script>
```

2.在 EsGoods.vue 组件的 DOM 结构中渲染商品的信息数据：

```vue
<template>
  <div class="goods-container">
    <!-- 左侧图片区域 -->
    <div class="left">
      <div class="custom-control custom-checkbox">
        <input type="checkbox" class="custom-control-input" :id="id" :checked='checked'/>
        <label class="custom-control-label" :for="id">
          <!-- 商品的缩略图 -->
          <img :src="thumb" alt="商品图片" class="thumb" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="right">
      <!-- 商品名称 -->
      <div class="top">{{title}}</div>
      <div class="bottom">
        <!-- 商品价格 -->
        <div class="price">{{price.tofixed(2)}}</div>
        <!-- 商品数量 -->
        <div class="count">{{count}}</div>
      </div>
    </div>
  </div>
</template>
```

3.App.vue 组件中使用 EsGoods.vue 组件时，动态绑定对应属性的值：

```vue
<template>
  <div class="app-container">
    <es-header title="购物车案例"></es-header>
    <es-goods
      v-for="item in goodslist"
      :key="item.id"
      :id="item.id"
      :thumb="item.goods_img"
      :count="item.goods_count"
      :price="item.goods_price"
      :title="item.goods_name"
      :checked="item.goods_state"
    ></es-goods>
    <es-footer @fullChange="onFullStateChange"></es-footer>
  </div>
</template>
```

#### 5.2.4 封装自定义事件 stateChange

> 点击复选框时，可以把最新的勾选状态，通过自定义事件的方式传递给组件的使用者。

1.在 EsGoods.vue 组件中，监听 checkbox 选中状态变化的事件：

```vue
<input
    type="checkbox"
    class="custom-control-input"
    :id="id"
    :checked="checked"
    @change="onCheckBoxChange"
/>
```

2.在 EsGoods.vue 组件的 methods 中声明对应的事件处理函数：

```js
methods: {
    onCheckBoxChange(e){
        console.log(e.target.checked);
    }
},
```

3.在 EsGoods.vue 组件中声明自定义事件：

```vue
emits:['stateChange']
```

4.完善 onCheckBoxChange 函数的处理逻辑，调用 $emit() 函数触发自定义事件：

```vue
methods: {
    onCheckBoxChange(e){
        this.$emit('stateChange',{
            id:this.id,
            ischecked:e.target.checked
        })
    }
},
```

5.在 App.vue 根组件中使用 EsGoods.vue 组件时，监听它的 stateChange 事件：

```vue
<es-goods
      v-for="item in goodslist"
      :key="item.id"
      :id="item.id"
      :thumb="item.goods_img"
      :count="item.goods_count"
      :price="item.goods_price"
      :title="item.goods_name"
      :checked="item.goods_state"
      @stateChange='onGoodsStateChange'
></es-goods>
```

并在 App.vue 的 methods 中声明如下的事件处理函数：

```vue
// 监听商品变化的事件
onGoodsStateChange(value) {
  this.goodslist.some(item=>{
    if(item.id===value.id){
      item.goods_state=value.ischecked;
      return true;
    }
  })
},
```

## 6. 实现合计、结算数量、全选功能

### 6.1 动态统计已勾选商品的总价格

> 需求分析：
> 合计的商品总价格，依赖于 goodslist 数组中每一件商品信息的变化，此场景下适合使用计算属
> 性。  

1.在 App.vue 中声明如下的计算属性：

```vue
computed:{
  // 已勾选商品的总价格
  amount(){
    let sum=0;
    this.goodslist.filter(item=>item.goods_state===true).forEach(item=>{
      sum+=item.goods_price*item.goods_count;
    });
    return sum;
  }
}
```

2.在 App.vue 中使用 EsFooter.vue 组件时，动态绑定已勾选商品的总价格：

```vue
<es-footer @fullChange="onFullStateChange" :amount='amount'></es-footer>
```

### 6.2 动态统计已勾选商品的总数量

> 需求分析：
> 已勾选商品的总数量依赖项 goodslist 中商品勾选状态的变化，此场景下适合使用计算属性。  

1.在 App.vue 中声明如下的计算属性：

```vue
total() {
  let sum = 0;
  this.goodslist
    .filter((item) => item.goods_state === true)
    .forEach((item) => {
      sum += item.goods_count;
    });
  return sum;
},
```

2.在 App.vue 中使用 EsFooter.vue 组件时，动态绑定已勾选商品的总数量：

```vue
<es-footer @fullChange="onFullStateChange" :amount="amount" :total="total"></es-footer>
```

### 6.3 实现全选功能

1.在 App.vue 组件中监听到 EsFooter.vue 组件的选中状态发生变化时，立即更新
goodslist 中每件商品的选中状态即可：  

```vue
<!-- 使用 footer 组件 -->
<es-footer :total="total" :amount="amount"
@fullChange="onFullStateChange"></es-footer>
```

2.在 onFullStateChange 的事件处理函数中修改每件商品的选中状态：

```vue
// 监听是否全选
onFullStateChange(isFull) {
    this.goodslist.forEach(item=>item.goods_state=isFull)
},
```

## 7. 封装 es-counter 组件

### 7.1 创建并注册 EsCounter 组件

1.在 src/components/es-counter/ 目录下新建 EsCounter.vue 组件：

2.在 EsGoods.vue 组件中导入并注册 EsCounter.vue 组件：

3.在 EsGoods.vue 的 template 模板结构中使用 EsCounter.vue 组件：

### 7.2 封装 es-counter 组件

#### 7.2.0 封装需求

1.渲染组件的 基础布局

2.实现数量值的 加减操作

3.处理 min 最小值

4.使用 watch 侦听器处理文本框输入的结果

5.封装 numChange 自定义事件  

> 代码示例

```vue
<es-counter :num="count" :min="1" @numChange="getNumber"></es-counter>
```

#### 7.2.1 渲染组件的基础布局

1.基于 bootstrap 提供的 Buttons https://v4.bootcss.com/docs/components/buttons/#examples 和 form-control 渲染组件的基础布局：  

```vue
<template>
  <div class="counter-container" id="counter">
    <!-- 数量 -1 按钮 -->
    <button type="button" id="button1" class="btn btn-light btn-sm">-</button>
    <!-- 输入框 -->
    <input
      type="number"
      id="input"
      class="form-control form-control-sm iptnum"
    />
    <!-- 数量 +1 按钮 -->
    <button type="button" id="button2" class="btn btn-light btn-sm">+</button>
  </div>
</template>
```

2.美化当前组件的样式：

```vue
<style lang='css' scoped>
.counter-container {
  display: flex;
  justify-content: center;
  align-items: center;
}
#input {
  box-sizing: border-box;
  width: 52px;
  padding-left: 8px;
  height: 25px;
  text-align: center;
}

#button1,
#button2 {
  width: 25px;
  height: 25px;
}
</style>
```

#### 7.2.2 实现数值的渲染及加减操作

> 思路分析：
> 1.加减操作需要依赖于 EsCounter 组件的 data 数据
>
> 2.初始数据依赖于父组件通过 props 传递进来
> 将父组件传递进来的 props 初始值转存到 data 中，形成 EsCounter 组件的内部状态！  

1.在 EsCounter.vue 组件中声明如下的 props：

```vue
props:{
    num:{
        type:Number,
        default:0
    }
}
```

2.在 EsGoods.vue 组件中通过属性绑定的形式，将数据传递到 EsCounter.vue 组件中：

```vue
<div class="count"><es-counter :num='count'></es-counter></div>
```

注意：不要直接把 num 通过 v-model 指令双向绑定到 input 输入框，因为 vue
规定：props 的值只读的！例如下面的做法是错误的：  

```vue
<!-- Warning 警告：不要模仿下面的操作 -->
<input type="number" class="form-control form-control-sm ipt-num"
v-model.number="num" />
```

3.正确的做法：将 props 的初始值转存到 data 中，因为 data 中的数据是可读可写的！示例代码如下：  

```vue
<script>
export default {
  name: "EsCounter",
  props: {
    // 初始数量值【只读数据】
    num: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      // 内部状态值【可读可写的数据】
      // 通过 this 可以访问到 props 中的初始值
      number: this.num,
    };
  },
};
</script>
```

并且把 data 中的 number 双向绑定到 input 输入框：

```vue
<input
    type="number"
    id="input"
    class="form-control form-control-sm iptnum"
    v-model="number"
/>
```

4.为 -1 和 +1 按钮绑定响应的点击事件处理函数：

```vue
<button type="button" class="btn btn-light btn-sm"
@click="onSubClick">-</button>
<input type="number" class="form-control form-control-sm ipt-num"
v-model.number="number" />
<button type="button" class="btn btn-light btn-sm"
@click="onAddClick">+</button
```

并在 methods 中声明对应的事件处理函数如下：  

```vue
methods: {
// -1 按钮的事件处理函数
onSubClick() {
this.number -= 1
},
// +1 按钮的事件处理函数
onAddClick() {
this.number += 1
},
},
```

#### 7.2.3 实现 min 最小值的处理

> 需求分析：
> 购买商品时，购买的数量最小值为 1  

1.在 EsCounter.vue 组件中封装如下的 props：

```vue
props: {
// 初始数量值【只读数据】
num: {
    type: Number,
    default: 0,
},
min:{
    type:Number,
    // 表示不指定最小值
    default:NaN,
}
},
```

2.在 -1 按钮的事件处理函数中，对 min 的值进行判断和处理：

```js
methods: {
	onSubClick(){
    // min 的值存在，且 number - 1 之后小于 min
        if(!isNaN(this.min)&&(this.number-1<this.min)){
        return;
        }else{
            this.number-=1;
        }
    },
}
```

3.在 EsGoods.vue 组件中使用 EsCounter.vue 组件时指定 min 最小值：

```vue
<div class="count"><es-counter :num='count' :min='0'></es-counter></div>
```

#### 7.2.4 处理输入框的输入结果

> 思路分析：
> 1.将输入的新值转化为整数
> 2.如果转换的结果不是数字，或小于1，则强制 number 的值等于1
> 3.如果新值为小数，则把转换的结果赋值给 number  

1.为输入框的 v-model 指令添加 .lazy 修饰符（当输入框触发 change 事件时更新 vmodel 所绑定到的数据源）：

```vue
<input type="number" class="form-control form-control-sm ipt-num" v-model.number.lazy="number" />
```

2.通过 watch 侦听器监听 number 数值的变化，并按照分析的步骤实现代码：

```vue
watch:{
    number(newValue){
        const parseResult=parseInt(newValue);
        //如果不为数字
        if(isNaN(parseResult)||parseResult<this.min){
            return this.number=1;
        }
        this.number=parseResult;
    }
}
```

#### 7.2.5 把最新的数据传递给使用者

> 需求分析：
> 当 EsGoods 组件使用 EsCounter 组件时，期望能够监听到商品数量的变化，此时需要使用自定
> 义事件的方式，把最新的数据传递给组件的使用者。  

1.在 EsCounter.vue 组件中声明自定义事件如下：

```vue
emits: ['numChange'],
```

2.在 EsCounter.vue 组件的 watch 侦听器中触发自定义事件：

```js
watch: {
number(newValue) {
    const parseResult = parseInt(newValue);
    //如果不为数字
    if (isNaN(parseResult) || parseResult < this.min) {
    return (this.number = 1);
    }
    this.number = parseResult;
    // 触发自定义事件
    this.$emit("numChange", this.number);
},
},
```

3.在 EsGoods.vue 组件中监听 EsCounter.vue 组件的自定义事件：

```vue
 <div class="count"><es-counter :num='count' :min='1' @numChange="getNumber"></es-counter></div>
```

并声明对应的事件处理函数如下：

```vue
getNumber(val){
    console.log(val);
}
```

#### 7.2.6 更新购物车中商品的数量

> 思路分析：
> 1.在 EsGoods 组件中获取到最新的商品数量
> 2.在 EsGoods 组件中声明自定义事件
> 3.在 EsGoods 组件中触发自定义事件，向外传递数据对象 { id, value }
> 4.在 App 根组件中监听 EsGoods 组件的自定义事件，并根据 id 更新对应商品的数量  

1.在 EsGoods.vue 组件中声明自定义事件 countChange ：

```vue
emits:['stateChange','countChange'],
```

2.在 EsCounter.vue 组件的 numChange 事件处理函数中，触发步骤1声明的自定义事件：

```vue
getNumber(val) {
//   触发自定义事件
// 向外传递数据对象
this.$emit('countChange',{id:this.id,num:val})
},
```

3.在 App.vue 根组件中使用 EsGoods.vue 组件时，监听它的自定义事件
countChange ：  

```vue
<es-goods
  v-for="item in goodslist"
  :key="item.id"
  :id="item.id"
  :thumb="item.goods_img"
  :count="item.goods_count"
  :price="item.goods_price"
  :title="item.goods_name"
  :checked="item.goods_state"
  @stateChange="onGoodsStateChange"
  @countChange="onGoodsCountChange"
></es-goods>
```

并在 methods 中声明对应的事件处理函数：

```vue
onGoodsCountChange(value){
  this.goodslist.some(item=>{
    if(item.id==value.id){
      item.goods_count=value.num;
    }
  })
}
```

# table案例

## 0. 实现步骤

① 搭建项目的基本结构

② 请求商品列表的数据

③ 封装 MyTable 组件

④ 实现删除功能

⑤ 实现添加标签的功能  

## 1. 搭建项目基本结构

### 1.1 初始化项目

1.在终端运行如下的命令，初始化 vite 项目：

```bash
npm init vite-app table-demo
```

2.cd 到项目根目录，安装依赖项：

```bash
npm install
```

3.安装 less 依赖包：

```bash
npm i less -D
```

4.使用 vscode 打开项目，并在 vscode 集成的终端下运行如下的命令，把项目运行起来：

```bash
npm run dev
```

### 1.2 梳理项目结构

1.重置 App.vue 根组件的代码结构：

```vue
<template>
  <div>
    <h1>App 根组件</h1>
  </div>
</template>

<script>
export default {
  name:'MyApp'
}
</script>

<style lang='less' scoped>

</style>
```

2.删除 components 目录下的 HelloWorld.vue 组件

3.重置 index.css 中的样式：

```css
:root{
  font-size: 12px;
}
body{
  padding: 8px;
}
```

4.把资料目录下的 css 文件夹复制、粘贴到 assets 目录中，并在 main.js 入口文件中入  bootstrap.css ：  

```js
import { createApp } from 'vue'
import App from './App.vue'
import './assets/css/bootstrap.css'
import './index.css'

createApp(App).mount('#app')
```

## 2. 请求商品列表的数据

1.运行如下的命令，安装 Ajax 的请求库：

```bash
npm install axios
```

2.在 main.js 入口模块中，导入并全局配置 axios：

```js
import { createApp } from 'vue'
import App from './App.vue'

// 1.导入axios
import axios from 'axios'

const app=createApp(App);

// 2.将axios挂载到全局，之后，每个组件都可以通过this.$http代替axios发送请求
app.config.globalProperties.$http=axios;

// 3.设置请求的base url
axios.defaults.baseURL='https://www.escook.cn'

app.mount('#app');
```

3.在 App.vue 组件的 data 中声明 goodslist 商品列表数据：

```js
data(){
  return{
    // 商品列表数据
    goodslist:[],
  }
}
```

4.在 App.vue 组件的 methods 中声明 getGoodsList 方法，用来从服务器请求商品列表的数据：  

```js
methods: {
  // 初始化商品列表数据
  async getGoodsList(){
    const res= await this.$http('/api/goods');
    // 请求失败 
    if(res.status!==200) console.log('获取商品列表失败');
    // 请求成功
    this.goodslist=res.data.data;
  }
},
```

5.在 App.vue 组件中，声明 created 生命周期函数，并调用 getGoodsList 方法：

```js
created(){
  this.getGoodsList();
}
```

## 3. 封装 MyTable 组件

### 3.0 MyTable 组件的封装要求

1.用户通过名为 data 的 prop 属性，为 MyTable.vue 组件指定数据源

2.在 MyTable.vue 组件中，预留名称为 header 的具名插槽

3.在 MyTable.vue 组件中，预留名称为 body 的作用域插槽  

### 3.1 创建并使用 MyTable 组件

1.在 components/my-table 目录下新建 MyTable.vue 组件：

```vue
<template>
  <div>Table组件</div>
</template>

<script>
export default {
    name:'MyTable',
}
</script>

<style lang='less' scoped>

</style>
```

2.在 App.vue 组件中导入并注册 MyTable.vue 组件：

3.在 App.vue 组件的 DOM 结构中使用 MyTable.vue 组件：

```vue
<template>
  <div>
    <h1>App 根组件</h1>
    <my-table></my-table>
  </div>
</template>

<script>
import MyTable from './components/my-table/MyTable.vue'
export default {
  name: "MyApp",
  components:{
    MyTable,
  },
};
</script>

```

### 3.2 为表格声明 data 数据源

1.在 MyTable.vue 组件的 props 节点中声明表格的 data 数据源：

```vue
<script>
export default {
    name:'MyTable',
    props:{
        // 表格数据源
        data:{
            type:Array,
            required:true,
            default:[]
        }
    }
}
</script>
```

2.在 App.vue 组件中使用 MyTable.vue 组件时，通过属性绑定的形式为表格指定 data 数据源:

```vue
<template>
  <div>
    <h1>App 根组件</h1>
    <my-table :data='goodslist'></my-table>
  </div>
</template>
```

### 3.3 封装 MyTable 组件的模板结构

1.基于 bootstrap 提供的Tables( https://v4.bootcss.com/docs/content/tables/)，在MyTable.vue 组件中渲染最基本的模板结构：  

```vue
<template>
  <table class="table table-bordered table-striped">
    <!-- 表格的标题区域 -->
    <thead>
      <tr>
        <th>#</th>
        <th>商品名称</th>
        <th>价格</th>
        <th>标签</th>
        <th>操作</th>
      </tr>
    </thead>
    <!-- 表格的主体区域 -->
    <tbody></tbody>
  </table>
</template>
```

2.为了提高组件的复用性，最好把表格的 标题区域 预留为 <slot> 具名插槽，方便使用者自定义表格的标题：  

```vue
<template>
  <table class="table table-bordered table-striped">
    <!-- 表格的标题区域 -->
    <thead>
      <tr>
        <!-- 命名插槽 -->
        <slot name="header"></slot>
      </tr>
    </thead>
    <!-- 表格的主体区域 -->
    <tbody></tbody>
  </table>
</template>
```

3.在 App.vue 组件中，通过具名插槽的形式，为 MyTable.vue 组件指定标题名称：

```vue
<template>
  <div>
    <h1>App 根组件</h1>

    <my-table :data='goodslist'>
      <!-- 具名插槽 -->
      <template v-slot:header>
          <th>#</th>
          <th>商品名称</th>
          <th>价格</th>
          <th>标签</th>
          <th>操作</th>
      </template>
    </my-table>
  </div>
</template>
```

### 3.4 预留名称为 body 的作用域插槽

1.在 MyTable.vue 组件中，通过 v-for 指令循环渲染表格的数据行：

```vue
<template>
  <table class="table table-bordered table-striped">
    <!-- 表格的标题区域 -->
    <thead>
      <tr>
        <!-- 命名插槽 -->
        <slot name="header"></slot>
      </tr>
    </thead>
    <!-- 表格的主体区域 -->
    <tbody>
        <tr v-for='(item,index) in data' :key='item.id'></tr>
    </tbody>
  </table>
</template>
```

2.为了提高 MyTable.vue 组件的复用性，最好把表格数据行里面的 td 单元格预留为`<slot>` 具名插槽。示例代码如下：  

```vue
<tr v-for='(item,index) in data' :key='item.id'>
    <!-- 为数据行预留的插槽 -->
    <slot name="body"></slot>
</tr>
```

3.为了让组件的使用者在提供 body 插槽的内容时，能够自定义内容的渲染方式，需要把body 具名插槽升级为 **作用域插槽** ：  

```vue
<tr v-for="(item, index) in data" :key="item.id">
<!-- 为数据行预留的作用域插槽 -->
    <slot name="body" :row="item" :index="index"></slot>
</tr>
```

4.在 App.vue 组件中，基于作用域插槽的方式渲染表格的数据：

```vue
<template>
  <div>
    <h1>App 根组件</h1>
    <my-table :data="goodslist">
      <!-- 具名插槽 -->
      <template v-slot:header>
        <th>#</th>
        <th>商品名称</th>
        <th>价格</th>
        <th>标签</th>
        <th>操作</th>
      </template>
      <template #body="{ row, index }">
        <td>{{ index + 1 }}</td>
        <td>{{ row.goods_name }}</td>
        <td>￥{{ row.goods_price }}</td>
        <td>{{ row.tags }}</td>
        <td>
          <button class="btn btn-danger btn-sm">删除</button>
        </td>
      </template>
    </my-table>
  </div>
</template>
```

## 4. 实现删除功能

1.为删除按钮绑定 click 事件处理函数：

```vue
<button class="btn btn-danger btn-sm" @click='onRemove(row.id)'>删除</button>
```

2.在 App.vue 组件的 methods 中声明事件处理函数如下：

```js
onRemove(id){
  this.goodslist.some((item,index)=>{
    if(item.id===id){
      this.goodslist.splice(index,1);
      return true;
    }
  })
}
```

### 5. 实现添加标签的功能

### 5.1 自定义渲染标签列

根据 bootstrap 提供的 Badge ( https://v4.bootcss.com/docs/components/badge/#contextual-variations )效果，循环渲染商品的标签信息如下：  

```vue
<!-- 循环渲染标签 -->
<td>
  <span class="badge badge-warning ml-2" v-for='item in row.tags' :key='item'>{{item}} </span>
</td>
```

### 5.2 实现 input 和 button 的按需展示

1.使用 v-if 结合 v-else 指令，控制 input 和 button 的按需展示：

```vue
<td>
  <input type="text" class="form-control form-control-sm form-ipt" v-if="row.inputVisible">
  <button type="button" class="btn btn-primary btn-sm" v-else>+Tag</button>
  <span
    class="badge badge-warning ml-2"
    v-for="item in row.tags"
    :key="item">
    {{ item }}
  </span>
</td>
```

2.点击按钮，控制 input 和 button 的切换：

```vue
<input type="text" class="form-control form-control-sm form-ipt" v-if="row.inputVisible">
<button type="button" class="btn btn-primary btn-sm" v-else @click='row.inputVisible=true'>+Tag</button>
```

### 5.3 让 input 自动获取焦点

1.在 App.vue 组件中，通过 directives 节点自定义 v-focus 指令如下：

```vue
directives:{
  focus(el){
    el.focus();
  }
}
```

2.为 input 输入框应用 v-focus 指令：

```vue
<input type="text" class="form-control form-control-sm form-ipt" v-if="row.inputVisible" v-focus>
```

### 5.4 文本框失去焦点自动隐藏

1.使用 v-model 指令把 input 输入框的值双向绑定到 row.inputValue 中：

```vue
<input type="text" class="form-control form-control-sm form-ipt" v-if="row.inputVisible" v-focus v-model='row.inputValue'>
```

2.监听文本框的 blur 事件，在触发其事件处理函数时，把 当前行的数据 传递进去：

```vue
<input
  type="text"
  class="form-control form-control-sm form-ipt"
  v-if="row.inputVisible"
  v-focus
  v-model.trim="row.inputValue"
  @blur="onInputConfirm(row)"
/>
```

3.在 App.vue 组件的 methods 节点下声明 onInputConfirm 事件处理函数：

```js
onInputConfirm(row) {
  const val = row.inputValue;
  row.inputValue = "";
  row.inputVisible = false;
},
```

### 5.5 为商品添加新的 tag 标签

进一步修改 onInputConfirm 事件处理函数如下：

```js
onInputConfirm(row) {
  const val = row.inputValue;
  row.inputValue = "";
  row.inputVisible = false;
  // 1.1 判断val的值是否为空，如果为空，则并添加
  // 1.2 判断val的值是否存在于数组中，如果存在则不添加 
  if(!val||row.tags.indexOf(val)!==-1) return;
  // 2将标签添加到tag数组中
  row.tags.push(val);

},
```

### 5.6 响应文本框的回车按键

当用户在文本框中敲击了 回车键 的时候，也希望能够把当前输入的内容添加为 tag 标签。此时，可以为文本框绑定 keyup 事件如下：  

```vue
<input
  type="text"
  class="form-control form-control-sm form-ipt"
  v-if="row.inputVisible"
  v-focus
  v-model.trim="row.inputValue"
  @blur="onInputConfirm(row)"
  @keyup.enter="onInputConfirm(row)"
/>
```

### 5.7 响应文本框的 esc 按键

当用户在文本框中敲击了 esc 按键的时候，希望能够快速清空文本框的内容。此时，可以为文本框绑定 keyup 事件如下  

```vue
<input
  type="text"
  class="form-control form-control-sm form-ipt"
  v-if="row.inputVisible"
  v-focus
  v-model.trim="row.inputValue"
  @blur="onInputConfirm(row)"
  @keyup.enter="onInputConfirm(row)"
  @keyup.esc="row.inputValue=''"
/>
```

# 用户管理小案例

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

