---
title: Vue组件基础
date: 2021-08-10 10:06:12
tags: [Vue, 组件]
---

# 单页面应用程序

单页面应用程序（英文名：Single Page Application）简称 SPA，顾
名思义，指的是一个 Web 网站中只有唯一的一个 HTML 页面，所有的
功能与交互都在这唯一的一个页面内完成。  

## 1. 单页面应用程序的特点

单页面应用程序将所有的功能局限于一个 web 页面中，**仅在该 web 页面初始化时加载相应的资源**（ HTML、JavaScript 和 CSS）。

一旦页面加载完成了，SPA **不会**因为用户的操作而**进行页面的重新加载或跳转**。而是利用 JavaScript 动态地变换HTML 的内容，从而实现页面与用户的交互。  

## 2. 单页面应用程序的优点

SPA 单页面应用程序最显著的 3 个优点如下：

① 良好的交互体验

- 单页应用的内容的改变不需要重新加载整个页面

- 获取数据也是通过 Ajax 异步获取

- 没有页面之间的跳转，不会出现“白屏现象”

② 良好的前后端工作分离模式

- 后端专注于提供 API 接口，更易实现 API 接口的复用

- 前端专注于页面的渲染，更利于前端工程化的发展

③ 减轻服务器的压力

- 服务器只提供数据，不负责页面的合成与逻辑的处理，吞吐能力会提高几倍  

## 3.单页面应用程序的缺点

任何一种技术都有自己的局限性，对于 SPA 单页面应用程序来说，主要的缺点有如下两个：

① 首屏加载慢，但可以通过以下方式解决：

- 路由懒加载
- 代码压缩
- CDN 加速
- 网络传输压缩

② 不利于 SEO，但可以通过以下方式解决：

- SSR 服务器端渲染  

## 4. 如何快速创建 vue 的 SPA 项目

vue 官方提供了两种快速创建工程化的 SPA 项目的方式：

① 基于 vite 创建 SPA 项目

② 基于 vue-cli 创建 SPA 项目  

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108051617916.png" alt="image-20210805161719813" style="zoom:50%;" />

# vite的基本使用

vite官网：https://cn.vitejs.dev/

## 1.创建vite的项目

按照顺序执行如下的命令，即可基于 vite 创建 vue 3.x 的工程化项目：  

```bash
npm init vite-app '项目名称'
cd code1
npm install 
npm run dev
```

## 2. 梳理项目的结构

使用vite创建的项目目录：

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108051631447.png" alt="image-20210805163142399" style="zoom:50%;" />

其中：

- node_modules 目录用来存放第三方依赖包

- public 是公共的静态资源目录

- src 是项目的源代码目录（程序员写的所有代码都要放在此目录下）

- .gitignore 是 Git 的忽略文件

- index.html 是 SPA 单页面应用程序中唯一的 HTML 页面

- package.json 是项目的包管理配置文件  

在 src 这个项目源代码目录之下，包含了如下的文件和文件夹：  

![image-20210805163330821](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108051633861.png)

其中：

- assets 目录用来存放项目中所有的**静态资源文件**（css、fonts等）

- components 目录用来存放项目中所有的自定义组件

- App.vue 是项目的**根组件**

- index.css 是项目的**全局样式表文件**

- main.js 是整个项目的**打包入口文件**

## 3. vite 项目的运行流程

在工程化的项目中，vue 要做的事情很单纯：**通过 main.js 把 App.vue 渲染到 index.html 的指定区域中。**

其中：

① App.vue 用来编写待渲染的**模板结构**

② index.html 中需要预留一个 el 区域

③ main.js 把 App.vue 渲染到了 index.html 所预留的区域中  

### 3.1 在 App.vue 中编写模板结构

清空 App.vue 的默认内容，并书写如下的模板结构：  

```vue
<template>
  <h1>这是app.vue根组件</h1>
</template>
```

3.2 在 index.html 中预留 el 区域

打开 index.html 页面，确认预留了 el 区域：  

```html
<body>
  <div id="app"></div>
  <script type="module" src="/src/main.js"></script>
</body>
```

### 3.3 在 main.js 中进行渲染

按照 vue 3.x 的标准用法，把 App.vue 中的模板内容渲染到 index.html 页面的 el 区域中：  

```js
//main.js
// 1. 从vue中按需导入createApp函数
//  createApp函数的作用 ：创建vue的spa
import { createApp } from 'vue'

// 2. 导入待渲染的App组件
import App from './App.vue'
import './index.css'

// 3. 调用createAPP()函数，返回值是SPA实例
//  同时将导入的APP 组件作为参数传给 createApp函数（将APP渲染到index.html上）
const spa_app=createApp(App);

// 4. 调用spa_app实例的mount方法，用来指定vue实际要控制的区域
spa_app.mount('#app');

```



```js
//main.js
// 1. 从vue中按需导入createApp函数
//  createApp函数的作用 ：创建vue的spa
import { createApp } from 'vue'

// 2. 导入待渲染的App组件
import App from './App.vue'
import './index.css'

// 3. 调用createAPP()函数，返回值是SPA实例
//  同时将导入的APP 组件作为参数传给 createApp函数（将APP渲染到index.html上）
const spa_app=createApp(App);

// 4. 调用spa_app实例的mount方法，用来指定vue实际要控制的区域
spa_app.mount('#app');

```

# 组件化开发思想

## 1.什么是组件化开发

组件化开发指的是：根据封装的思想，把页面上可重用的部分封装为组件，从而方便项目的开发和维护。

例如：http://www.ibootstrap.cn/ 所展示的效果，就契合了组件化开发的思想。用户可以通过拖拽组件的方式，快速生成一个页面的布局结构。  

![image-20210805170030731](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108051700807.png)

## 2. 组件化开发的好处

前端组件化开发的好处主要体现在以下两方面：

- 提高了前端代码的复用性和灵活性
- 提升了开发效率和后期的可维护性  

## 3. vue 中的组件化开发

vue 是一个完全支持组件化开发的框架。**vue 中规定组件的后缀名是 .vue。**之前接触到的 App.vue 文件本质上就是一个 vue 的组件。  

# vue组件的构成

## 1.vue组件的构成

每个 .vue 组件都由 3 部分构成，分别是：

- template -> 组件的模板结构

- script -> 组件的 JavaScript 行为

- style -> 组件的样式

其中，**每个组件中必须包含 template 模板结构**，而 script 行为和 style 样式是可选的组成部分。  

## 2.组件的template节点

vue规定：每个组件对应的模板结构，需要定义到 `<template> `节点中。

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108051714617.png" alt="image-20210805171410574" style="zoom:67%;" />

注意：`<template>` 是 vue 提供的容器标签，只起到包裹性质的作用，它不会被渲染为真正的 DOM 元素。  

1. 在template中使用指令

   在组件的 `<template> `节点中，支持使用前面所学的指令语法，来辅助开发者渲染当前组件的 DOM 结构。

   代码示例如下：  

   ![image-20210805171703762](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108051717821.png)

2. 在template中定义根节点

   在 vue 2.x 的版本中，<template> 节点内的 DOM 结构仅支持单个根节点：

   ![image-20210805171811774](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108051718841.png)

   但是，在 vue 3.x 的版本中，<template> 中支持定义多个根节点： 

   ![image-20210805171853981](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108051718042.png) 

## 3.组件中的script节点

vue 规定：组件内的 `<script> `节点是**可选**的，开发者可以在 `<script> `节点中封装组件的 JavaScript 业务逻辑。

```vue
<template>
  <h1>这是app.vue根组件</h1>
</template>

<script>
// 今后，组件相关的data数据、 methods方法等，都需要定义到 export default所导出的对象中。
export default {
  
}
</script>
```

3.1 script 中的 name 节点

可以通过 name 节点为当前组件定义一个名称：

```js
<script>
export default {
  // name属性指向的是当前组件的名称（建议：每个单词的首字母大写）
  name:'MyAPP'
}
</script>
```

在使用 vue-devtools 进行项目调试的时候，自定义的组件名称可以清晰的区分每个组件：  

![image-20210805172753292](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108051727353.png)

3.2 script 中的 data 节点  

**vue 组件渲染期间需要用到的数据**，可以定义在 data 节点中：  

