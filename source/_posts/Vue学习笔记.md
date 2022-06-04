---
title: Vue学习笔记
date: 2021-07-27 18:38:33
tags: Vue
---


笔记基于：https://www.runoob.com/vue2/

# Vue起步

每个 Vue 应用都需要通过实例化 Vue 来实现。

语法格式如下：

```html
<div id="app">
    <h1>site : {{site}}</h1>
    <h1>url : {{url}}</h1>
    <h1>{{details()}}</h1>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            site: "菜鸟教程",
            url: "www.runoob.com",
            alexa: "10000"
        },
        methods: {
            details: function() {
                return  this.site + " - 学的不仅是技术，更是梦想！";
            }
        }
    })
</script>
```

可以看到在 Vue 构造器中有一个el 参数，它是 DOM 元素中的 id。在上面实例中 id 为 app，在 div 元素中：

```html
<div id = "app"></div>
```

这意味着我们接下来的改动全部在以上指定的 div 内，div 外部不受影响。

接下来我们看看如何定义数据对象。

**data** 用于定义属性，实例中有三个属性分别为：site、url、alexa。

**methods** 用于定义的函数，可以通过 return 来返回函数值。

`{{ }}` 用于输出对象属性和函数返回值。

```html
<div id="vue_det">
    <h1>site : {{site}}</h1>
    <h1>url : {{url}}</h1>
    <h1>{{details()}}</h1>
</div>
```

当一个 Vue 实例被创建时，它向 Vue 的响应式系统中加入了其 data 对象中能找到的所有的属性。当这些属性的值发生改变时，html 视图将也会产生相应的变化。

## 实例一

```html
<div id="vue_det">
    <h1>site : {{site}}</h1>
    <h1>url : {{url}}</h1>
    <h1>Alexa : {{alexa}}</h1>
</div>
<script type="text/javascript">
// 我们的数据对象
var data = { site: "菜鸟教程", url: "www.runoob.com", alexa: 10000}
var vm = new Vue({
    el: '#vue_det',
    data: data
})
// 它们引用相同的对象！
document.write(vm.site === data.site) // true
document.write("<br>")
// 设置属性也会影响到原始数据
vm.site = "Runoob"
document.write(data.site + "<br>") // Runoob
 
// ……反之亦然
data.alexa = 1234
document.write(vm.alexa) // 1234
</script>
```

## 实例二

除了数据属性，Vue 实例还提供了一些有用的实例属性与方法。它们都有前缀 $，以便与用户定义的属性区分开来。例如：

```js
<div id="vue_det">
    <h1>site : {{site}}</h1>
    <h1>url : {{url}}</h1>
    <h1>Alexa : {{alexa}}</h1>
</div>
<script type="text/javascript">
// 我们的数据对象
var data = { site: "菜鸟教程", url: "www.runoob.com", alexa: 10000}
var vm = new Vue({
    el: '#vue_det',
    data: data
})
// 它们引用相同的对象！
document.write(vm.site === data.site) // true
document.write("<br>")
// 设置属性也会影响到原始数据
vm.site = "Runoob"
document.write(data.site + "<br>") // Runoob
 
// ……反之亦然
data.alexa = 1234
document.write(vm.alexa) // 1234
</script>
```

# 模板语法

Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。

Vue.js 的核心是一个允许你采用简洁的模板语法来声明式的将数据渲染进 DOM 的系统。

结合响应系统，在应用状态改变时， Vue 能够智能地计算出重新渲染组件的最小代价并应用到 DOM 操作上。

## 插值

### 文本

数据绑定最常见的形式就是使用 `{{...}}`（双大括号）的文本插值：

```html
<div id="app">
  <p>{{ message }}</p>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!'
  }
})
</script>
```

### html和text

 使用 v-html 指令用于输出 html 代码，使用v-text用于输出text代码：

```HTML
<div id="app">
    <div v-html="message"></div>
    <div v-text="message"></div>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    message: '<h1>菜鸟教程</h1>'
  }
})
</script>
```

![image-20210727192913338](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210727192921.png)

### 属性

HTML 属性中的值应使用 v-bind 指令。

以下实例判断 use 的值，如果为 true 使用 class1 类的样式，否则不使用该类：

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
</head>
<style>
.class1{
  background: #444;
  color: #eee;
}
</style>
<body>
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>

<div id="app">
  <label for="r1">修改颜色</label><input type="checkbox" v-model="use" id="r1">
  <br><br>
  <div v-bind:class="{'class1': use}">
    v-bind:class 指令
  </div>
</div>
    
<script>
new Vue({
    el: '#app',
  data:{
      use: false
  }
});
</script>
</body>
```

![image-20210727193358942](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210727193400.png)

### 表达式

Vue.js 都提供了完全的 JavaScript 表达式支持。

```html
<div id="app">
    {{5+5}}<br>
    {{ ok ? 'YES' : 'NO' }}<br>
    {{ message.split('').reverse().join('') }}
    <div v-bind:id="'list-' + id">菜鸟教程</div>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    ok: true,
    message: 'RUNOOB',
    id : 1
  }
})
</script>
```

### 指令

指令是带有 v- 前缀的特殊属性。

指令用于在表达式的值改变时，将某些行为应用到 DOM 上。如下例子：

```html
<div id="app">
    <p v-if="seen">现在你看到我了</p>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    seen: true
  }
})
</script>
```

这里， v-if 指令将根据表达式 seen 的值(true 或 false )来决定是否插入 p 元素。

另一个例子是 v-on 指令，它用于监听 DOM 事件：监听

```html
<a v-on:click="doSomething">
```

### 修饰符

修饰符是以半角句号 **.** 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，**.prevent** 修饰符告诉 **v-on** 指令对于触发的事件调用 **event.preventDefault()**：

```js
<form v-on:submit.prevent="onSubmit"></form>
```

## 用户输入

在 input 输入框中我们可以使用 v-model 指令来实现双向数据绑定：

```html
<div id="app">
    <p>{{ message }}</p>
    <input v-model="message">
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    message: 'Runoob!'
  }
})
</script>
```

使用v-model会将input等输入框的value发生变化，会将其值传给Vue实例data的message属性，同时如果Vue实例的message发生变化，也会将数据更新表单的value。

**v-model** 指令用来在 input、select、textarea、checkbox、radio 等表单控件元素上创建双向数据绑定，根据表单上的值，自动更新绑定的元素的值。

## 过滤器

Vue.js 允许你自定义过滤器，被用作一些常见的文本格式化。由"管道符"指示, 格式如下：

```html
<!-- 在两个大括号中 -->
{{ message | capitalize }}

