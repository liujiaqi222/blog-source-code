---
title: JavaScipt 面试题
date: 2021-09-20 23:26:23
tags: [面试题, JS,JavaScript]
---

# JavaScript

## 变量类型

### 前置知识

- 值类型 vs 引用类型

  ```js
  // 值类型
  let a = 100;
  let b = a;
  a = 200;
  console.log(b); //100
  ```

  ```js
  // 引用类型
  let a = { age: 20 };
  let b = a;
  b.age = 21;
  console.log(a.age); //21
  ```

  ![image-20210906141809809](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202109061418905.png)

  ----

  

  ![image-20210906142017216](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202109061420355.png)

  ```js
  // 常见值类型
  let  a // undefined
  const s = 'abc' 
  const n = 100
  const b = true
  const m = Symbol('m');
  ```

  ```js
  // 常见引用类型
  const obj = { x: 100 };
  const arr = ['a', 'b', 'c'];
  const n = null // 特殊引用类型 指针指向空地址
  
  // 特殊引用类型 因为不用于存储数据，所以没有“拷贝、复制函数”一说
  function fn(){}
  ```

- 逻辑运算

  - truthy变量：!!a === true 的变量

    ```js
    let a = 100;
    !a; //false
    !!a; //true
    ```

  - falsy变量：!!a === false 的变量

    在 JavaScript 中只有 8 **个** **falsy** 值。

    ```js
    // 以下是falsely变量 除此之外都是truely变量
    !!0 === false
    !!-0 ===false
    !!NaN === false
    !!0n ===false (bigInt)
    !!'' === false
    !!null === false
    !!undefined === false
    !!false === false
    ```

  1. if 语句 判断的是否为truthy或者falsy。

  2. 与或非同样判断的是 truthy或者falsy

     ```js
     console.log(10&&0); //0
     console.log(''||'abc') //'abc'
     console.log(!window.abc) //true
     ```

     

### 题目

1. typeof 能判断哪些类型

   ```js
   // typeof 可以判断所有值类型
   let a;                     typeof a; //undefined
   let str = 'abc';           typeof str; // string
   const n = 100;             typeof n; // number
   const b = true;            typeof b; // boolean
   const s = Symbol('s');     typeof s; // symbol
   let big = BigInt(10e15);   typeof big; // bigint
   ```

   ```js
   // 能判断引用类型
   typeof console.log         // function
   typeof function(){}        // function
   
   // 能识别引用类型
   typeof null;               // object
   typeof [1, 2, 3];          // object
   typeof {x:100};            // object
   ```

   

2. 何时使用 === 何时使用 ==

   ```js
   // 除了 == null以外，其他一律用 ===， 例如
   const obj = {x:100}
   if(obj.a == null) {}
   // 相当于
   if(obj.a === null || obj===undefined){}
   ```

3. 值类型和引用类型的区别

   值类型的值直接存储在栈中，而引用类型在栈中存储的是地址，地址指向其在堆中对应的值。

4. 手写深拷贝

   ```js
   const obj = {
       age: 20,
       name: '张三',
       address: {
           city: 'beijing'
       },
       arr: [1, 2, 3, [4, 5, 6]]
   }
   
   /**
    * 深拷贝
    * @param {Object} obj  要拷贝的对象
    */
   
   function deepClone(obj) {
       if (typeof obj !== 'object' || obj === undefined) {
           // 如果obj不是object类型或者为undefined
   
           return obj;
       }
       //  初始化返回结果
       let result;
       if (obj instanceof Array) {
           result = [];
       } else {
           result = {};
       }
       for(let key in obj){
           // 保证key 不为原型的属性
           if(obj.hasOwnProperty(key)){
               // 递归 如果是数组，此时的key为索引
               result[key] = deepClone(obj[key]);
           }
       }
       return result;
   }
   const obj2 = deepClone(obj);
   ```

   

## 类

### 前置知识

1. class

   - constructor
   - 属性
   - 方法

   ```js
   class Student{
       constructor(name,number){
           this.name = name;
           this.number = number;
       }
       sayHi(){
           console.log(`姓名${this.name}  学号：${this.number}`);
       }
   }
   // 实例化对象
   const Alex = new Student('Alex','001');
   console.log(Alex.name);
   console.log(Alex.number);
   Alex.sayHi();
   ```