```vue
<template>
  <h1>这是app.vue根组件 {{username}}</h1>
</template>

<script>
// 今后，组件相关的data数据、 methods方法等，都需要定义到 export default所导出的对象中。
export default {
  // name属性指向的是当前组件的名称（建议：每个单词的首字母大写）
  name: "MyAPP",
  data(){
    return {
      // 组件的数据（data方法中 return出去的对象，就是当前组件渲染期间需要用到的数据对象）
      username:'jiaqi'
    }
  }
};
</script>
```

组件中的 data 必须是**函数**!

vue 规定：组件中的 data 必须是一个函数，不能直接指向一个数据对象。因此在组件中定义 data 数据节点。

3.3 script 中的 methods 节点

组件中的事件处理函数，必须定义到 methods 节点中。

```vue
<template>
  <div>
    <h1>这是app.vue根组件 {{count}}</h1>
    <button @click='addCount'>+1</button>
  </div>
  
</template>

<script>
// 组件相关的data数据、 methods方法等，都需要定义到 export default所导出的对象中。
export default {
  // name属性指向的是当前组件的名称（建议：每个单词的首字母大写）
  name: "MyAPP",
  data(){
    return {
      count:0,
    }
  },
  methods: {
    addCount(){
      this.count++;
    }
  },
};
</script>
```

## 4.组件中的style节点

vue 规定：组件内的 `<style> `节点是可选的，开发者可以在 <style> 节点中编写样式美化当前组件的 UI 结构。  

```vue
<template>
  <div>
    <h1>这是app.vue根组件 {{count}}</h1>
    <button @click='addCount'>+1</button>
  </div>
  
</template>

<script>
// 组件相关的data数据、 methods方法等，都需要定义到 export default所导出的对象中。
export default {
  // name属性指向的是当前组件的名称（建议：每个单词的首字母大写）
  name: "MyAPP",
  data(){
    return {
      count:0,
    }
  },
  methods: {
    addCount(){
      this.count++;
    }
  },
};
</script>

<style lang="css">
    h1{
      color: red;
    }
</style>
```

其中 `<style> `标签上的 lang="css" 属性是可选的，它表示所使用的样式语言。默认只支持普通的 css 语法，可选值还有 less、scss 等。  

### 4.1 让 style 中支持 less 语法

如果希望使用 less 语法编写组件的 style 样式，可以按照如下两个步骤进行配置：

① 运行 `npm install less -D` 命令安装依赖包，从而提供 less 语法的编译支持

② 在 `<style>` 标签上添加 lang="less" 属性，即可使用 less 语法编写组件的样式  

```vue
<style lang="less">
    h1 {
      color: red;
      i {
        color:blue;
      }
    }
</style>
```

# 组件的基本使用

## 1.组件的注册

组件之间可以进行相互的引用，例如：  

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108051811991.png" alt="image-20210805181154925" style="zoom:50%;" />

vue 中组件的**引用**原则：**先注册后使用**。  

### 1.1 组件注册的两种方式

vue 中注册组件的方式分为“全局注册”和“局部注册”两种，其中：

- 被全局注册的组件，可以在全局任何一个组件内使用

- 被局部注册的组件，只能在当前注册的范围内使用  

![image-20210805181439691](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108051814792.png)

### 1.2 全局注册组件  

首先准备好两个组件文件如 `Swiper.vue`和`Test.vue`，并放在components文件夹下。

接着在main.js中导入这两个组件，并使用app.component()全局注册组件，app.component()的第一个参数是为组件自定义的名字，第二个参数是需要被注册的组件。

```js
//main.js

// 1.按需导入createApp函数
import {createApp} from 'vue'

// 2.导入待渲染的app.vue 组件
import App from './App.vue'

    // 全局注册组件 1.导入需要被全局注册的组件
import Swiper from './components/01.globalReg/Swiper.vue'

// 3.创建实例：调用createApp函数，并将App作为参数
const app=createApp(App);

    //  全局注册组件 2.调用 app.component()方法来全局注册组件
app.component('my-swiper',Swiper);


// 4.调用mount方法把APP组件的模板结构，渲染到指定的el区域中。
app.mount('#app');

```

接着可以在其他组件中，以标签的形式，使用刚才注册的全局组件即可

```vue
//App.vue

<template>
  <div>
    <h1>这是<i>app.vue</i>根组件 {{ count }}</h1>
    <button @click="addCount">+1</button>
    <my-swiper></my-swiper>
  </div>
</template>
```

### 1.3 局部注册组件

```vue
<template>
  <div>
    <h1>这是<i>app.vue</i>根组件 {{ count }}</h1>
    <button @click="addCount">+1</button>
    <my-search></my-search>
  </div>
</template>

<script>
import Search from './components/02.privateReg/search.vue'
export default {
  name: "MyAPP",
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    addCount() {
      this.count++;
    },
  },
  components:{
    'my-search':Search
  }
};
</script>
```

### 1.4  全局注册和局部注册的区别

- 被全局注册的组件，可以在全局任何一个组件内使用

- 被局部注册的组件，只能在当前注册的范围内使用  

### 1.5组件注册时名称的大小写

在进行组件的注册时，定义组件注册名称的方式有两种：

① 使用 kebab-case 命名法（俗称短横线命名法，例如 my-swiper 和 my-search）

② 使用 PascalCase 命名法（俗称帕斯卡命名法或大驼峰命名法，例如 MySwiper 和 MySearch）  

短横线命名法的特点：

- 必须严格按照短横线名称进行使用

帕斯卡命名法的特点：

- **既可以严格按照帕斯卡名称进行使用，又可以转化为短横线名称进行使用**。
- 注意：在实际开发中，推荐使用帕斯卡命名法为组件注册名称，因为它的适用性更强。  

### 1.6 通过name属性注册组件

除了可以直接提供组件的注册名称:

```vue
//components/swiper.vue

<template>
    <h3>Swiper轮播图组件</h3>
</template>

<script>
export default {
    // 指定组件的名字
    name:'MySwiper'
}
</script>
```

```js
//main.js
import Swiper from './components/01.globalReg/Swiper.vue'
app.component('my-swiper',Swiper);
```

还可以把组件的 name 属性作为注册后组件的名称，示例代码如下：  

```js
//main.js
import Swiper from './components/01.globalReg/Swiper.vue'
app.component(Swiper.name,Swiper);
```

## 2. 组件之间的样式冲突问题  

默认情况下，写在 .vue 组件中的样式会全局生效，因此很容易造成多个组件之间的样式冲突问题。导致组件之间样式冲突的根本原因是：

① 单页面应用程序中，所有组件的 DOM 结构，都是基于唯一的 index.html 页面进行呈现的。

② 每个组件中的样式，都会影响整个 index.html 页面中的 DOM 元素  。

```vue
<template>
    <div>
        <h1>App.vue组件</h1>
        <p>App.vue中的p</p>
        <p>App.vue中的p</p>
        <my-list></my-list>
    </div>
</template>

<script>
import MyList from './List.vue';

export default {
    name:'MyApp',
    components:{
        MyList
    }
}
</script>

<style lang="css">
    p{
        color:red;
    }
</style>
```

在父组件中p标签文字变红了，子组件同样的p标签文字同样变红了。

![image-20210805195116759](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108051951848.png)

### 2.1 思考：如何解决组件样式冲突的问题

为每个组件分配**唯一的自定义属性**，在编写组件样式时，通过属性选择器来控制样式的作用域，示例代码如下：  

```vue
<template>
    <div >
        <h1 >App.vue组件</h1>
        <p data-v-001>App.vue中的p</p>
        <p data-v-001>App.vue中的p</p>
        <my-list></my-list>
    </div>
</template>

<script>
import MyList from './List.vue';

export default {
    name:'MyApp',
    components:{
        MyList
    }
}
</script>

<style lang="css">
/* 通过中括号“属性选择器”，
来防止组件之间的样式冲突问题，因为每个组件分配的自定义属性是“唯一的” */
    p[data-v-001]{
        color:red;
    }
</style>
```

### 2.2 style 节点的 scoped 属性

为了提高开发效率和开发体验，vue 为 **style 节点提供了 scoped 属性**，从而防止组件之间的样式冲突问题：  

style节点的 scoped属性，用来自动为每个组件分配唯一的“自定义属性”，并自动为当前组件的DOM标签和style样式应用这个自定义属性，防止组件的样式冲突问题。