<!-- 在 v-bind 指令中 -->
<div v-bind:id="rawId | formatId"></div>
```

过滤器函数接受表达式的值作为第一个参数。

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>
</head>
<body>
<div id="app">
  {{ message | capitalize }}
</div>
	
<script>
new Vue({
  el: '#app',
  data: {
	message: 'runoob'
  },
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
</script>
</body>
</html>
```

## 缩写

Vue.js 为两个最为常用的指令提供了特别的缩写：

### v-bind

```html
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```

### v-on 缩写

```html
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a
```

# Vue.js 条件和循环语句

## 条件判断

条件判断使用 v-if 指令：

### v-if

```html
<div id="app">
    <p v-if="seen">现在你看到我了</p>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    seen: true,
  }
})
</script>
```

这里， v-if 指令将根据表达式 seen 的值(true 或 false )来决定是否插入 p 元素。

### v-else

可以用 v-else 指令给 v-if 添加一个 "else" 块：

随机生成一个数字，判断是否大于0.5，然后输出对应信息：

```html
<div id="app">
    <div v-if="Math.random() > 0.5">
      Sorry
    </div>
    <div v-else>
      Not sorry
    </div>
</div>
    
<script>
new Vue({
  el: '#app'
})
</script>
```

### v-else-if

用作 v-if 的 else-if 块，可以链式的多次使用。

```html
<div id="app">
    <div v-if="type === 'A'">
      A
    </div>
    <div v-else-if="type === 'B'">
      B
    </div>
    <div v-else-if="type === 'C'">
      C
    </div>
    <div v-else>
      Not A/B/C
    </div>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    type: 'C'
  }
})
</script>
```

> v-else 、v-else-if 必须跟在 v-if 或者 v-else-if之后。

## 条件渲染指令

条件渲染指令用来辅助开发者按需控制 DOM 的显示与隐藏。条件渲染指令有如下两个，分别是：

- v-if
-  v-show  

```html
<div id="app">
    <button @click='flag=!flag'>Toggle Flag</button>
    <p v-if='flag'>显示：v-if</p>
    <p v-show='flag'>显示：v-show</p>
</div>

<script>
    new Vue({
        el: "#app",
        data: {
            // 用来控制元素的隐藏和显示
            flag:true
        },
    })
</script>
```

v-if 和 v-show 的区别

1.实现原理不同：

 v-if 指令会动态地创建或移除 DOM 元素，从而控制元素在页面上的显示与隐藏；

 v-show 指令会动态为元素添加或移除 style="display: none;" 样式，从而控制元素的显示与隐藏；

2.性能消耗不同：

v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。

- 如果需要非常频繁地切换，则使用 v-show 较好

- 如果在运行时条件很少改变，则使用 v-if 较好  

## 循环语句

循环使用 v-for 指令。v-for 指令需要以 **site in sites** 形式的特殊语法， sites 是源数据数组并且 site 是数组元素迭代的别名。

### v-for 遍历数组

v-for 可以绑定数据到数组来渲染一个列表：

```html
<div id="app">
  <ol>
    <li v-for="site in sites">
      {{ site.name }}
    </li>
  </ol>
</div>
<script>
new Vue({
  el: '#app',
  data: {
    sites: [
      { name: 'Runoob' },
      { name: 'Google' },
      { name: 'Taobao' }
    ]
  }
})
</script>
```

![image-20210727232644828](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210727232652.png)

### v-for迭代对象

v-for 可以通过一个对象的属性来迭代数据：

```html
<div id="app">
  <ul>
    <li v-for="value in object">
    {{ value }}
    </li>
  </ul>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    object: {
      name: '菜鸟教程',
      url: 'http://www.runoob.com',
      slogan: '学的不仅是技术，更是梦想！'
    }
  }
})
</script>
```

以提供第二个的参数为键名，第三个参数为索引。

```html
<div id="app">
  <ul>
    <li v-for="(value, key, index) in object">
     {{ index }}. {{ key }} : {{ value }}
    </li>
  </ul>
</div>
```

### v-for迭代整数

```html
<div id="app">
  <ul>
    <li v-for="n in 10">
     {{ n }}
    </li>
  </ul>
</div>
```

## 使用 key 维护列表的状态

当**列表的数据变化**时，默认情况下，vue **会尽可能的复用**已存在的 DOM 元素，从而提升渲染的性能。但这种默认的性能优化策略，会导致有状态的列表无法被正确更新。
为了给 vue 一个提示，以便它能跟踪每个节点的身份，从而在保**证有状态的列表被正确更新**的前提下，**提升渲染的性能**。此时，需要为每项提供一个唯一的 key 属性：  

```html
<li v-for="(user,index) in userlist" :key='user.id'>
    <input type="checkbox">
    姓名：{{user.name}}
</li>
```

使用key的注意事项：

① key 的值只能是字符串或数字类型

② key 的值必须具有唯一性（即：key 的值不能重复）

③ 建议把数据项 id 属性的值作为 key 的值（因为 id 属性的值具有唯一性）

④ **使用 index 的值当作 key 的值没有任何意义**（因为 index 的值不具有唯一性）

⑤ 建议使用 v-for 指令时一定要指定 key 的值（既提升性能、又防止列表状态紊乱）  



# Vue.js 计算属性

## computed

计算属性关键词: **computed**。计算属性在处理一些复杂逻辑时是很有用的。可以看下以下反转字符串的例子：

```html
<div id="app">
  {{ message.split('').reverse().join('') }}
</div>
```

 上述例子变得很复杂，也不容易看懂理解。