2. 继承

   - extends
   - super
     - 执行 `super.method(...)` 来调用一个父类方法。
     - 执行 `super(...)` 来调用一个父类 constructor（只能在我们的 constructor 中）。

   ```js
   // 父类
   class People{
       constructor(name){
           this.name = name;
       }
       eat(){
           console.log(`${this.name} can eat`);
       }
   } 
   
   // 子类
   class Student extends People{
       constructor(name,number){
           super(name);
           this.number = number;
       }
       sayHi(){
           super.eat();
           console.log(`姓名${this.name}  学号：${this.number}`);
       }
   }
   
   const Alex = new Student('Alex','001');
   console.log(Alex.name);
   console.log(Alex.number);
   Alex.sayHi();
   ```

   



### 题目

1. 如何准确判断一个变量是否为数组？

   `Array.isArray(xx)` 或者 `xx instanceof Array`

2. 手写一个简易的jQuery，考虑插件和拓展性？

   ```js
   class jQuery{
       constructor(selector){
           this.selector = selector;
           const result = document.querySelectorAll(selector);
           for(let i =0; i<result.length;i++){
               this[i] = result[i];
           }
           this.length = result.length;
       }
       get(index){
           return this[index];
       }
       each(fn){
           for(let i=0;i<this.length;i++){
               fn(this[i])
           }
       }
       on(type,fn){
           return this.each(elem=>{
               elem.addEventListener(type,fn,false);
           })
       }
   }
   
   // 插件
   jQuery.prototype.dialog =function(info){
       alert(info);
   }
   
   // 复写
   class myJQuery extends jQuery{
       constructor(selector){
           super(selector);
       }
       getLenth(){
           console.log(this.length);
       }
   }
   
    new myJQuery('p').getLenth();
   
   ```

   

3. 如何理解class的原型本质

   人们常说 `class` 是一个语法糖，因为我们实际上可以在没有 `class` 的情况下声明相同的内容。

   尽管，它们之间存在着重大差异：

   1. 首先，通过 `class` 创建的函数具有特殊的内部属性标记 `[[IsClassConstructor]]: true`。因此，它与手动创建并不完全相同。

      编程语言会在许多地方检查该属性。例如，与普通函数不同，必须使用 `new` 来调用它：

   2. 类方法不可枚举。 类定义将 `"prototype"` 中的所有方法的 `enumerable` 标志设置为 `false`。

      这很好，因为如果我们对一个对象调用 `for..in` 方法，我们通常不希望 class 方法出现。

   3. 类总是使用 `use strict`。 在类构造中的所有代码都将自动进入严格模式。

   

## 作用域和闭包

### 前置知识



- 自由变量
  - 一个变量在**当前作用域**没有定义，但是被使用了
  - 向上一级作用域，一层一层依次寻找，直至找到为止
  - 如果到全局作用域都没有找到，则报错`xx is not defined`

- 作用域

  - 全局作用域
  - 函数作用域
  - 块级作用域

- 闭包

  - 作用域应用的特殊情况，有两种表现：①函数作为参数传递 ②函数作为返回值被返回

  - 最关键的地方：在函数定义的位置找！

  - **所有的自由变量的查找，是在函数定义的地方，向上级作用域查找，不是在执行的地方。**

    ```js
    // 函数作为返回值
    function create() {
        let a = 100;
        return function () {
            console.log(a);
        }
    }
    
    let fn = create();
    
    let a = 200;
    
    fn(); //100
    ```

    

    ```js
    // 函数作为参数
    function print(fn){
        let a = 200;
        fn();
    }
    
    let a =100;
    
    function fn(){
        console.log(a);
    }
    print(fn); //100
    ```

    

- this

  > this的值是在函数调用时确定的，不是在函数定义时确定的。

  - 作为普通函数

    ```js
    function fn1(){
        console.log(this);
    }
    
    fn1(); // window
    ```

    

  - 使用call apply bind

    ```js
    fn1.call({x:100});  // this指向 {x:100}
    
    const fn2 = fn1.bind({x:200});
    fn2();  //{x:200}
    ```

  - 作为对象方法被调用

    ```js
    const zhangsan = {
        name: '张三',
        sayHi(){
            // this 即当前对象
            console.log(this);
        },
        wait(){
            setTimeout(function(){
                // this指向window
                console.log(this);
            },10);
        },
        waitAbitLonger(){
            setTimeout(() => {
                console.log(this);
            }, 100);
        }
    }
    
    zhangsan.sayHi();
    zhangsan.wait(); //window
    zhangsan.waitAbitLonger(); //指向对象张三
    
    const wait = zhangsan.waitAbitLonger;
    wait(); // window
    ```

  - 在class方法中被调用

    ```js
    class People{
        constructor(name){
            this.name = name;
            this.age = 20;
        }
        sayHi(){
            console.log(this);
        }
    }
    
    const zhangsan = new People('张三');
    zhangsan.sayHi(); // zhangsan 对象
    ```

  - 箭头函数