```vue
<template>
  <div>
    <h1>App.vue组件</h1>
    <p>App.vue中的p</p>
    <p>App.vue中的p</p>
    <my-list></my-list>
  </div>
</template>

<script>
import MyList from "./List.vue";

export default {
  name: "MyApp",
  components: {
    MyList,
  },
};
</script>

<style lang="css" scoped>
/* 
style节点的 scoped属性，用来自动为每个组件分配唯一的“自定义属性”，
并自动为当前组件的DOM标签和style样式应用这个自定义属性，防止组件的样式冲突问题。
 */
p {
  color: red;
}
</style>
```

```vue
<template>
  <div>
    <h3 class="title">list.vue组件</h3>
    <p>list.vue中的p</p>
    <p>list.vue中的p</p>
  </div>
</template>

<script>
export default {
  name: "MyList",
};
</script>
```



### 2.3 :deep()样式穿透

如果给当前组件的 style 节点添加了**scoped** 属性，则**当前组件的样式对其子组件是不生效的**。如果想让某些样式对子组件生效，可以使用**`:deep()` 深度选择器**。  

```vue
<style lang="css" scoped>
/* 
style节点的 scoped属性，用来自动为每个组件分配唯一的自定义属性，
并自动为当前组件的DOM标签和style样式应用这个自定义属性，防止组件的样式冲突问题。
 */

p {
  color: red;
}

.title {
  color: bisque;
  /* 不加:deep()时生成的选择器格式为 .data-v-7bc0fb25[title] */
}

:deep().title {
  color: bisque;
  /* 加:deep()时生成的选择器格式为 [data-v-7bc0fb25] .title */
}
</style>
```

注意：`/deep/` 是 vue2.x 中实现样式穿透的方案。在 vue3.x 中推荐使用 `:deep()` 替代 `/deep/`。  

## 3. 组件的 props

为了提高组件的复用性，在封装 vue 组件时需要遵守如下的原则：

- 组件的 DOM 结构、Style 样式要尽量复用

- 组件中要展示的数据，尽量由组件的使用者提供

为了方便使用者为组件提供要展示的数据，vue 组件提供了 props 的概念。  

### 3.1 什么是组件的 props

props 是**组件的自定义属性**，**组件的使用者可以通过 props 把数据传递到子组件内部**，供子组件内部进行使用。代码示例如下：  

props 的作用：父组件通过 props 向子组件传递要展示的数据。

props 的好处：提高了组件的复用性。  

### 3.2 在组件中声明 props

在封装 vue 组件时，**可以把动态的数据项声明为 props 自定义属性**。自定义属性可以在当前组件的模板结构中被直接使用。示例代码如下：  

```vue
//article.vue
<template>
    <div>
        <h3>标题：{{title}}</h3>
        <h5>作者: {{author}}</h5>
    </div>
</template>

<script>
export default {
    name:'MyArticle',
    // props接收外界传递的数据
    props:['title','author'],
}
</script>
```

```vue
<template>
    <div>
        <h1>这是App.vue 根组件</h1>
        <hr>
        <my-article title="面朝大海"  author="海子"></my-article>
    </div>
</template>

<script>
import MyArticle from './Article.vue'

export default {
    name:'MyApp',
    components:{
        MyArticle
    },
}
</script>
```

![image-20210806115102272](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108061151404.png)

### 3.3 无法使用未声明的 props

如果父组件给子组件传递了子组件并没有声明的 props 属性，则这些属性会被忽略，无法被子组件使用，示例代码如下：  

```vue
<template>
    <div>
        <h3>标题：{{title}}</h3>
        <h5>作者: {{author}}</h5>
    </div>
</template>

<script>
export default {
    name:'MyArticle',
    // props接收外界传递的数据
    props:['title'], 
    // 并没有声明author属性，因此子组件无法访问到author的值
}
</script>
```

App.vue的代码同上个例子，故省略。最终渲染结果：

![image-20210806115419523](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108061154576.png)

### 3.4 动态绑定 props 的值  

可以使用 v-bind 属性绑定的形式，为组件动态绑定 props 的值，示例代码如下：  

```vue
// App.vue
<template>
    <div>
        <h1>这是App.vue 根组件</h1>
        <hr>
        <my-article :title="info.title"  :author="info.author"></my-article>
    </div>
</template>

<script>
import MyArticle from './Article.vue'

export default {
    name:'MyApp',
    components:{
        MyArticle
    },
    data() {
        return {
            info:{title:'abc',author:'jiaqi'}
        }
    },
}
</script>
```

### 3.5 props 的大小写命名

组件中如果使用“camelCase (驼峰命名法)”声明了 props 属性的名称，则有两种方式为其绑定属性的值：  

```vue
//article.vue
<template>
    <div>
        <h5>发布时间: {{pubTime}}</h5>
    </div>
</template>

<script>
export default {
    name:'MyArticle',
    // props接收外界传递的数据
    props:['title','author','pubTime'],
}
</script>

//====================================
// App.vue
<template>
    <div>
        <h1>这是App.vue 根组件</h1>
        <my-article  pubTime='1989'></my-article>
        <my-article  pub-time='1989'></my-article>
    </div>
</template>
```

## 4. Props验证

### 1.什么是 props 验证

在封装组件时对外界传递过来的 props 数据进行合法性的校验，从而防止数据不合法的问题。

![image-20210806142210426](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108061422528.png)

使用数组类型的 props 节点的缺点：无法为每个 prop 指定具体的数据类型。  

### 2. 对象类型的 props 节点

使用对象类型的 props 节点，可以对每个 prop 进行数据类型的校验，示意图如下： 

![image-20210806142342004](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108061423096.png) 

```vue
<template>
    <div>
        <p>数量：{{count}}</p>
        <p>状态：{{state}}</p>
    </div>
</template>

<script>
export default {
    name:'Count',
    props:{
        count:Number,
        state:Boolean
    }
}
</script>
```

### 3. props 验证

对象类型的 props 节点提供了多种数据验证方案，例如：

① 基础的类型检查

可以直接为组件的 prop 属性指定基础的校验类型，从而防止组件的使用者为其绑定错误类型的数据：  

```vue
<script>
export default {
    props:{
        // 支持8种基础类型
        propA:String,
        propB:Number,
        propC:Boolean,
        propD:Array,
        propE:Object,
        propF:Date,
        propG:Function,
        propH:Symbol
    }
}
</script>
```

② 多个可能的类型

如果某个 prop 属性值的类型不唯一，此时可以通过数组的形式，为其指定多个可能的类型，示例代码如下：  

```vue
<script>
export default {
  props: {
    // propA的的值可以为字符串或者数字
    propA: [String, Number],
  },
};
</script>
```

③ 必填项校验

如果组件的某个 prop 属性是必填项，必须让组件的使用者为其传递属性的值。此时，可以通过如下的方式将其设置为必填项：  

```vue
<script>
export default {
  props: {
    propA: [String, Number],
    propB:{
        type:String,
        required:true
    }
  },
};
</script>
```

④ 属性默认值

在封装组件时，可以为某个 prop 属性指定默认值。示例代码如下：  

```vue
<script>
export default {
  props: {
    propB:{
        type:Number,
        default:100
    }
  },
};
</script>
```

⑤ 自定义验证函数    

在封装组件时，可以为 prop 属性指定自定义的验证`validator`函数，从而对 prop 属性的值进行更加精确的控制：  

```vue
<script>
export default {
  props: {
    propB:{
        // 属性的值通过形参value进行接收
        validator(value){
            //validator 函数返回值为true表示验证成功，false为验证失败
            return ['success','warning','danger'].indexOf(value)!==-1;
            //propB的值必须为其中的一个

        }
    }
  },
};
</script>
```



## 5.class与style样式绑定

在实际开发中经常会遇到动态操作元素样式的需求。因此，vue 允许开发者通过 v-bind 属性绑定指令，为元素动态绑定 class 属性的值和行内的 style 样式。  

### 5.1 动态绑定 HTML 的 class

可以通过三元表达式，动态的为元素绑定 class 的类名。示例代码如下：  

```vue
<template>
  <div>
      <h3 class='thin' :class=" isItalic?'italic':''">MyStyle组件</h3>
      <button @click='isItalic=!isItalic'>Toggle Italic</button>
  </div>
</template>

<script>
export default {
    name:'MyStyle',
    data(){
        return{
            // 字体是否倾斜
            isItalic:true
        }
    }
}
</script>

<style lang="css">
    .thin{
        font-weight: 200;
    }
    .italic{
        font-style: italic;
    }
</style>
```

### 5.2 以数组语法绑定 HTML 的 class

