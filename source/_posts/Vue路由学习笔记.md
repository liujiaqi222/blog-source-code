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

   