### 题目

1. `this` 的不同应用场景，如何取值？

2. 手写 `bind` 函数

   ```js
   function fn1(...args){
       console.log('this',this);
       console.log(...args);
       return 'this is fn1'
   }
   
   
   // const fn2 = fn1.bind({x:200},10,20,30);  原生bind
   // const res = fn2();
   // console.log(res);
   
   // 手写bind
   Function.prototype.bind1= function(){
       // 将参数变为数组
       const args = [...arguments];
       // 获取this
       const t = args.shift();
         
       // 此时的this指向调用bind1的函数 比如fn1.bind1() 则bind1()中的this指向fn1
        const self = this;
   
       //返回一个函数
        return function(){
             return self.apply(t,args);
        }
   
   }
   
   let test = fn1.bind1({x:200},10,20,30);
   console.log(test());
   ```

   ```js
   // 手写apply
   function fn(...args) {
       console.log(this);  //(**)
       console.log('I got called with ' + [...args].join(' '));
       return [...args].reduce((total, curr) => total + curr, 0);
   }
   
   
   Function.prototype.applyTest = function (t, args) {
       // 此时的this 指向调用applyTest的函数
       // t为需要被指定的上下文
       t.__proto__.fn = this; 
       // 为什么要在原型链上添加，因为如果不在原型链添加 在(**)中打印this时就多了这个函数
   
       let result = t.fn(...args);
       delete t.__proto__.fn;
       return result
   }
   
   let result = fn.apply({ zs: 100 }, [2, 3, 4]);
   console.log(result);
   
   // 瞎写补充一点
   Function.prototype.applyTest = function (t, args) {
       if (t == undefined) {
           if (args == undefined) {
               // 此时没有上下文 也没有参数 直接调用函数即可
               return this(args);
           } else {
               // 展开参数
               return this(...args);
           }
       }
       t.__proto__.fn = this;
       let result;
       if (args == undefined) {
           result = t.fn(args);
       } else {
           result = t.fn(...args);
       }
       delete t.__proto__.fn;
       return result;
   }
   ```

   

3. 闭包在实际开发中的应用场景，举例说明

   ```js
   // 隐藏闭包中的数据 缓存数据
   // 例子1
   function createCache(){
       const data = {}; // 闭包中的数据被隐藏 不被外界访问
       return {
           set: function(key,val){
               data[key] = val;
           },
           get:function(key){
               return data[key];
           }
       }
   }
   
   const c = createCache();
   c.set('a',100);
   console.log(c.get('a'));
   
   
   // 例子2
   function slow(x) {
     // 这里可能会有重负载的 CPU 密集型工作
     alert(`Called with ${x}`);
     return x;
   }
   
   function cachingDecorator(func) {
     let cache = new Map();
   
     return function(x) {
       if (cache.has(x)) {    // 如果缓存中有对应的结果
         return cache.get(x); // 从缓存中读取结果
       }
   
       let result = func(x);  // 否则就调用 func
   
       cache.set(x, result);  // 然后将结果缓存（记住）下来
       return result;
     };
   }
   
   slow = cachingDecorator(slow);
   
   alert( slow(1) ); // slow(1) 被缓存下来了
   alert( "Again: " + slow(1) ); // 一样的
   ```

   