如果元素需要动态**绑定多个** class 的类名，此时可以**使用数组**的语法格式：  

```vue
<template>
  <div>
    <h3 class="thin" :class="[isItalic ? 'italic' : '',isDelete?'delete':'']">MyStyle组件</h3>
    <button @click="isItalic = !isItalic">Toggle Italic</button>
    <button @click="isDelete = !isDelete">Toggle Delete</button>
  </div>
</template>

<script>
export default {
  name: "MyStyle",
  data() {
    return {
      // 字体是否倾斜
      isItalic: true,
      isDelete: true,
    };
  },
};
</script>

<style lang="css">
.thin {
  font-weight: 200;
}
.italic {
  font-style: italic;
}
.delete {
  text-decoration: line-through;
}
</style>
```

![image-20210806125040359](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108061250434.png)

### 5.3 以对象语法绑定 HTML 的 class

使用数组语法动态绑定 class 会导致模板结构臃肿的问题。此时可以使用**对象语法**进行简化：  

```vue
<template>
  <div>
    <h3 class="thin" :class="classObj">MyStyle组件</h3>
    
    <button @click="classObj.italic = !classObj.italic">Toggle Italic</button>
    <button @click="classObj.delete = !classObj.delete">Toggle Delete</button>
  </div>
</template>

<script>
export default {
  name: "MyStyle",
  data() {
    return {
      classObj:{
          delete:false,
          italic:false
      }
    };
  },
};
</script>
```

### 5.4 以对象语法绑定内联的 style  

`:style` 的对象语法十分直观——看着非常像 CSS，但其实是一个 **JavaScript 对象**。CSS property 名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名：  

```vue
<template>
  <div>
    <div :style="{color:active,'font-size':fsize+'px',backgroundColor:bgcolor}">今天好热</div>
    <button @click='fsize++'>字号+1</button>
    <button  @click='fsize--'>字号-1</button>
  </div>
</template>

<script>
export default {
  name: "MyStyle",
  data() {
    return {
    //   高亮时的文本颜色
    active:'red',
    // 文字大小
    fsize:30,
    // 背景颜色
    bgcolor:'pink'
    };
  },
};
</script>

```

## 封装组件的案例

封装要求：
① 允许用户自定义 title 标题

② 允许用户自定义 bgcolor 背景色

③ 允许用户自定义 color 文本颜色

④ MyHeader 组件需要在页面顶部进行 fixed 固定定位，且 z-index 等于 999  

```vue
//App.vue
<template>
    <div class="app-container">
        <h1>App根组件</h1>
        <hr>
        <my-header title="电商平台A" bgcolor="red" color='#fff'></my-header>
    </div>
</template>

<script>
import MyHeader from './MyHeader.vue';
export default {
    name:'MyApp',
    components:{
        MyHeader
    }
}
</script>

<style lang="less" scoped>
    .app-container{
        margin-top: 45px;
    }
</style>
```

```vue
//MyHeader.vue

<template>
    <div class="header-container" :style="{backgroundColor:bgcolor,color:color}">
        {{title ||'Header-container'}}
    </div>
</template>
<script>
export default {
    name:'MyHeader',
    props:['title','bgcolor','color']
}
</script>

<style lang="less" scoped>
    .header-container{
        height: 45px;
        background-color: pink;
        text-align: center;
        line-height: 45px;
        position: fixed;
        top: 0;
        left:0;
        width: 100%;
        z-index: 9999;
    }
</style>
```

## 6.自定义事件

### 1.什么是自定义事件

在封装组件时，为了让**组件的使用者**可以**监听到组件内状态的变化**，此时需要用到组件的自定义事件  

![image-20210806155958160](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108061559242.png)

### 2. 自定义事件的 3 个使用步骤

#### 在封装组件时：

① **声明**自定义事件 (但这步可以省略)

开发者为自定义组件封装的自定义事件，必须事先在 `emits` 节点中声明，示例代码如下：  

```vue
//counter.vue
<template>
  <div>
    <h3>Counter 组件</h3>
    <button @click="onBtnClick">+1</button>
  </div>
</template>

<script>
export default {
    emits:['change'],
}
</script>
```

② **触发**自定义事件

在 emits 节点下声明的自定义事件，可以通过 `this.$emit('自定义事件的名称')` 方法进行触发，示例代码如下：  

```vue
//counter.vue
<template>
  <div>
    <h3>Counter 组件</h3>
    <button @click="onBtnClick">+1</button>
  </div>
</template>

<script>
export default {
    emits:['change'],
    methods: {
        onBtnClick(){
            this.$emit('change')
            // 当点击+1按钮时，调用this.$emit()方法，触发自定义的change事件
        }
    },
}
</script>
```

#### 在使用组件时：

③ **监听**自定义事件  

在使用自定义的组件时，可以通过 v-on 的形式监听自定义事件。  

```vue
//App.vue
<template>
    <div>
        <!-- 使用v-on监听自定义事件 -->
        <my-counter @change='getCount'></my-counter>
    </div>
</template>

<script>
import MyCounter from './Counter.vue'
export default {
    components:{
        MyCounter
    },
    methods: {
        getCount(){
            console.log('监听到count值的变化');
        }
    },
}
</script>
```

### 3. 自定义事件传参

在调用 `this.$emit()`方法触发自定义事件时，可以通过第 2 个参数为自定义事件传参，示例代码如下：  

```vue
//counter.vue
<template>
  <div>
    <h3>Counter 组件</h3>
    <button @click="onBtnClick">+1</button>
  </div>
</template>

<script>
export default {
    emits:['change'],
    methods: {
        onBtnClick(){
            this.$emit('change',this.count)
            // 触发自定义事件时，通过第二个参数传递数据
            // 第一个参数为自定义事件的名称
        }
    },
    data(){
        return {count:0}
    }
}
</script>
```

```vue
//App.vue
<template>
    <div>
        <!-- 使用v-on监听自定义事件 -->
        <my-counter @change='getCount'></my-counter>
    </div>
</template>

<script>
import MyCounter from './Counter.vue'
export default {
    name:'MyApp',
    components:{
        MyCounter
    },
    methods: {
        getCount(val){
            console.log('监听到count值的变化 '+val);
        }
    },
}
</script>
```

## 7.组件上的v-model

### 1. 为什么需要在组件上使用 v-model

v-model 是双向数据绑定指令，当需要**维护组件内外数据的同步**时，可以在组件上使用 v-model 指令。示意图如下：  

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108061645006.png" alt="image-20210806164507918" style="zoom:80%;" />

- 外界数据的变化会自动同步到 counter 组件中
- counter 组件中数据的变化，也会自动同步到外界

### 2. 在组件上使用 v-model 的步骤

#### 父向子同步数据(数据由父提供)：

① 父组件通过 v-bind: 属性绑定的形式，把数据传递给子组件

② 子组件中，通过 props 接收父组件传递过来的数据  

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108061647767.png" alt="image-20210806164753694" style="zoom:67%;" />

```vue
//父组件
<template>
    <div>
        <h1>APP根组件----{{count}}</h1>
        <button @click='count++'>+1</button>
        <hr>
        <my-counter :number='count'></my-counter>
    </div>
</template>
<script>
import MyCounter from './Count.vue'

export default {
    name:'MyApp',
    data(){
        return {
            count:0
        }
    },
    components:{
        MyCounter
    }
}
</script>
```

```vue
//子组件
<template>
    <div>
        <p>
            count值是：{{number}}
        </p>
    </div>
</template>
<script>
export default {
    name:'MyCounter',
    props:['number']
}
</script>
```

#### 子向父传递数据(数据由父提供)：

① 在 v-bind: 指令之前添加 v-model 指令

② 在子组件中声明 emits 自定义事件，格式为 update:xxx

③ 调用 $emit() 触发自定义事件，更新父组件中的数据  

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108061700899.png" alt="image-20210806170013827" style="zoom:67%;" />

```vue
// 父组件
<template>
    <div>
        <h1>APP根组件----{{count}}</h1>
        <button @click='count++'>+1</button>
        <hr>
        <my-counter v-model:number='count'></my-counter>
    </div>
</template>
<script>
import MyCounter from './Count.vue'

export default {
    name:'MyApp',
    data(){
        return {
            count:0
        }
    },
    components:{
        MyCounter
    }
}
</script>
```

