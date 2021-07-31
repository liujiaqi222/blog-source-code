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

![image-20210727192913338](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210727192921.png)

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

![image-20210727193358942](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210727193400.png)

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

![image-20210727232644828](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210727232652.png)

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

![动1](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210728001253.gif)

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

![动2](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210728004031.gif)

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

![img](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210728105745.png)

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

![image-20210728114748599](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210728114757.png)

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

![image-20210728115636548](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210728115637.png)

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

![img](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210728120331.png)

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

![image-20210730162439990](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210730162448.png)

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