4. 创建10个 `<a>` 标签，点击的时候弹出对应的序号。

   这个题目和[这里](https://jiaqicoder.com/2021/06/24/JavaScript-ES6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/#%E9%9D%9E%E5%B8%B8%E7%BB%8F%E5%85%B8%E7%9A%84%E6%A1%88%E4%BE%8B)一模一样：

   ```js
   // 创建10个a标签 点击的时候弹出对应的序号
   // 方法一
   for(let i = 0; i<10;i++){
       a = document.createElement('a');
       a.innerHTML = i +' ';
       a.addEventListener('click',(e)=>{
           e.preventDefault();
           alert(i);
       })
       document.body.appendChild(a);
   }
   // 方法二
   for (i = 0; i < 10; i++) {
       (function (i) {
           a = document.createElement('a');
           a.innerHTML = i + ' ';
           a.addEventListener('click', (e) => {
               e.preventDefault();
               alert(i);
           })
           document.body.appendChild(a);
       })(i)
   }
   ```
   
   ```js
   const div = document.querySelector('div');
   // 创建一个文档片段，还没有插入到文档片段中
   const frag = document.createDocumentFragment();
   for(let i = 0; i<10;i++){
       const a = document.createElement('a');
       a.innerHTML = `list item ${i}`;
       // 将a标签插入到文档片段中
       frag.appendChild(a);
       a.addEventListener('click',()=>{
           console.log(i);
       })
   }
   div.appendChild(frag);
   ```
   
   

## 异步和单线程

### 前置知识

- 单线程和异步
  - 单线程：js是单线程语言，只能同时做一件事情。因为js可以修改DOM结构，所以js和dom渲染必须共用同一个线程。
  - 异步：遇到等待（网络请求，定时任务）不能卡住，因此需要异步，异步是基于callback函数形式，当存在许多callback时，容易导致回调地狱，因此又诞生了promise。
    - 因为js是单线程语言，异步不会阻塞代码执行，同步会阻塞代码执行
  - 应用场景：
    - 网络请求：如ajax加载图片
    - 定时任务：如setTimeOut

### 题目

1. 同步和异步的区别是什么？

   - 基于Js是单线程语言，异步不会阻塞代码执行，同步会阻塞代码执行。

2. 用 `promise` 加载一张图片

   ```js
   const url = 'https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202109101448581.png';
   
   function loadImg(url){
       return new Promise((resolve,reject)=>{
           const img = document.createElement('img');
           img.src = url;
           img.onload= ()=>{
               resolve(img);
           }
           img.onerror=()=>{
               reject(new Error(`图片加载失败 ${src}`))
           }
       })
   }
   loadImg(url).then((img)=>{
       document.body.appendChild(img);
   }).catch(err=>console.log(err));
   ```

3. 前端使用异步的场景有哪些？

   - 网络请求：如ajax加载图片
   - 定时任务：如setTimeOut

## DOM 

### 前置知识

- DOM 本质
  - 文档对象模型（Document Object Model），简称 DOM，将所有页面内容表示为可以修改的对象。DOM 将 HTML 表示为标签的树形结构。
  - <img src="https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202109101535190.png" alt="image-20210910153552147" style="zoom:50%;" />
  
- DOM 节点的操作
  - 获取DOM节点

    | 方法名                   | 搜索方式     | 可以在元素上调用？ | 实时的？ |
    | ------------------------ | ------------ | ------------------ | -------- |
    | `querySelector`          | CSS-selector | ✔                  | -        |
    | `querySelectorAll`       | CSS-selector | ✔                  | -        |
    | `getElementById`         | `id`         | -                  | -        |
    | `getElementsByName`      | `name`       | -                  | ✔        |
    | `getElementsByTagName`   | tag or `'*'` | ✔                  | ✔        |
    | `getElementsByClassName` | class        | ✔                  | ✔        |

    - `elem.getElementsByTagName(tag)` 查找具有给定标签的元素，并返回它们的集合。`tag` 参数也可以是对于“任何标签”的星号 `"*"`。
    - `elem.getElementsByClassName(className)` 返回具有给定CSS类的元素。
    - `document.getElementsByName(name)` 返回在文档范围内具有给定 `name` 特性的元素。很少使用。

    

  - attribute

    - attribute是HTML标签上的特性，它的值只能够是字符串；修改标签的属性，会改变html结构。

      ```js
      const span = document.querySelector('span');
      span.setAttribute('data-name','hello');
      console.log(span.getAttribute('data-name')); //hello
      span.setAttribute('class','hello')
      ```

  - property

    - property是DOM中的属性，是JavaScript里的对象；修改对象属性，不会体现到html结构中。

      ```js
      const span = document.querySelector('span');
      console.log(span.style.width);
      console.log(span.className); // hello work 字符串
      console.log(span.classList);// ['hello', 'work', length: 2 value: 'hello work']
      console.log(span.parentNode);
      console.log(span.nodeName); //SPAN
      console.log(span.nodeType); //1
      ```

      property和attribute两者都可能导致重新渲染dom。

- DOM 结构操作

  - 新增节点

    - appenChild:

      target.appendChild(newChild)

      newChild作为target的子节点插入最后的一子节点之后

      ```js
      const box = document.querySelector('.box');
      // 新建节点
      const p = document.createElement('p');
      p.innerHTML = 'this is p';
      // 插入节点
      box.appendChild(p);
      ```

    - insertBefore

      target.insertBefore(newChild,existingChild)

      newChild作为target的子节点插入到existingChild节点之前

      existingChild为可选项参数，当为null时其效果与appendChild一样

      ```js
      const box = document.querySelector('.box');
      const ul = document.querySelector('ul');
      const p = document.createElement('p');
      p.innerHTML = 'hello world';
      // box为父盒子 p为新增元素 ul为已经存在的元素
      box.insertBefore(p,ul)
      ```

  - 移动节点

    如果元素已经存在，使用`insertBefore`或者`appendChild`即可移动元素。

  - 获取子元素或者父元素

    | 功能                     | 获取节点        | 获取纯元素             |
    | ------------------------ | --------------- | ---------------------- |
    | 获取父节点               | parentElement   | parentNode             |
    | 获取上一个节点（元素）   | previousSibling | previousElementSibling |
    | 获取下一个节点（元素）   | nextSibling     | nextElementSibling     |
    | 获取第一个节点（元素）   | firstChild      | firstElementChild      |
    | 获取最后一个节点（元素） | lastChild       | lastElementChild       |
    | 获取子节点（元素）       | childNodes      | children               |

  - 删除子元素：

    ```js
    const box = document.querySelector('.box');
    const ul = document.querySelector('ul');
    box.removeChild(ul);
    ```

- DOM 性能

  - Dom操作非常昂贵，避免频繁的DOM操作

  - 对DOM查询做缓存

    ![image-20210912195331814](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202109121953925.png)

  - 将频繁操作改为一次性操作

    ```js
    const list = document.querySelector('ul');
    // 创建一个文档片段，还没有插入到文档片段中
    const frag = document.createDocumentFragment();
    for(let i = 0; i<10;i++){
        const li = document.createElement('li');
        li.innerHTML = `list item ${i}`;
        // 将li标签插入到文档片段中
        frag.appendChild(li);
    }
    list.appendChild(frag);
    ```

### 题目

1. `DOM` 是哪种数据结构？

   文档对象模型（Document Object Model），简称 DOM，将所有页面内容表示为可以修改的对象。DOM 将 HTML 表示为标签的树形结构。(树)

2. `DOM` 操作的常用API？

3. `attribute` 和 `property` 的区别？

   - attribute是HTML标签上的特性，它的值只能够是字符串；修改标签的属性，会改变html结构。
   - property是DOM中的属性，是JavaScript里的对象；修改对象属性，不会体现到html结构中。
   - 两者都可能引起DOM重新渲染。

4. 一次性插入多个 `DOM` 节点，考虑性能

   ```js
   const list = document.querySelector('ul');
   // 创建一个文档片段，还没有插入到文档片段中
   const frag = document.createDocumentFragment();
   for(let i = 0; i<10;i++){
       const li = document.createElement('li');
       li.innerHTML = `list item ${i}`;
       // 将li标签插入到文档片段中
       frag.appendChild(li);
   }
   list.appendChild(frag);
   ```

## BOM 操作

### 前置知识

- navigator

  [navigator](https://developer.mozilla.org/zh/docs/Web/API/Window/navigator) 对象提供了有关浏览器和操作系统的背景信息。navigator 有许多属性，但是最广为人知的两个属性是：`navigator.userAgent` — 关于当前浏览器，`navigator.platform` — 关于平台（可以帮助区分 Windows/Linux/Mac 等）。

  ```js
  console.log(navigator.userAgent);
  // Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.63 Safari/537.36
  console.log(navigator.platform);
  // Win32
  ```

- location

  ```js
  // 在resume.jiaqicoder.com测试
  location.href //'https://resume.jiaqicoder.com/#about'
  location.protocol //'https:'
  location.host //'resume.jiaqicoder.com'
  location.search //'?a=100&b=200'
  location.hash //'#about'
  location.pathname // '/'
  ```

- history

  ```js
  history.back(); // history.go(-1);
  history.forward();//history.go(1);
  ```

### 题目

1. 如何识别浏览器的类型？
2. 分析拆解url各个部分？



## 事件

### 前置知识

- 事件绑定

  ```js
  const button = document.querySelector('button');
  // 通用的事件绑定函数
  
  function bindEvent(elem,type,fn){
      elem.addEventListener(type,fn);
  }
  bindEvent(button,'click',(e)=>{
      e.preventDefault();
      console.log(e.target);
  })
  ```

- 事件冒泡

  `event.stopPropagation();`阻止事件冒泡。

  `event.stopImmediatePropagation();` 阻止事件冒泡并且阻止该元素之后的相同事件类型的监听器触发。

- 事件代理

  ```html
  <div class="box">
      <a href="#">1</a>
      <a href="#">2</a>
      <a href="#">3</a>
      <a href="#">4</a>
      <button>加载更多...</button>
  </div>
  <script>
      const box = document.querySelector('.box');
      box.addEventListener('click',e=>{
          e.preventDefault();
          if(e.target.nodeName= 'A'){
              console.log(e.target.innerHTML);  
          }
      })
  </script>
  ```

### 题目

1. 编写一个通用的事件监听函数

   ```js
   <div class="box">
       <a href="#">1</a>
       <a href="#">2</a>
       <a href="#">3</a>
       <a href="#">4</a>
       <button>加载更多...</button>
   </div>
   <script>
   
       // elem：添加事件监听的元素 type 事件监听的类型
       // seletor：被事件代理的元素（可选）fn：触发监听后执行的回调函数
       function bindEvent(elem, type, selector, fn) {
           if (fn == undefined) {
               // 说明是三个参数的这种情况 selector的值为传入的fn
               // 交换fn和selector的值
               [fn, selector] = [selector, fn];
               console.log(3);
           }
           elem.addEventListener(type, e => {
               if (selector) {
                   // 需要被代理
                   // matches它检查 elem 是否与给定的 CSS 选择器匹配。它返回 true 或 false。
                   if (e.target.matches(selector)) {
   
                       fn.call(e.target, e); // 让this指向触发的元素
                   }
   
               } else {
                   // 普通绑定
                   fn.call(e.target, e);
               }
           })
       }
       const box = document.querySelector('.box');
       bindEvent(box, 'click', 'a', function (e) {
           e.preventDefault();
           console.log(this.innerHTML) //因为要使用this此时无法使用箭头函数
       })
   </script> 
   ```

   

2. 描述事件冒泡的流程

   事件冒泡基于DOM树形结构，会顺着触发元素往上冒泡。

3. 无线下拉的图片列表，如何监听每个图片的点击

   - 通过事件代理
   - 用e.target 获取触发元素
   - 用matches来判断是否为触发元素



## ajax

### 前置知识

- fetch

  ```js
  fetch('https://reqres.in/api/users/',{
      method:'POST',
      headers:{
          'Content-Type':'applicaton/json'
      },
      body:JSON.stringify(
          {
          name:'User 21'
      }
      )
  }).then(res=>{
      return res.json();
  }).then(data=>{
      console.log(data);
  })
  ```


- XMLHttpRequest

  ```js
  // get 请求
  const xhr = new XMLHttpRequest();
  xhr.open('GET', './0.data.json', true); //代表异步
  xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
          if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
              // console.log(xhr.responseText);
              // 解析为json格式
              console.log(JSON.parse(xhr.responseText))
          }
      }
  }
  xhr.send(null);
  
  // send 请求
  const xhr = new XMLHttpRequest();
  xhr.open('POST','./0.data.json',true);
  xhr.onreadystatechange = function(){
    if(xhr.readyState === 4){
      if(xhr.status >= 200 &&xhr.status<300 ||xhr.status===304){
        console.log(xhr.responseText);
      }
    }
  }  
  const postData ={
    userName:'zhangsan',
    age:32
  };
  xhr.send(JSON.stringify(postData));
  ```

  - xhr.readyState

    > - 0 - （未初始化） 还没有调用send()方法
    > - 1 - （载入）已经调用send()方法，正在发送请求
    > - 2 - （载入完成）send（）方法，已经接收到全部响应内容
    > - 3 - （交互）正在解析响应内容
    > - 4 - （完成） 响应内容解析完成，可以在客户端调用

  - xhr.status

    > - 2xx - 表示成功处理请求 如200
    > - 3xx - 需要重新定向，浏览器直接跳转，如301,302,304
    > - 4xx - 客户端请求错误，如404,403
    > - 5xx - 服务器端错误

- 同源策略

  - ajax请求时，浏览器要求当前网页必须和server端同源

  - 同源：协议、域名、端口，三者必须一致
  - 加载图片、css、js可以无视同源策略
    - `<img src=''/>` 可以用于统计打点，使用第三方统计服务
    - `<link/>` `<script/>` 可以使用CDN，CDN一般都是外域
    - `<script>` 可以实现 `JSONP`

- 跨域

  - 所有的跨域，都必须经过server端运行和配合
  - 未经server端运行就实现跨域，说明浏览器有漏洞，危险信号

- JSONP

  `<script>` 可以绕过跨域限制，服务器可以任意动态拼接数据返回。所以，`<script>` 就可以获得跨域的数据，只要服务端愿意返回。

  ```html
  <script>
      // 在 这个页面http://127.0.0.1:5500/24.jsonp.html
      window.callback = function (data) {
          console.log(data);
      }
  </script>
  <!-- 访问不同端口的服务器的数据 -->
  <!-- 该服务器的返回结果为callback({name:'zhangsan'}) -->
  <script src="http://localhost/24.jsonp.js"></script>
  
  
  <!--或者说这样写-->
  <script>
      const jsonp = document.createElement('script');
      jsonp.src ='http://localhost/24.jsonp.js'
      document.body.appendChild(jsonp);
      const callback = function (data) {
          console.log(data);
      }
  </script>
  ```

  

- 服务器设置header

  ```js
  const express = require('express')
  const bodyParser = require('body-parser')
  
  const app = express()
  // 处理参数
  app.use(bodyParser.json());
  app.use(bodyParser.urlencoded({ extended: false }));
  
  // 设置允许跨域访问该服务
  app.all('*', function (req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header('Access-Control-Allow-Methods', 'PUT, GET, POST, DELETE, OPTIONS');
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    res.header('Access-Control-Allow-Headers', 'Content-Type');
    next();
  });
  
  // 启动监听
  app.listen(3000, () => {
    console.log('running...')
  })
  ```

  

### 题目

1. 手写一个简易的`ajax` 

   ```js
   function ajax(url) {
       const p = new Promise((resolve, reject) => {
           const xhr = new XMLHttpRequest();
           xhr.open('GET', url, true);
           xhr.onreadystatechange = function () {
               if (xhr.readyState === 4) {
                   if (xhr.status > 199 && xhr.status < 300 || xhr.status === 304) {
                       resolve(xhr.responseText);
                   } else {
                       reject('someting went wrong');
                   }
               }
           }
           xhr.send(null);
   
       });
   
       return p;
   }
   ajax('https://reqres.in/api/users/').then(data => console.log(JSON.parse(data))).catch(err => console.log(err));
   ```

   

2. 跨域的常见方式


- jsonp

- cors



## 存储

### 前置知识

- cookie

  cookie全称为HTTP cookie，简称cookie，是浏览器存储数据的一种方式。cookie是本地存储，一般会自动随着浏览器发送请求时发送到服务器端。本身用于浏览器和服务器通讯，一般最大为4kb。

  ```js
  document.cookie = 'username=zs';
  document.cookie = `user=${encodeURIComponent('张三')}`; //中文需要用到encodeURIComponent
  // 告知失效时间 如果不设置，默认会是session
  document.cookie = `hobby=sleep;expires=${new Date('2022-09-09')}`
  // 还可以设置domain path 当domain、path以及name的值完全相同时，下一个cookie才会覆盖上一个
  document.cookie = `hobby=eat;path='/info';domain='jiaqicoder.com;max-age=100 `; 
  // 该cookie只能在 jiaqicoder.com/info下读写 且100s后cookie被删除
  // 一个一个的设置但是会一起打印 
  console.log(document.cookie); //username=zs; user=%E5%BC%A0%E4%B8%89;  hobby=sleep
  ```

- localStorage和sessionStorage

  `localStorage`也是一种浏览器存储数据的方式（本地存储），它只是存储在本地，不会发送到服务器端。而cookie会随着浏览器向服务器发送请求时发送到服务器。`localStorage` 中的键值对总是以字符串的形式存储。

  ```js
  localStorage.setItem('name','jiaqii');
  localStorage.setItem('age',332);
  console.log(localStorage.getItem('age'));
  localStorage.removeItem('age');
  localStorage.clear();//清空所有的
  ```

  `localStorage`是持久化本地存储，除非手动清除（比如通过JavaScript删除，或者清除浏览器缓存），否则数据是永远不会过期的。

  但是当会话结束（比如关闭浏览器）的时候， `sessionStorage`中的数据会被清空，它的方法和`localStorage`完全一样。

### 题目

1. 描述`cookie`、`localStorage`、`sessionStorage`的区别

   

## HTTP

### 前置知识

- http状态码

  - 状态码分类
    - 1xx 服务器收到请求
    - 2xx 请求成功
    - 3xx 重定向 
    - 4xx 客户端错误
    - 5xx 服务端错误
  - 常见的状态码
    - 200 成功
    - 301 永久重定向 （配合location，浏览器自动处理）
    - 302 临时重定向 （配合location，浏览器自动处理）
    - 304 资源未被修改，从缓存中读取数据
    - 403 没有权限
    - 404 资源不存在
    - 500 服务器错误
    - 504 网关超时

- http方法

  - 传统的 methods

    - get 获取服务器的数据
    - post 向服务器提交数据

  - 现在的methods

    - get 获取数据

    - post 新建数据

    - patch/put 更新数据

    - delete 删除数据

      > 这些方法只是一个规范，比如get可以用来新建数据，但是尽量遵守规范

  - Restful API

    - 传统的api设计：把每个url当做一个**功能**

    - restful api设计：把每个url当做一个**唯一的资源（标识）**

    - 如何将url设置为资源？

      - 尽量不用url参数

        > 传统的api设计：/api/list?pageIndex=2
        >
        > restful api设计：/api/list/2

      - 用 method 表示操作类型

        > 传统的api：
        >
        > - post请求 /api/create-blog
        > - put请求 /api/update-blog?id=100
        > - get请求 /api/get-blog?id=100
        >
        > restful api:用method 表示操作类型
        >
        > - post请求 /api/blog  如`axios.post('/api/blog',{name:12,age:18})`
        > - put请求 /api/blog/100 如`axios.put('/api/blog/100',{age:12})`
        > - get请求 /api/blog/100 如`axio.get('/api/blog/100')`

- http headers
  - 常见的request headers
    - Accept 浏览器可接收的数据格式
    - Accept-Encoding 浏览器可接收的压缩算法，如gzip
    - Accept-Language 浏览器可接收的语言，如zh-CN
    - Connection: keep-alive 一次TCP连接重复使用
    - cookie 同域一般浏览器会自动发送cookie
    - Host 域名
    - UserAgent 浏览器信息
    - Content-Type 发送数据的格式，如果application/json
  - 常见的response headers
    - Content-Type 返回数据的格式，如果application/json
    - Content-Length 返回的数据的大小
    - Content-Encoding 返回数据的压缩算法，如gzip
    - Set-Cookie 设置cookie
  - 缓存相关的headers：
    - Cache-Control:Expires
    - Last-Modified:If-Modified-Since
    - Etag:If-None-Match 

- http 缓存

  https://juejin.cn/post/6844903763665240072

  - 关于缓存的介绍

    - 什么是缓存？ 

    - 为什么需要缓存？

      部分数据可以直接从缓存中读取，减少网络请求的数据，加快页面加载速度。

  - http缓存策略（强制缓存 + 协商缓存）

    - 强制缓存

      简单粗暴，如果资源没过期，就取缓存，如果过期了，则请求服务器。

      ![image-20210914161638760](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202109141616830.png)

      Cache-Control 在Response-Headers中，控制强制缓存的逻辑。

      **Cache-Control 的几个取值含义：**

      **private：** 仅浏览器可以缓存

      **public：** 浏览器和代理服务器都可以缓存（对于private和public，前端可以认为一样，不用深究）

      **max-age=xxx** 过期时间（重要）

      **no-cache**  不进行强缓存（重要）

      **no-store**   不强缓存，也不协商缓存，基本不用，缓存越多才越好呢

      注意：规则可以同时多个。

      

    - 协商缓存

      触发条件：

      1. Cache-Control 的值为 no-cache （不强缓存）
      2. 或者 max-age 过期了 （强缓存，但总有过期的时候）

      也就是说，不管怎样，都可能最后要进行协商缓存（no-store除外）

      服务器端判断客户端资源，是否和服务端资源一样，如果判断一直则返回304，否则返回200和最新的资源。

      ![协商缓存](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202109172239320.png)

      response header里面的设置

      ```bash
      etag: '5c20abbd-e2e8'
      last-modified: Mon, 24 Dec 2018 09:49:49 GMT
      ```

      etag：每个文件有一个，改动文件了就变了，就是个文件hash，每个文件唯一。

      last-modified：文件的修改时间，精确到秒。

      也就是说，每次请求返回来 `response header` 中的 `etag` 和 `last-modified`，在下次请求时在 `request header` 就把这两个带上，服务端把你带过来的标识进行对比，然后判断资源是否更改了，如果更改就直接返回新的资源，和更新对应的 `response header` 的标识 `etag`、`last-modified` 。如果资源没有变，那就不变 `etag`、`last-modified`，这时候对客户端来说，每次请求都是要进行协商缓存了，即：发请求-->看资源是否过期-->过期-->请求服务器-->服务器对比资源是否真的过期-->没过期-->返回304状态码-->客户端用缓存的老资源。

      这就是一条完整的协商缓存的过程。

      

  - 刷新操作方式，对缓存的影响
  
    

### 题目

1. http 常见的状态码有哪些？

2. http 常见的header有哪些？ 

3. 什么是Restful Api？

4. 描述http的缓存机制？（**重要**）

   

## 手写

### 防抖

- 监听输入框，文字变化后会触发change事件
- 直接用keyup事件，则会频繁触发change事件
- 防抖：用户输入结束或暂停时，才会触发change事件