```vue
// 子组件
<template>
    <div>
        <p>count值是：{{number}}</p>
        <button @click='add'>+1</button>
    </div>
</template>
<script>
export default {
    name:'MyCounter',
    props:['number'],
    emits:['update:number'],
    methods: {
        add(){
            // 注意不是this.number++, 通过props接收的值不应该被修改，但是可以将props接收的值+1传递给父组件，由父组件来更新props的值
            this.$emit('update:number',this.number+1)
        }
    },
}
</script>
```





# Vue知识点

## 1.计算属性

### 1.什么是计算属性

计算属性本质上就是一个 function 函数，它可以实时监听 data 中数据的变化，并 return 一个计算后的新值，供组件渲染 DOM 时使用。  

### 2. 如何声明计算属性

计算属性需要以 function 函数的形式声明到组件的 computed 选项中，示例代码如下：

```vue
<template>
  <div>
    <input type="text" v-model.number="count" >
    <p>{{count}}乘以2的值为：{{plus}}</p>
  </div>
</template>

<script>
export default {
    data(){
        return {count:1}
    },
    computed:{
        plus(){
            // 计算属性，监听data中count值的变化，并自动计算出 count*2的新值
            return this.count*2;
        }
    }
};
</script>
```

注意：计算属性侧重于得到一个计算的结果，因此计算属性中必须有 return 返回值！  

### 3. 计算属性的使用注意点

① 计算属性必须定义在 computed 节点中

② 计算属性必须是一个 function 函数

③ 计算属性必须有返回值

④ 计算属性必须当做普通属性使用  

### 4. 计算属性 vs 方法

相对于方法来说，计算属性会缓存计算的结果，只有计算属性的依赖项发生变化时，才会重新进行运算。因此计算属性的性能更好  :

```vue
<template>
  <div>
    <input type="text" v-model.number="count" >
    <p>{{ count }}乘以2的值为：{{ plus }}  {{time()}}</p>
    <p>{{ count }}乘以2的值为：{{ plus }}  {{time()}}</p>
    <p>{{ count }}乘以2的值为：{{ plus }}  {{time()}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return { count: 1 };
  },
  computed: {
    plus() {
      console.log('计算属性被执行了');
      return this.count * 2;
    },
  },
  methods: {
      time(){
          console.log('方法被执行了');
          return this.count*2;
      }
  },
};
</script>
```

## 2.watch侦听器

### 1.什么是 watch 侦听器

watch 侦听器允许开发者监视数据的变化，从而针对数据的变化做特定的操作。例如监视用户名的变化并发起请求，判断用户名是否可用 。

2.watch 侦听器的基本语法

开发者需要在 watch 节点下，定义自己的侦听器。实例代码如下：  

```vue
<template>
    <div>
        <h3>watch 侦听器的用法</h3>
        <input type="text" v-model.trim="username">
    </div>
</template>

<script>
export default {
    data(){
        return {
            username:''
        }
    },
    watch:{
        username(newValue,oldValue){
            console.log(newValue,oldValue);
        }
    }
}
</script>
```

### 3. 使用 watch 查询商铺

监听 username 值的变化，并使用 axios 发起 Ajax 请求，检测当前输入的用户名是否可用。

首先需要下载`axios`:

```bash
npm install axios
```

```vue
<template>
    <div>
        <h3>watch 侦听器的用法</h3>
        <input type="text" v-model.trim="shopid">
    </div>
</template>

<script>
import axios from 'axios'


export default {
    name:'MyWatch',
    data(){
        return {
            shopid:''
        }
    },
    watch:{
        async shopid(newValue,oldValue){
            //解析出data，并重命名为res
            const {data:res}=await axios.get('https://www.escook.cn/shops/'+newValue);
            console.log(res);
        }
    }
}
</script>
```

### 4. immediate 选项

默认情况下，组件在初次加载完毕后不会调用 watch 侦听器。比如，,设置shopid的默认值为1，会发现此时控制台并没有打印店铺信息。

```js
import axios from 'axios'


export default {
    name:'MyWatch',
    data(){
        return {
            shopid:1
        }
    },
    watch:{
        async shopid(newValue,oldValue){
            //解析出data，并重命名为res
            const {data:res}=await axios.get('https://www.escook.cn/shops/'+newValue);
            console.log(res);
        }
    }
}
```

如果想让 watch 侦听器**立即被调用**，则需要使用 immediate 选项。实例代码如下：  

```js
export default {
    name:'MyWatch',
    data(){
        return {
            shopid:1
        }
    },
    watch:{
        // async shopid(newValue,oldValue){
        //     //解析出data，并重命名为res
        //     const {data:res}=await axios.get('https://www.escook.cn/shops/'+newValue);
        //     console.log(res);
        // }
        
        
        // 1.监听shopid值的变化，此时的shopid变成了对象
        shopid:{
            // 2.handler属性为固定写法，当username变化时，调用handler
            async handler(newValue,oldValue){
                const {data:res}=await axios.get('https://www.escook.cn/shops/'+newValue);
                console.log(res);
            },
            // 3.表示组件加载完毕后调用一次当前的watch侦听器
            immediate:true
        }
    }
}
</script>
```

### 5. deep 选项  

当 **watch 侦听的是一个对象**，如果对象中的属性值发生了变化，则无法被监听到。此时需要使用 deep 选项，代码示例如下：  

```vue
<script>
import axios from 'axios'
export default {
    name:'MyWatch',
    data(){
        return {
            info:{
                shopid:1
            }
        }
    },
    watch:{
        info:{
            // handler属性为固定写法，当username变化时，调用handler
            async handler(newValue,oldValue){
                const {data:res}=await axios.get('https://www.escook.cn/shops/'+newValue.shopid);
                console.log(res);
            },
            // 表示组件加载完毕后调用一次当前的watch侦听器
            immediate:true,
            //需要使用deep选项，否则info.shopid值的变化无法被监听
            deep:true
        }
    }
}
</script>
```

### 6. 监听对象单个属性的变化

开启deep选项后，watch会监听对象中的每一个属性值，任何一个属性值发生变化，都会触发handler处理函数。**如果只想监听对象中单个属性的变化**，则可以按照如下的方式定义 watch 侦听器：  

```vue

<script>
import axios from "axios";

export default {
  name: "MyWatch",
  data() {
    return {
      info: {
        shopid: 1,
      },
    };
  },
  watch: {
    "info.shopid": {
      async handler(newValue, oldValue) {
        const { data: res } = await axios.get(
          "https://www.escook.cn/shops/" + newValue
        );
        console.log(res);
      },
      immediate:true,
    },
  },
};
</script>
```

### 7. 计算属性 vs 侦听器

计算属性和侦听器侧重的应用场景不同：

**计算属性侧重于监听多个值的变化，最终计算并返回一个新值。**

**侦听器侧重于监听单个数据的变化，最终执行特定的业务处理**，不需要有任何返回值。

## 3. 组件的声明周期

### 1. 组件运行的过程

![image-20210808162852517](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108081628645.png)

组件的生命周期指的是：组件从创建 -> 运行（渲染） -> 销毁的整个过程，强调的是一个时间段。  

### 2. 如何监听组件的不同时刻

vue 框架为组件**内置**了不同时刻的生命周期函数，**生命周期函数会伴随着组件的运行而自动调用。**例如：

① 当组件在内存中被**创建完毕**之后，会自动调用 **created** 函数

② 当组件被成功的**渲染到页面上**之后，会自动调用 **mounted** 函数

③ 当组件被**销毁完毕**之后，会自动调用 **unmounted** 函数  

当在父组件中使用子组件后，会调用created函数和mounted函数，**当使用v-if隐藏子组件后，会调用unmounted函数**

```vue
// app.vue
<template>
  <div>
    <h1>根组件</h1>
    <life-cycle v-if="flag"></life-cycle>
    <button @click="flag = !flag">Toggle</button>
  </div>
</template>

<script>
import LifeCycle from "./LifeCycle.vue";
export default {
  name: "MyApp",
  data() {
    return {
      flag: true,
    };
  },
  components: {
    LifeCycle,
  },
};
</script>
```

```vue
//LifeCycle.vue
<template>
    <div>
        <h3>LifeCycle 组件</h3>
    </div>
</template>

<script>
export default {
    name:'LifeCycle',
    // 组件在内存中被创建完毕了
    created(){
        console.log('created:组件在内存中被创建完毕了');
    },
    // 组件被渲染到页面中
    mounted() {
        console.log('mounted:组件被渲染到页面中');
    },
    // 组件被销毁
    unmounted() {
        console.log('unmounted:组件被销毁');
    },
}
</script>
```