接着，让我们看看使用计算属性的实例：

```html
<div id="app">
  <p>原始字符串: {{ message }}</p>
  <p>计算后反转字符串: {{ reversedMessage }}</p>
</div>
 
<script>
var vm = new Vue({
  el: '#app',
  data: {
    message: 'Runoob!'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
</script>
```

上述例子 中声明了一个计算属性 reversedMessage 。提供的函数将用作属性 vm.reversedMessage 的 getter 。

vm.reversedMessage 依赖于 vm.message，在 vm.message 发生改变时，vm.reversedMessage 也会更新。

## computed vs methods

我们可以使用 methods 来替代 computed，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用 methods ，在重新渲染的时候，函数总会重新调用执行。

```html
<div id="app">
  <p>原始字符串: {{ message }}</p>
  <p>计算后反转字符串: {{ reversedMessage }}</p>
  <p>使用方法后反转字符串: {{ reversedMessage2() }}</p>
</div>

<script>
var vm = new Vue({
  el: '#app',
  data: {
    message: 'Runoob!'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  },
  methods: {
    reversedMessage2: function () {
      return this.message.split('').reverse().join('')
    }
  }
})
</script>
```

> 关于getter和setter，[点击查看](https://jiaqicoder.com/2021/07/27/%E5%AF%B9%E8%B1%A1%E5%B1%9E%E6%80%A7%E6%8F%8F%E8%BF%B0%E7%AC%A6%E5%92%8C%E8%AE%BF%E9%97%AE%E5%99%A8%E5%B1%9E%E6%80%A7/#%E8%AE%BF%E9%97%AE%E5%99%A8%E5%B1%9E%E6%80%A7-getter%E5%92%8Csetter)

## computed setter

computed 属性默认只有 getter ，不过在需要时你也可以提供一个 setter ：

```html
<script src="0.vue.js"></script>
<div id="app">
  <p>{{site}}</p>
</div>

<script>
  let vm=new Vue({
    el:'#app',
    data:{
      name:'Google',
      url:'http://www.google.com'
    },
    computed:{
      site:{
        // getter
        get:function () {
          return this.name+' '+this.url;
        },
        // setter
        set:function(newValue){
          [this.name,this.url]=newValue.split(' ');
        }
      }
    }
  })
  // 1s 后页面上的site将会发生变化
  setInterval(()=>{
    vm.site='jiaqi https://www.jiaqicoder.com'
  },1000);
</script>
```

# Vue.js 监听属性

虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。这就是为什么 Vue 通过 `watch` 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

```html
<div id="app">
  <p>{{counter}}</p>
  <p>{{text}}</p>
  <button @click='counter++'>按我</button>
</div>

<script src="0.vue.js"></script>
<script>
  let vm = new Vue({
    el: '#app',
    data: {
      counter: 1,
      text:''
    },
    watch:{
      counter:function(newValue,oldValue){
        this.text=`counter从${oldValue}变为${newValue}`;
      }
    }
  })
</script>
```

![动1](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210728001253.gif)

以下实例进行**华氏度**与**摄氏度**之间的换算：

```html
<div id="app">
  华氏度: <input type="text" @change='fahrenheit=$event.target.value' :value='fahrenheit'>
  摄氏度：<input type="text" @change='celsius=$event.target.value' :value='celsius'>
</div>

<script src="0.vue.js"></script>
<script>
  let vm = new Vue({
    el: '#app',
    data: {
      fahrenheit: '',
      celsius: ''
    },
    watch: {
      fahrenheit: function (value) {
        this.celsius = ((value - 32) / 1.8).toFixed(2);
      },
      celsius: function (value) {
        this.fahrenheit = (value * 1.8 + 32).toFixed(2);
      }
    }
  })
</script>
```

![动2](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210728004031.gif)

# Vue.js 样式绑定

## Vue.js class

class 与 style 是 HTML 元素的属性，用于设置元素的样式，我们可以用 v-bind 来设置样式属性。

Vue.js v-bind 在处理 class 和 style 时， 专门增强了它。表达式的结果类型除了字符串之外，还可以是对象或数组。

## class属性绑定

我们可以为 v-bind:class 设置一个对象，从而动态的切换 class:

```html
<div v-bind:class="{ 'active': isActive }"></div>
```

如果isActive的值为true，则上面的代码相当于：

```html
<div class="active"></div>
```

我们也可以直接绑定一个对象

```html
<div id="app">
  <div v-bind:class="classObject"></div>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    classObject: {
      active: true,
      'text-danger': true
    }
  }
})
</script>
```

此外，我们也可以在这里绑定返回对象的计算属性。

```html
<div id="app">
  <div v-bind:class="classObject"></div>
</div>
<script>

new Vue({
  el: '#app',
  data: {
    isActive: true,
    error: {
      value: true,
      type: 'fatal'
    }
  },
  computed: {
    classObject: function () {
      return {
  base: true,
        active: this.isActive && !this.error.value,
        'text-danger': this.error.value && this.error.type === 'fatal',
      }
    }
  }
})
</script>
```

### 数组语法

我们可以把一个数组传给 **v-bind:class** ，实例如下：

```html
<div id="app">
	<div v-bind:class="[activeClass, errorClass]"></div>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    activeClass: 'active',
    errorClass: 'text-danger'
  }
})
</script>
```

我们还可以使用三元表达式来切换列表中的 class ：errorClass 是始终存在的，isActive 为 true 时添加 activeClass 类：

```html
<div v-bind:class="[errorClass ,isActive ? activeClass : '']"></div>
```

## Vue.js style 内联样式

我们可以在 **v-bind:style** 直接设置样式：

```html
<div id="app">
    <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">菜鸟教程</div>
</div>
```

也可以直接绑定到一个样式对象，让模板更清晰：

```html
<div id="app">
  <div v-bind:style="styleObject">菜鸟教程</div>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    styleObject: {
      color: 'green',
      fontSize: '30px'
    }
  }
})
</script>
```

v-bind:style 可以使用数组将多个样式对象应用到一个元素上：

```html
<div id="app">
  <div v-bind:style="[baseStyles, overridingStyles]">菜鸟教程</div>
</div>
```

# Vue.js 事件处理器

## v-on

事件监听可以使用 v-on 指令：

```html
<div id="app">
  <button v-on:click="counter += 1">增加 1</button>
  <p>这个按钮被点击了 {{ counter }} 次。</p>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    counter: 0
  }
})
</script>
```

v-on 可以接收一个定义的方法来调用。

```html
<div id="app">
   <!-- `greet` 是在下面定义的方法名 -->
  <button v-on:click="greet">Greet</button>
</div>
 
<script>
var app = new Vue({
  el: '#app',
  data: {
    name: 'Vue.js'
  },
  // 在 `methods` 对象中定义方法
  methods: {
    greet: function (event) {
      // `this` 在方法里指当前 Vue 实例
      alert('Hello ' + this.name + '!')
      // `event` 是原生 DOM 事件
      if (event) {
          alert(event.target.tagName)
      }
    }
  }
})
// 也可以用 JavaScript 直接调用方法
app.greet() // -> 'Hello Vue.js!'
</script>
```

除了直接绑定到一个方法，也可以用内联 JavaScript 语句：

```html
<div id="app">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
 
<script>
new Vue({
  el: '#app',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
</script>
```

## 事件对象

在原生的 DOM 事件绑定中，可以在事件处理函数的形参处，接收事件对象 event。同理，在 `v-on` 指令（简写为 @ ）所绑定的事件处理函数中，同样可以接收到事件对象 event，示例代码如下：  

```html
<div id="app">
    <h3>count的值为{{count}}</h3>
    <button v-on:click='addCount'>++</button>
</div>

<script>
    new Vue({
        el: "#app",
        data: {
            count:0,
        },
        methods: {
            addCount(e){
                this.count++;
                e.target.style.backgroundColor='blue';
            }
        },
    })
</script>
```

## 绑定事件并传参

在使用 v-on 指令绑定事件时，可以使用 ( ) 进行传参。**$event 是 vue 提供的特殊变量，用来表示原生的事件参数对象 event。$event 可以解决事件参数对象 event被覆盖的问题。**示例用法如下：  

```html
<div id="app">
    <h3>count的值为{{count}}</h3>
    <button v-on:click='addCount(2,$event)'>+2</button>
</div>

<script>
    new Vue({
        el: "#app",
        data: {
            count: 0,
        },
        methods: {
            addCount(step,e) {
                this.count+=step;
                e.target.style.backgroundColor = 'blue';
            }
        },
    })
</script>
```



## 事件修饰符

Vue.js 为 v-on 提供了事件修饰符来处理 DOM 事件细节，如：event.preventDefault() 或 event.stopPropagation()。

Vue.js 通过由点 **.** 表示的指令后缀来调用修饰符。

- `.stop` - 阻止冒泡
- `.prevent` - 阻止默认事件
- `.capture` - 阻止捕获
- `.self` - 只监听触发该元素的事件
- `.once` - 只触发一次
- `.left` - 左键事件
- `.right` - 右键事件
- `.middle` - 中间滚轮事件

```html
<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联  -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身（而不是子元素）触发时触发回调 -->
<div v-on:click.self="doThat">...</div>

<!-- click 事件只能点击一次，2.1.4版本新增 -->
<a v-on:click.once="doThis"></a>
```

## 按键修饰符

Vue 允许为 v-on 在监听键盘事件时添加按键修饰符：

```html
<!-- 只有在 keyCode 是 13 时调用 vm.submit() -->
<input v-on:keyup.13="submit">
```

记住所有的 keyCode 比较困难，所以 Vue 为最常用的按键提供了别名：

```html
<!-- 同上 -->
<input v-on:keyup.enter="submit">
<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

```html
<p><!-- Alt + C -->
<input @keyup.alt.67="clear">
<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```

# Vue.js 表单

你可以用 v-model 指令在表单控件元素上创建双向数据绑定。

![img](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210728105745.png)

## 控件

### 输入框

实例中演示了 input 和 textarea 元素中使用 v-model 实现双向数据绑定：

```html
<div id="app">
  <p>input 元素：</p>
  <input v-model="message" placeholder="编辑我……">
  <p>消息是: {{ message }}</p>
    
  <p>textarea 元素：</p>
  <p style="white-space: pre">{{ message2 }}</p>
  <textarea v-model="message2" placeholder="多行文本输入……"></textarea>
</div>
 
<script>
new Vue({
  el: '#app',
  data: {
    message: 'Runoob',
    message2: '菜鸟教程\r\nhttp://www.runoob.com'
  }
})
</script>
```

## 复选框

复选框如果是一个为逻辑值，如果是多个则绑定到同一个数组：

```html
<div id="app">
  <p>单个复选框：</p>
  <input type="checkbox" id="checkbox" v-model="checked" value="hhhhhh">
  <label for="checkbox">{{ checked }}</label>
    
  <p>多个复选框：</p>
  <input type="checkbox" id="runoob" value="Rob" v-model="checkedNames">
  <label for="runoob">Runoob</label>
  <input type="checkbox" id="google" value="Google" v-model="checkedNames">
  <label for="google">Google</label>
  <input type="checkbox" id="taobao" value="Taobao" v-model="checkedNames">
  <label for="taobao">taobao</label>
  <br>
  <span>选择的值为: {{ checkedNames }}</span>
</div>
  <script src="0.vue.js"></script>
<script>
new Vue({
  el: '#app',
  data: {
    checked : false,
    checkedNames: []
  }
})
</script>
```

![image-20210728114748599](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210728114757.png)

可见，如果复选框只有一个，通过v-model绑定的checked的值为true或者false，而复选框有多个时，v-model绑定的checkedNames的值为input的value，且checkedNames是一个数组。

### select列表

注意：此时的v-model 添加在select上面，而非option上。

```html
<div id="app">
  <select name="fruit" v-model='selected'>
    <option value="">请选择一个网站</option>
    <option value="www.baidu.com">百度</option>
    <option value="www.google.com">谷歌</option>
  </select>
  <p>选择的网站为 {{selected}}</p>
</div>
<script src="0.vue.js"></script>
<script>
  new Vue({
    el: '#app',
    data: {
      selected:''
    }
  })
</script>
```

![image-20210728115636548](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210728115637.png)

## 修饰符

### .lazy

在默认情况下， v-model 在 input 事件中同步输入框的值与数据，但你可以添加一个修饰符 lazy ，从而转变为在 change 事件中同步：

```html
<!-- 在 "change" 而不是 "input" 事件中更新 -->
<input v-model.lazy="msg" >
```

### .number

如果想自动将用户的输入值转为 Number 类型（如果原值的转换结果为 NaN 则返回原值），可以添加一个修饰符 number 给 v-model 来处理输入值：

```html
<input v-model.number="age" type="number">
```

这通常很有用，因为在 type="number" 时 HTML 中输入的值也总是会返回字符串类型。

### .trim

如果要自动过滤用户输入的首尾空格，可以添加 trim 修饰符到 v-model 上过滤输入：

```html
<input v-model.trim="msg">
```

# Vue.js 组件

组件（Component）是 Vue.js 最强大的功能之一。

组件可以扩展 HTML 元素，封装可重用的代码。

组件系统让我们可以用独立可复用的小组件来构建大型应用，几乎任意类型的应用的界面都可以抽象为一个组件树：

![img](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210728120331.png)

注册一个全局组件语法格式如下：

```js
Vue.component(tagName, options)
```

tagName 为组件名，options 为配置选项。注册后，我们可以使用以下方式来调用组件：

```html
<tagName></tagName>
```

## 全局组件

所有实例都能用全局组件。

```html
<div id="app">
  <hello></hello>
</div>
<script>
  // 组件注册
  Vue.component('hello', {
    template: '<h1>自定义组件</h1>'
  })
  new Vue({
    el: '#app',

  })
</script>
```

局部组件

我们也可以在实例选项中注册局部组件，这样组件只能在这个实例中使用：

```html
<div id="app">
  <hello></hello>
</div>
<script>
  // 组件注册
  Vue.component('hello', {
    template: '<h1>自定义组件</h1>'
  })
  new Vue({
    el: '#app',
    components:{
      // 只能在父模板中使用
      'hello':{
        template:'<h1>笑死我么</h1>'
      }
    }
  })
</script>
```

## Prop

prop 是子组件用来接受父组件传递过来的数据的一个自定义属性。**父组件的数据需要通过 props 把数据传给子组件，子组件需要显式地用 props 选项声明 "prop"**。

```html
<div id="app">
  <child message='hello'></child>
</div>
<script>
  // 组件注册
  Vue.component('child',{
    // 声明props
    props:['message'],
    template:'<span>{{message}}</span>'
  })
  new Vue({
    el:'#app'
  })
</script>
```

## 动态prop

类似于用 v-bind 绑定 HTML 特性到一个表达式，也可以用 v-bind 动态绑定 props 的值到父组件的数据中。每当父组件的数据变化时，该变化也会传导给子组件。

注意: prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是不会反过来。

```html
<div id="app">
  <child v-bind:message='parentMsg'></child>
</div>
<script>
  // 组件注册
  Vue.component('child',{
    // 声明props
    props:['message'],
    template:'<span>{{message}}</span>'
  })
  // 创建根实例
  new Vue({
    el:'#app',
    data:{
      parentMsg:'父组件内容aa'
    }
  })
</script>
```

以下实例中使用 v-bind 指令将 todo 传到每一个重复的组件中：

```html
<div id="app">
  <ol>
    <todo-item v-for='(item) in sites' v-bind:todo=item.text></todo-item>
  </ol>
</div>
<script>
  // 组件注册
  Vue.component('todo-item',{
    props:['todo'],
    template:'<li>{{todo}}</li>'
  })
  // 创建根实例
  new Vue({
    el:'#app',
    data:{
      sites:[
        {text:'jiaqi'},
        {text:'google'},
        {text:'taobao'}
      ]
    }
    
  })
</script>
```

## Prop 验证

组件可以为 props 指定验证要求。当 prop 验证失败的时候，(开发环境构建版本的) Vue 将会产生一个控制台的警告。

为了定制 prop 的验证方式，你可以为 props 中的值提供一个带有验证需求的对象，而不是一个字符串数组。例如：

```js
Vue.component('my-component',{
  props:{
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA:Number,
    // 多个可能的类型
    propB:[String,Number],
    // 必填的字符串
    propC:{
      type:String,
      required:true
    },
    // 带有默认值的数字
    propD:{
      type:Number,
      default:100
    },
    // 带有默认值的对象
    // 对象或数组的默认值必须从一个工厂函数中获取
    propE:{
      type:Object,
      default:function(){
        return {hello:'hello'}
      }
    },
    // 自定义验证函数
    propF:{
      validator:function(value){
        // 这个值必须从下面的字符串中选择一个
        return ['success','warning','danger'].indexOf(value)!==-1;
      }
    }
  }
})
```

type 可以是下面原生构造器：

- `String`
- `Number`
- `Boolean`
- `Array`
- `Object`
- `Date`
- `Function`
- `Symbol`

type 也可以是一个自定义构造器，使用 instanceof 检测。

# Vue组件- 自定义事件

父组件是使用 props 传递数据给子组件，但如果子组件要把数据传递回去，就需要使用自定义事件！

我们可以使用 v-on 绑定自定义事件, 每个 Vue 实例都实现了事件接口(Events interface)，即：

- 使用 `$on(eventName)` 监听事件
- 使用 `$emit(eventName)` 触发事件

另外，父组件可以在使用子组件的地方直接用 v-on 来监听子组件触发的事件。

```html
<div id="app">
    <div id="counter-event-example">
      <p>{{ total }}</p>
      <button-counter v-on:increment="incrementTotal"></button-counter>
      <button-counter v-on:increment="incrementTotal"></button-counter>
    </div>
</div>
 
<script>
Vue.component('button-counter', {
  template: '<button v-on:click="incrementHandler">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  },
  methods: {
    incrementHandler: function () {
      this.counter += 1
      this.$emit('increment')
    }
  },
})
new Vue({
  el: '#counter-event-example',
  data: {
    total: 0
  },
  methods: {
    incrementTotal: function () {
      this.total += 1
    }
  }
})
</script>
```

如果你想在某个组件的根元素上监听一个原生事件。可以使用 .native 修饰 v-on 。例如：

```html
<my-component v-on:click.native="doTheThing"></my-component>
```

```html
<div id="app">
  {{total}}
  <my-component @click.native='dosome'></my-component>
</div>
<script>
  Vue.component('my-component',{
    template:`<button>按我<botton>`
  })
  // 创建根实例
  new Vue({
    el: '#app',
    data:{
      total:''
    },
    methods: {
      dosome(){
        this.total++;
      }
    },
  })
</script>
```

## data 必须是一个函数

上面例子中，可以看到 button-counter 组件中的 data 不是一个对象，而是一个函数：

```js
data: function () {
  return {
    count: 0
  }
}
```

这样的好处就是每个实例可以维护一份被返回对象的独立的拷贝，如果 data 是一个对象则会影响到其他实例，如下所示：

```html
<div id="components-demo3" class="demo">
    <button-counter2></button-counter2>
    <button-counter2></button-counter2>
    <button-counter2></button-counter2>
</div>
 
<script>
var buttonCounter2Data = {
  count: 0
}
Vue.component('button-counter2', {
    /*
    data: function () {
        // data 选项是一个函数，组件不相互影响
        return {
            count: 0
        }
    },
    */
    data: function () {
        // data 选项是一个对象，会影响到其他实例
        return buttonCounter2Data
    },
    template: '<button v-on:click="count++">点击了 {{ count }} 次。</button>'
})
new Vue({ el: '#components-demo3' })
</script>
```

## 自定义组件的v-model

**组件上的 v-model 默认会利用名为 value 的 prop 和名为 input 的事件。**

```html
<input v-model="parentData">
```

等价于：

```html
<input 
    :value="parentData"
    @input="parentData = $event.target.value"
>
```

以下实例自定义组件 runoob-input，父组件的 num 的初始值是 100，更改子组件的值能实时更新父组件的 num：

```html
<div id="app">
    <runoob-input v-model="num"></runoob-input>
    <p>输入的数字为:{{num}}</p>
</div>
<script>
Vue.component('runoob-input', {
    template: `
    <p>   <!-- 包含了名为 input 的事件 -->
      <input
       ref="input"
       :value="value" 
       @input="$emit('input', $event.target.value)"
      >
    </p>
    `,
    props: ['value'], // 名为 value 的 prop
})
   
new Vue({
    el: '#app',
    data: {
        num: 100,
    }
})
</script>
```

由于 v-model 默认传的是 value，不是 checked，所以对于复选框或者单选框的组件时，我们需要使用 model 选项，model 选项可以指定当前的事件类型和传入的 props。

```html
<div id="app">
    <base-checkbox v-model="lovingVue"></base-checkbox> 
     <div v-show="lovingVue"> 
        如果选择框打勾我就会显示。 
    </div>
</div> 
<script>
// 注册
Vue.component('base-checkbox', {
 
  model: {
    prop: 'checked',
    event: 'change'  // onchange 事件
  },
  props: {
    checked: Boolean
  },
   
  template: `
    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >
  `
})
// 创建根实例
new Vue({
  el: '#app',
  data: {
    lovingVue: true
  }
})
</script>
```

# Vue.js 自定义指令

除了默认设置的核心指令( v-model 和 v-show ), Vue 也允许注册自定义指令。

下面我们注册一个全局指令 v-focus, 该指令的功能是在页面加载时，元素获得焦点：

```html
<div id="app">
    <p>页面载入时，input 元素自动获取焦点：</p>
    <input v-focus>
</div>
 
<script>
// 注册一个全局自定义指令 v-focus
Vue.directive('focus', {
  // 当绑定元素插入到 DOM 中。
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
// 创建根实例
new Vue({
  el: '#app'
})
</script>
```

我们也可以在实例使用 directives 选项来注册局部指令，这样指令只能在这个实例中使用：

```html
<div id="app">
  <p>页面载入时，input 元素自动获取焦点：</p>
  <input v-focus>
</div>
 
<script>
// 创建根实例
new Vue({
  el: '#app',
  directives: {
    // 注册一个局部的自定义指令 v-focus
    focus: {
      // 指令的定义
      inserted: function (el) {
        // 聚焦元素
        el.focus()
      }
    }
  }
})
</script>
```

## 钩子

### 钩子函数

指令定义函数提供了几个钩子函数（可选）：

- `bind`: 只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个在绑定时执行一次的初始化动作。
- `inserted`: 被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于 document 中）。
- `update`: 被绑定元素所在的模板更新时调用，而不论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新（详细的钩子函数参数见下）。
- `componentUpdated`: 被绑定元素所在模板完成一次更新周期时调用。
- `unbind`: 只调用一次， 指令与元素解绑时调用。

### 钩子函数的参数

钩子函数的参数有：

- **el**: 指令所绑定的元素，可以用来直接操作 DOM 。
- binding: 一个对象，包含以下属性：
  - **name**: 指令名，不包括 `v-` 前缀。
  - **value**: 指令的绑定值， 例如： `v-my-directive="1 + 1"`, value 的值是 `2`。
  - **oldValue**: 指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
  - **expression**: 绑定值的表达式或变量名。 例如 `v-my-directive="1 + 1"` ， expression 的值是 `"1 + 1"`。
  - **arg**: 传给指令的参数。例如 `v-my-directive:foo`， arg 的值是 `"foo"`。
  - **modifiers**: 一个包含修饰符的对象。 例如： `v-my-directive.foo.bar`, 修饰符对象 modifiers 的值是 `{ foo: true, bar: true }`。
- **vnode**: Vue 编译生成的虚拟节点。
- **oldVnode**: 上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用。

```html
<div id="app"  v-runoob:hello.a.b="message">
</div>
 
<script>
Vue.directive('runoob', {
  bind: function (el, binding, vnode) {
    var s = JSON.stringify
    el.innerHTML =
      'name: '       + s(binding.name) + '<br>' +
      'value: '      + s(binding.value) + '<br>' +
      'expression: ' + s(binding.expression) + '<br>' +
      'argument: '   + s(binding.arg) + '<br>' +
      'modifiers: '  + s(binding.modifiers) + '<br>' +
      'vnode keys: ' + Object.keys(vnode).join(', ')
  }
})
new Vue({
  el: '#app',
  data: {
    message: '菜鸟教程!'
  }
})
</script>
```

![image-20210730162439990](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210730162448.png)

有时候我们不需要其他钩子函数，我们可以简写函数，如下格式：

```js
Vue.directive('runoob', function (el, binding) {
  // 设置指令的背景颜色
  el.style.backgroundColor = binding.value.color
})
```

指令函数可接受所有合法的 JavaScript 表达式，以下实例传入了 JavaScript 对象：

```html
<div id="app">
  <div v-jiaqi='{color:"pink",text:"666"}'></div>
</div>
<script>
  Vue.directive('jiaqi',function(el,binding){
    el.style.color=binding.value.color;
    el.innerHTML=binding.value.text;
  })
  new Vue({
    el: '#app',
  })
</script>
```

# Vue数组处理

## 变异方法

1.变异方法和替换数组有什么区别

- 变异的方法能够实现数据更新视图自动更新
- 替换数组不会修改原始数据，数据改变视图不一定更新

2.变异方法和替换数组有哪些

- 变异方法：push  pop shift unshift  splice  sort  reverse

- 替换数组：filter  concat  slice

```html
<div id="app">
  <div><input type="text" v-model='fname'></div>
  <button @click='add'>添加</button> 
  <button @click='del'>删除</button>
  <button @click='change'>替换</button>
  <ol>
    <li v-for='item in list'>{{item}}</li>
  </ol>
</div>
<script>

  new Vue({
    el: '#app',
    data() {
      return {
        list:['apple','orange','banana'],
        fname:''
      }
    },
    methods: {
      add:function(){
        this.list.push(this.fname);
      },
      del(){
        this.list.pop(this.fname);
      },
      change(){
        this.list=this.list.slice(0,2)
      }
    },
  })
</script>
```

## 数组响应式变化

- Vue.set(vm.items, indexOfItem, newValue)  
- vm.$set(vm.items, indexOfItem, newValue)  
- ① 参数一表示要处理的数组名称
  ② 参数二表示要处理的数组的索引
  ③ 参数三表示要处理的数组的值  

```js
let vm = new Vue({
  el: '#app',
  data() {
    return {
      list: ['apple', 'orange', 'banana'],
    }
  },
})
Vue.set(vm.list,0,'lemon'); // 数据和视图都发生了变化
vm.$set(vm.list,1,'banana');// 数据和视图都发生了变化
//  vm.list[1]='lemon';  数据被修改了，但是视图没有被修改
```

## 对象响应式变化

- Vue.set(vm.items, key, newValue)  
- vm.$set(vm.items, key, newValue)  
- ① 参数一表示要处理的对象名称
  ② 参数二表示要处理的对象的属性名
  ③ 参数三表示要处理的对象的值  

```html
<div id="app">
  <div>
    <p>{{info.name}}</p>
    <p>{{info.age}}</p>
    <p>{{info.gender}}</p>
  </div>
</div>
<script>

  let vm = new Vue({
    el: '#app',
    data() {
      return {
        info: {
          name: 'lisi',
          age: 23,
          gender: 'male'
        }
      }
    },
  })

  // vm.info.gender='female'  不会修改视图层，只会修改数据
  vm.$set(vm.info, 'gender', 'non-binary') // 而且此时再用vm.info.gender来修改也是响应式的
</script>
```

- vm.$delete(vm.items, key) ：删除对象中的某个键值对


# 进入&离开过渡

## 单元素/组件的过渡

Vue 提供了 `transition` 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡

- 条件渲染 (使用 `v-if`)
- 条件展示 (使用 `v-show`)
- 动态组件
- 组件根节点

```html
<div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>
```

```js
new Vue({
  el: '#demo',
  data: {
    show: true
  }
})
```

```css
/* 整个进入和离开的过程 */
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
/*或者或写成这样
p{
   transition: opacity .5s;
}
*/
/* 进入的起点和离开的终点 */
.fade-enter, .fade-leave-to  {
  opacity: 0;
}
```

### 过渡的类名

在进入/离开的过渡中，会有 6 个 class 切换。

1. `v-enter`：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。
2. `v-enter-active`：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
3. `v-enter-to`：在元素被插入之后下一帧生效 (与此同时 `v-enter` 被移除)，在过渡/动画完成之后移除。
4. `v-leave`：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。
5. `v-leave-active`：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
6. `v-leave-to`：在离开过渡被触发之后下一帧生效 (与此同时 `v-leave` 被删除)，在过渡/动画完成之后移除。

![Transition Diagram](https://cn.vuejs.org/images/transition.png)

对于这些在过渡中切换的类名来说，如果你使用一个没有名字的 `<transition>`，则 `v-` 是这些类名的默认前缀。如果你使用了 `<transition name="my-transition">`，那么 `v-enter` 会替换为 `my-transition-enter`。

`v-enter-active` 和 `v-leave-active` 可以控制进入/离开过渡的不同的缓和曲线，在下面章节会有个示例说明。



### 自定义过渡的类名

我们可以通过以下 attribute 来自定义过渡类名：

- `enter-class`
- `enter-active-class`
- `enter-to-class` 
- `leave-class`
- `leave-active-class`
- `leave-to-class` 

他们的优先级高于普通的类名，这对于 Vue 的过渡系统和其他第三方 CSS 动画库，如 [Animate.css](https://daneden.github.io/animate.css/) 结合使用十分有用。

```html
<link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">

<div id="example-3">
  <button @click="show = !show">
    Toggle render
  </button>
  <transition
    name="custom-classes-transition"
    enter-active-class="animated tada"
    leave-active-class="animated bounceOutRight"
  >
    <p v-if="show">hello</p>
  </transition>
</div>
```



## 初始渲染过渡

可以通过 `appear` attribute 设置节点在初始渲染的过渡

```html
<transition appear>
  <!-- ... -->
</transition>
```

## 多个元素的过渡

于原生标签可以使用 `v-if`/`v-else`。最常见的多标签过渡是一个列表和描述这个列表为空消息的元素：

```HTML
<transition>
  <table v-if="items.length > 0">
    <!-- ... -->
  </table>
  <p v-else>Sorry, no items found.</p>
</transition>
```

可以这样使用，但是有一点需要注意：

当有**相同标签名**的元素切换时，需要通过 `key` attribute 设置唯一的值来标记以让 Vue 区分它们，否则 Vue 为了效率只会替换相同标签内部的内容。即使在技术上没有必要，**给在 `<transition>` 组件中的多个元素设置 key 是一个更好的实践。**

```
<transition>
  <button v-if="isEditing" key="save">
    Save
  </button>
  <button v-else key="edit">
    Edit
  </button>
</transition>
```

在一些场景中，也可以通过给同一个元素的 `key` attribute 设置不同的状态来代替 `v-if` 和 `v-else`，上面的例子可以重写为：

```HTML
<transition>
  <button v-bind:key="isEditing">
    {{ isEditing ? 'Save' : 'Edit' }}
  </button>
</transition>
```

### 过渡模式

`<transition>` 的默认行为 - 进入和离开同时发生。

同时生效的进入和离开的过渡不能满足所有要求，所以 Vue 提供了**过渡模式**

- `in-out`：新元素先进行过渡，完成之后当前元素过渡离开。
- `out-in`：当前元素先进行过渡，完成之后新元素过渡进入。

用 `out-in` 重写之前的开关按钮过渡：

```HTML
<transition name="fade" mode="out-in">
  <!-- ... the buttons ... -->
</transition>
```

## 多个组件的过渡

多个组件的过渡简单很多 - 我们不需要使用 `key` attribute。相反，我们只需要使用[动态组件](https://cn.vuejs.org/v2/guide/components.html#动态组件)：

```html
<transition name="component-fade" mode="out-in">
  <component v-bind:is="view"></component>
</transition>
```

```js
new Vue({
  el: '#transition-components-demo',
  data: {
    view: 'v-a'
  },
  components: {
    'v-a': {
      template: '<div>Component A</div>'
    },
    'v-b': {
      template: '<div>Component B</div>'
    }
  }
})
```

```css
.component-fade-enter-active, .component-fade-leave-active {
  transition: opacity .3s ease;
}
.component-fade-enter, .component-fade-leave-to
/* .component-fade-leave-active for below version 2.1.8 */ {
  opacity: 0;
}
```

## 列表过渡

目前为止，关于过渡我们已经讲到：

- 单个节点
- 同一时间渲染多个节点中的一个

那么怎么同时渲染整个列表，比如使用 `v-for`？在这种场景中，使用 `<transition-group>` 组件。在我们深入例子之前，先了解关于这个组件的几个特点：

- 不同于 `<transition>`，它会以一个真实元素呈现：默认为一个 `<span>`。你也可以通过 `tag` attribute 更换为其他元素。
- [过渡模式](https://cn.vuejs.org/v2/guide/transitions.html#过渡模式)不可用，因为我们不再相互切换特有的元素。
- 内部元素**总是需要**提供唯一的 `key` attribute 值。
- CSS 过渡的类将会应用在内部的元素中，而不是这个组/容器本身。



这个例子有个问题，当添加和移除元素的时候，周围的元素会瞬间移动到他们的新布局的位置，而不是平滑的过渡，我们下面会解决这个问题。现在让我们由一个简单的例子深入，进入和离开的过渡使用之前一样的 CSS 类名。

```HTML
<div id="list-demo" class="demo">
  <button v-on:click="add">Add</button>
  <button v-on:click="remove">Remove</button>
  <transition-group name="list" tag="p">
    <span v-for="item in items" v-bind:key="item" class="list-item">
      {{ item }}
    </span>
  </transition-group>
</div>
```

```js
new Vue({
  el: '#list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9],
    nextNum: 10
  },
  methods: {
    randomIndex: function () {
      return Math.floor(Math.random() * this.items.length)
    },
    add: function () {
      this.items.splice(this.randomIndex(), 0, this.nextNum++)
    },
    remove: function () {
      this.items.splice(this.randomIndex(), 1)
    },
  }
})
```

```css
.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to
/* .list-leave-active for below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(30px);
}
```

这个例子有个问题，当添加和移除元素的时候，周围的元素会瞬间移动到他们的新布局的位置，而不是平滑的过渡，我们下面会解决这个问题。

#### 列表过渡排序

`<transition-group>` 组件还有一个特殊之处。不仅可以进入和离开动画，还可以改变定位。要使用这个新功能只需了解新增的 **`v-move` class**，它会在元素的改变定位的过程中应用。像之前的类名一样，可以通过 `name` attribute 来自定义前缀，也可以通过 `move-class` attribute 手动设置。

`v-move` 对于设置过渡的切换时机和过渡曲线非常有用，你会看到如下的例子：

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/lodash.min.js"></script>

<div id="flip-list-demo" class="demo">
  <button v-on:click="shuffle">Shuffle</button>
  <transition-group name="flip-list" tag="ul">
    <li v-for="item in items" v-bind:key="item">
      {{ item }}
    </li>
  </transition-group>
</div>
```

```js
new Vue({
  el: '#flip-list-demo',
  data: {
    items: [1,2,3,4,5,6,7,8,9]
  },
  methods: {
    shuffle: function () {
      this.items = _.shuffle(this.items)
    }
  }
})
```

```css
.flip-list-move {
  transition: transform 1s;
}
```