### 3. 如何监听组件的更新

**当组件的 *data* 数据更新之后，vue 会自动重新渲染组件的 DOM 结构，从而保证 View 视图展示的数据和Model 数据源保持一致。**

当组件被重新渲染完毕之后，会自动调用 `updated` 生命周期函数。  

```vue
<template>
    <div>
        <h3>LifeCycle 组件---{{count}}</h3>
        <button @click='count++'>+1</button>
    </div>
</template>

<script>
export default {
    name:'LifeCycle',
    data(){
        return {
            count:0
        }
    },
    // 组件在内存中被创建完毕了
    created(){
        console.log('created:组件在内存中被创建完毕了');
    },
    // 组件被渲染到页面中
    mounted() {
        console.log('mounted:组件被渲染到页面中');
    },
    // 组件更新
    updated() {
        console.log('updated:组件被重新渲染完毕了');
    },
    // 组件被销毁
    unmounted() {
        console.log('unmounted:组件被销毁');
    },
}
</script>
```

![动23](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108081704197.gif)

### 4. 组件中主要的生命周期函数

![image-20210808170708405](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108081707478.png)

注意：在实际开发中，created 是最常用的生命周期函数！  

### 5. 组件中全部的生命周期函数

![image-20210808170909186](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108081709280.png)

### 6. 完整的生命周期图示

可以参考 vue [官方文档](https://www.vue3js.cn/docs/zh/guide/instance.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)给出的“生命周期图示”，进一步理解组件生命周期执行的过程：


![生命周期图示](https://www.vue3js.cn/docs/zh/images/lifecycle.png)

## 4.组件中的数据共享

### 1. 组件之间的关系  

在项目开发中，组件之间的关系分为如下 3 种：

① 父子关系

② 兄弟关系

③ 后代关系  

### 2. 父子组件之间的数据共享

父子组件之间的数据共享又分为：

① 父 -> 子共享数据

父组件通过 **v-bind 属性**绑定向子组件共享数据。同时，子组件需要使用 **props** 接收数据。示例代码如下：  

```vue
// 父组件
<template>
    <div>
        <h1>App 根组件--{{count}}</h1>
        <button @click='count++'>+ 1</button>
        <hr>
        <my-son :num='count'></my-son>
    </div>
</template>

<script>
import MySon from './Son.vue'

export default {
    name:'MyApp',
    components:{
        MySon
    },
    data(){
        return {
            count:0
        }
    }   
}
</script>
```

```vue
// 子组件
<template>
    <div>
        <h3>子组件 {{num}}</h3>
    </div>
</template>

<script>
export default {
    name:'SON',
    props:['num'],
}
</script>
```

② 子 -> 父共享数据

子组件通过自定义事件的方式向父组件共享数据。示例代码如下：  

```vue
// 父组件
<template>
    <div>
        <h1>App 根组件--{{count}}</h1>
        <button @click='count++'>+ 1</button>
        <hr>
        <my-son :num='count' @numChange="getNum"></my-son>
    </div>
</template>

<script>
import MySon from './Son.vue'

export default {
    name:'MyApp',
    components:{
        MySon
    },
    data(){
        return {
            count:0
        }
    },
    methods: {
        getNum(value){
            // 将子组件传递的num + 1 赋值给count
            this.count=value;
        }
    },   
}
</script>
```

```vue
// 子组件
<template>
    <div>
        <h3>子组件 {{num}}</h3>
        <button @click='add'>+1</button>
    </div>
</template>

<script>
export default {
    name:'SON',
    props:['num'],
    emits:['numChange'],
    methods: {
        add(){
            // 将最新的num加1，然后通过自定义事件传递出去
            this.$emit('numChange',this.num+1);
        }
    },
}
</script>
```



③ 父 <-> 子双向数据同步  

父组件在使用子组件期间，可以使用 v-model 指令维护组件内外数据的双向同步：  

![image-20210808174949239](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108081749342.png)

使用`v-model` 后，父组件便不需要监听子组件自定义的事件。

```vue
// 父组件
<template>
    <div>
        <h1>App 根组件--{{count}}</h1>
        <button @click='count++'>+ 1</button>
        <hr>
        <!--要传递给子组件的值，前面加v-model -->
        <my-son v-model:num='count' ></my-son>
    </div>
</template>

<script>
import MySon from './Son.vue'

export default {
    name:'MyApp',
    components:{
        MySon
    },
    data(){
        return {
            count:0
        }
    },
}
</script>
```

```vue
// 子组件
<template>
    <div>
        <h3>子组件 {{num}}</h3>
        <button @click='add'>+1</button>
    </div>
</template>

<script>
export default {
    name:'SON',
    props:['num'],
    // 'update:要更新的值' 是固定写法
    emits:['update:num'],
    methods: {
        add(){
            // 将最新的num加1
            // 'update:要更新的值' 是固定写法
            this.$emit('update:num',this.num+1);
        }
    },
}
</script>
```

### 3. 兄弟组件之间的数据共享

兄弟组件之间实现数据共享的方案是 `EventBus`。可以借助于第三方的包 `mitt` 来创建 eventBus 对象，从而实现兄弟组件之间的数据共享。示意图如下：  

![image-20210808175921307](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108081759439.png)



#### 3.1 安装 mitt 依赖包

在项目中运行如下的命令，安装 mitt 依赖包：  

```bash
npm install mitt
```

#### 3.2 创建公共的 EventBus 模块

在项目中创建公共的 `eventBus 模块`如下：  

```js
// 创建一个JS文件 eventBus.js

// 导入mitt包
import mitt from 'mitt';

// 创建EventBus的实例对象
const bus=mitt();

// 将EventBus的实例对象共享出去
export default bus;
```

#### 3.3 在数据接收方自定义事件

在数据接收方，调用 `bus.on('事件名称', 事件处理函数)` 方法注册一个自定义事件。示例代码如下：  

```vue
// 数据接收方
<script>

// 导入eventBus.js 模块，得到共享的bus对象
import bus from './eventBus'

export default {
    name:'MyRight',
    data(){
        return {num:0};
    },
    //当组件在内存中被创建完毕后
    created(){
        //调用bus.on()方法注册一个自定义事件，通过事件处理函数的形参接收数据
        bus.on('countChange',(count)=>{
            this.num=count;
        })
    }
}
</script>
```

#### 3.4 在数据接发送方触发事件

在数据发送方，调用 `bus.emit('事件名称', 要发送的数据) `方法**触发自定义事件**。示例代码如下：  

```vue
<template>
    <div>
        <div>数据发送</div>
        <p>count的值:{{count}}</p>
        <button @click="add">+1</button>
    </div>

</template>

<script>

// 导入eventBus.js模块，得到共享的bus对象
import bus from './eventBus';

export default {
    data(){
        return {count:0};
    },
    methods: {
        add(){
            this.count++;
            // 调用bus.emit()方法触发自定义事件，并发送数据
            bus.emit('countChange',this.count);
        }
    },

}
</script>
```

### 4. 后代关系组件之间的数据共享  

后代关系组件之间共享数据，指的是**父节点的组件**向其**子孙组件**共享数据。此时组件之间的嵌套关系比较复杂，可以使用 `provide` 和 `inject` 实现后代关系组件之间的数据共享。  

![image-20210808194300054](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108081943125.png)

#### 4.1 父节点通过 provide 共享数据

父节点的组件可以通过 **`provide` 方法**，对其**子孙组件**共享数据：  

```vue
<script>
export default {
    data(){
        return {
            // 定义父组件要向子组件共享的数据
            color:'red'
        }
    },
    provide(){
        // 向子孙组件提供需要共享的数据
        return {
            color:this.color
        }
    }
}
</script>
```

#### 4.2 子孙节点通过 inject 接收数据

子孙节点可以使用 `inject` **数组**，接收父级节点向下共享的数据。示例代码如下：  

```vue
<script>
export default {
    name:'level-three',
    // 子孙组件，使用inject接收父节点向下共享的color数据
    inject:['color'],
}
</script>
```

#### 4.3 父节点对外共享响应式的数据

如果父节点修改了数据，但子节点接收的值并没有改变。因此，父节点使用 provide 向下共享数据时，可以结合 **computed** **函数**向下共享响应式的数据。示例代码如下：  

```vue
<script>
import LevelTwo from './LevelTwo.vue'
// 1.从vue中按需导入computed函数
import {computed} from 'vue'

export default {
    name:'level-one',
    components:{
        LevelTwo
    },
    data(){
        return {
            color:'red',
        }
    },
    provide(){
        // 2.使用computed函数，可以要共享的数据包装为响应式的数据
        return {
            color:computed(()=>this.color),
        }
    }
}
</script>
```

#### 4.4 子孙节点使用响应式的数据

如果父级节点共享的是响应式的数据，则子孙节点必须以 .value 的形式进行使用。示例代码如下：  

```vue
<template>
    <div>
        <h5>第三层级 {{color.value}}</h5>
        <hr>
    </div>
</template>

<script>
export default {
    name:'level-three',
    // 子孙组件，使用inject接收父节点向下共享的color数据
    inject:['color'],
}
</script>
```

### 5. vuex

vuex 是终极的组件之间的数据共享方案。在企业级的 vue 项目开发中，vuex 可以让组件之间的数据共享变得高效、清晰、且易于维护。

 <img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108082021382.png" alt="image-20210808202145298" style="zoom:80%;" />

###  6. 总结

1.父子关系

① 父 -> 子 属性绑定

② 子 -> 父 事件绑定

③ 父 <-> 子 组件上的 v-model

2.兄弟关系

④ EventBus

3.后代关系

⑤ provide & inject

4.全局数据共享
⑥ vuex  

# 全局配置 axios  

## 1. 为什么要全局配置 axios

在实际项目开发中，几乎每个组件中都会用到 axios 发起数据请求。此时会遇到如下两个问题：

① 每个组件中都需要导入 axios（代码臃肿）

![image-20210808202541733](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108082025860.png)

② 每次发请求都需要填写完整的请求路径（不利于后期的维护）  

## 2. 如何全局配置 axios

在 main.js 入口文件中，通过 `app.config.globalProperties` 全局挂载 axios，其中$http为自定义属性，示例代码如下：  

![image-20210808202805130](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108082028265.png)

```js
import { createApp } from 'vue'
import App from './components/06.network/App.vue'
import './index.css'

import axios from 'axios';

const app=createApp(App);

axios.defaults.baseURL='http://www.escook.cn';

// 全局挂载axios
app.config.globalProperties.$http=axios;

app.mount('#app');
```

组件便可以简便地使用`axios`:

```vue
<template>
    <div>
        <h3>Get Info 组件</h3>
        <hr>
        <button @click=" getInfo">发起GET请求</button>
    </div>
</template>

<script>
export default {
    name:'GetInfo',
    methods: {
        async getInfo(){
           let res=await this.$http.get('/shops',{params:{
               id:1
           }});
           console.log(res.data);
        }
    },
}
</script>
```

# ref引用

## 1. 什么是 ref 引用

ref 用来辅助开发者在**不依赖于 jQuery** 的情况下，*获取 DOM 元素或组件的引用*。
每个 vue 的组件实例上，都包含一个 **$refs 对象**，里面存储着对应的 DOM 元素或组件的引用。默认情况下，**组件的 $refs 指向一个空对象**。  

## 2. 使用 ref 引用 DOM 元素

如果想要使用 ref 引用页面上的 DOM 元素，则可以按照如下的方式进行操作：  

```vue
<template>
  <div>
    <!-- 使用ref属性，为对应的DOM对象添加引用名称 -->
    <h1 ref='myH1'>App根组件</h1>
    <button @click="getRefs">获取$refs引用</button>
  </div>
</template>

<script>
export default {
  name: "MyApp",
  methods: {
    getRefs() {
    // 通过this.$refs 引用的名称可以获取到dom元素的引用
      console.log(this.$refs); //{myH1: h1}
      this.$refs.myH1.style.color='red';
    },
  },
};
</script>
```

## 3. 使用 ref 引用组件实例

使用$refs拿到组件的引用后，可以调用组件的方法。

```vue
//父组件
<template>
  <div>
    <h1 >App根组件</h1>
    <button @click="getRefs" >重置为0</button>
    <my-counter ref='counterRef'></my-counter>
  </div>
</template>

<script>
import MyCounter from './counter.vue'
export default {
  name: "MyApp",
  components:{MyCounter},
  methods: {
    getRefs() {
        // this.$refs.counterRef获取到组件
        // 然后调用组件的方法reset
        this.$refs.counterRef.reset();
    },
  },
};
</script>
```

```vue
//被引用的组件
<template>
  <div class="counter-container">
      <h3>counter组件---{{count}}</h3>
      <button type='button' class="btn btn-info" @click='count+=1'>+1</button>
  </div>
</template>

<script>
export default {
    name:'MyCounter',
    data(){
        return {
            count:0,
        }
    },
    methods: {
        reset(){
            this.count=0;
        }
    },
}
</script>

```

## 4. this.$nextTick(回调函数) 方法  

通过布尔值 inputVisible 来控制组件中的文本框与按钮的按需切换。当文本框展示出来之后，如果希望它立即获得焦点，则尝试为其添加 ref 引用，并调用原生 DOM 对象的.focus() 方法，但由于组件的dom元素是异步更新的，当执行.focus()方法时，还没有获取到对应的dom元素。因此，会在控制台看到报错。

```vue
<template>
  <div>
      <h1>App 根组件</h1>
      <hr>
      <input type="text" v-if='inputVisible' ref='ipt'>
      <button v-else @click='showInput'>展示输入框</button>
  </div>
</template>

<script>
export default {
    name:'MyApp',
    data(){
        return {inputVisible:false}
    },
    methods: {
        showInput(){
            //展示文本框
            this.inputVisible=true;
            // dom元素是异步更新的，当点击了按钮后，dom还没有来得及更新
            // 因此this.$refs.ipt 为undefined
            console.log(this.$refs.ipt); //undefined
            // 自动获取焦点
            this.$refs.ipt.focus();
        }
    },
}
</script>
```

组件的 `$nextTick(回调函数)` 方法，会**把回调函数推迟到下一个 DOM 更新周期之后执行。**通俗的理解是：等组件的DOM 异步地重新渲染完成后，再执行 回调函数，从而能保证 回调函数可以操作到最新的 DOM 元素。  

```vue
<script>
export default {
    name:'MyApp',
    data(){
        return {inputVisible:false}
    },
    methods: {
        showInput(){
            //展示文本框
            this.inputVisible=true;
            // 把对文本框的操作推迟到下次dom更新后，否则页面上根本不存在文本框
            this.$nextTick(()=>{
                this.$refs.ipt.focus();
            })
        }
    },
}
</script>
```

# 动态组件

## 1. 什么是动态组件

动态组件指的是**动态切换组件的显示与隐藏**。vue 提供了一个内置的 `<component>` 组件，专门用来实现组件的动态渲染。  

① `<component>` 是组件的占位符

② 通过 `is` 属性动态指定要渲染的组件名称

③ `<component is="要渲染的组件的名称"></component>`  

## 2. 如何实现动态组件渲染

```vue
<template>
  <div>
      <h1>App 根组件</h1>
      <!-- 3.点击按钮动态切换组件的名称 -->
      <!-- 注意组件的有引号包裹着 -->
      <button @click="comName='MyHome'">首页</button>
      <button @click="comName='MyMovie'">电影</button>
      <!-- 2.通过is属性，动态指定要渲染的组件名称 -->
      <component :is='comName'></component>
  </div>
</template>

<script>
export default {
    components:{
        MyHome,MyMovie
    },
    data(){
        return {
            // 1.当前要渲染的组件名称
            comName:'MyHome',
        }
    }
}
</script>

```

## 3. 使用 keep-alive 保持状态

在home组件声明了一个计数的变量count=0，点击按钮count的时候，count值会增加。但是当切换到了moive组件，再切换回home组件后，会发现count的值恢复到0了。

![组件](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108091826851.gif)

默认情况下，切换动态组件时无法保持组件的状态。此时可以使用 vue 内置的 `<keep-alive>` 组件保持动态组件的状态。组件切换后，并没有被销毁。

```vue
<keep-alive>
    <component :is='comName'></component>
</keep-alive>
```

![组件没有被销毁](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108091829447.gif)

# 插槽

## 1. 什么是插槽

插槽（Slot）是 vue 为组件的封装者提供的能力。允许开发者在封装组件时，把**不确定的、希望由用户指定的部分**定义为插槽。  

![image-20210809183221206](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108091832326.png)

可以把插槽认为是组件封装期间，为用户预留的**内容的占位符**。  

## 2. 体验插槽的基础用法

在封装组件时，可以通过 `<slot>` 元素定义插槽，从而**为用户预留内容占位符**。示例代码如下：  

```vue
//MyCom.vue
<template>
  <div>
      <p>这是第一个p标签</p>
      <!-- 通过slot标签，为用户预留内容占位符 -->
      <slot></slot>
      <p>最后一个p标签</p>
  </div>
</template>
```

```vue
//App.vue 
<template>
  <div>
    <my-com>
        <!-- 在使用my-com组件时，为插槽填充内容 -->
      <p>hhhhhhh</p>
    </my-com>
  </div>
</template>
```

### 2.1 没有预留插槽的内容会被丢弃

如果在封装组件时没有预留任何 `<slot>` 插槽，则用户提供的任何自定义内容都会被丢弃。

```vue
<template>
      <p>这是第一个p标签</p>
      <!-- 封装组件时，没有预留任何插槽 -->
      <p>最后一个p标签</p>
</template>
```

```vue
<my-com>
    <!-- 自定义的内容会被丢弃 -->
  <p>hhhhhhh</p>
</my-com>
```

### 2.2 后备内容

封装组件时，可以为预留的 `<slot>` 插槽提供后备内容（默认内容）。如果组件的使用者没有为插槽提供任何内容，则后备内容会生效。

```vue
<template>
  <div>
    <p>这是第一个p标签</p>
    <slot>这是后备内容</slot>
    <p>最后一个p标签</p>
  </div>
</template>
```

## 3. 具名插槽

如果在封装组件时需要**预留多个插槽节点**，则需要为每个 `<slot>` 插槽指定具体的 name 名称。这种带有具体名称的插槽叫做“**具名插槽**”。示例代码如下：  

```vue
<div>
    <header>
      <!-- 希望把页头放在这里 -->
      <slot name="header"></slot>
    </header>
    <main>
      <!-- 我们希望把主要内容放在这里 -->
      <slot name="main"></slot>
    </main>
    <footer>
      <!-- 我们希望把页脚放在这里 -->
      <slot name="footer"></slot>
    </footer>
</div>
```

注意：没有指定 name 名称的插槽，会有隐含的名称叫做 “default”。  

### 3.1 为具名插槽提供内容

在向具名插槽提供内容的时候，我们可以在一个 `<template>` 元素上使用 v-slot 指令，并以 v-slot 的参数的形式提供其名称。示例代码如下：  

```vue
<template>
  <div>
    <h1>APP 根组件</h1>
    <my-com>
      <template v-slot:header>
        <p>文章标题</p>
      </template>
      <template v-slot:main>
        <p>文章内容</p>
      </template>
      <template v-slot:footer>
        <p>结尾</p>
      </template>
    </my-com>
  </div>
</template>
```

只有默认插槽使用的时候可以省略template，其余的插槽均不能省略template。

### 3.2 具名插槽的简写形式

跟 v-on 和 v-bind 一样，v-slot 也有缩写，即把参数之前的所有内容 (`v-slot:`) 替换为字符 `#`。例如 `v-slot:header`可以被重写为 `#header`  。

```vue
<my-com>
  <template #header>
    <p>文章标题</p>
  </template>
  <template #main>
    <p>文章内容</p>
  </template>
  <template #footer>
    <p>结尾</p>
  </template>
</my-com>
```

## 4. 作用域插槽

在封装组件的过程中，可以为预留的 `<slot>` 插槽绑定 props 数据，这种**带有 props 数据的 `<slot>` 叫做“作用域插槽”**。示例代码如下：  

```vue
<template>
  <div id="box">
    <h3>这是MyCom组件</h3>
    <!-- 插槽给组件提供数据 -->
    <slot :info="information"></slot>
  </div>
</template>

<script>
export default {
  data() {
    return {
      information: {
        phone: 12122133,
        address: "北京市",
      },
    };
  },
};
</script>
```

接收插槽中的数据：

```vue
<my-com>
  <!-- 如把接收的数据命名为scope -->
  <template v-slot:default='scope'>
    <p>{{scope}}</p>
<!-- { "info": { "phone": 12122133, "address": "北京市" } } -->
  </template>
</my-com>
```

### 4.1 解构作用域插槽的 Prop

作用域插槽对外提供的数据对象，可以使用解构赋值简化数据的接收过程。示例代码如下：  

```vue
<my-com>
  <!-- 解构赋值接收的数据 -->
  <template v-slot:default="{ info }">
    <p>{{ info.address }}</p>
    <!-- 北京市 -->
  </template>
</my-com>
```

### 4.2 使用作用域插槽的小案例

向用户提供mytable组件和table中的数据，但是用户想要按照自己的想法渲染表格。在封装 MyTable 组件的过程中，可以通过作用域插槽把表格每一行的数据传递给组件的使用者。  

```vue
//mytable.vue
<template>
  <table>
      <tr>
          <th>Id</th>
          <th>Name</th>
          <th>State</th>
      </tr>
      <tr v-for='item in list' :key='item.id'>
          <slot :user='item'>
          </slot>
      </tr>
  </table>
</template>

<script>
export default {
    name:'MyTable',
    data(){
        return{
            // 列表的数据
            list:[
                {id:1,name:'张三',state:true},
                {id:2,name:'李四',state:false},
                {id:3,name:'王五',state:true},
            ]
        }
    }
}
</script>
```

在使用 MyTable 组件时，自定义单元格的渲染方式，并接收作用域插槽对外提供的数据。示例代码如下：  

```vue
<my-table>
  <template #default='{user}'>
    <td>{{user.id}}</td>
    <td>{{user.name}}</td>
    <td><input type="checkbox" :checked="user.state"></td>
  </template>
</my-table>
```

# 自定义指令

## 1. 什么是自定义指令

vue 官方提供了 v-for、v-model、v-if 等常用的内置指令。除此之外 vue 还允许开发者自定义指vue 中的自定义指令分为两类，分别是：

- 私有自定义指令

- 全局自定义指令  

## 2. 声明私有自定义指令的语法

在每个 vue 组件中，可以在 directives 节点下声明私有自定义指令。示例代码如下：  

```js
directives:{
    // 自定义一个私有指令
    // 声明指令的时候不需要加v- ,使用自定义指令的时候需要加v-
    focus:{
        // 当被绑定的元素插入到dom中时，自动触发mounted函数
        mounted(el) {
            // 让绑定的元素获得焦点
            el.focus() 
        },
    }
}
```

## 3. 使用自定义指令

在使用自定义指令时，需要加上 v- 前缀。示例代码如下：  

```vue
<!-- 声明自定义指令时，指令的名字是focus -->
<!-- 使用自定义指令时，需要加v-前缀 -->
<input type="text" v-focus>
```

## 4. 声明全局自定义指令的语法

全局共享的自定义指令需要通过“单页面应用程序的实例对象”进行声明，示例代码如下：  

```js
//main.js
import { createApp } from 'vue'
import App from './components/05.directive/App.vue'
import './index.css'

const app=createApp(App);

//===========================
//注册一个全局自定义指令，'v-focus'
app.directive('focus',{
    mounted(el){
        el.focus();
    }
})
//===========================

app.mount('#app');
```

## 5.updated 函数

**mounted 函数只在元素第一次插入 DOM 时被调用**，当 DOM 更新时 mounted 函数不会被触发。 **updated函数会在每次 DOM 更新完成后被调用。**示例代码如下：  

```js
directives: {
focus: {
    mounted(el) {
    //第一次插入dom时触发这个函数
    el.focus();
    },
    updated(el) {
    // 每次dom更新时 都会触发updated函数
    el.focus();
    },
},
},
```

注意：在 vue2 的项目中使用自定义指令时，【 mounted -> bind 】【 updated -> update 】  

## 6. 函数简写

如果 mounted 和updated 函数中的逻辑完全相同，则可以简写成如下格式：  

```js
//全局
app.directive('focus',(el)=>{
    // 在mounted和updated时都会被触发
    el.focus();
})
//===============
// 私有指令
directives:{
    focus(el){
        el.focus();
    }
}
```

## 7. 指令的参数值

在绑定指令时，可以通过“等号”的形式为指令绑定具体的参数值，示例代码如下：  

```vue
<h1 v-color="'red'">App 根组件</h1>

app.directive('color',(el,binding)=>{
    // binding.value 便可以获取通过等号为指令绑定的值
    el.style.color=binding.value;
})
```



