---
title: JS模块化_Promise_EventLoop_宏任务和微任务
date: 2021-08-04 14:20:54
tags: [es6, 模块化, EventLoop, 宏任务, 微任务]
---

# ES6模块化

## 1. node中如何实现模块化

node.js 遵循了 CommonJS 的模块化规范。其中：

- 导入其它模块使用 require() 方法

- 模块对外共享成员使用 module.exports 对象

模块化的好处：
大家都遵守同样的模块化规范写代码，降低了沟通的成本，极大方便了各个模块之间的相互调用，利人利己 。

## 2. 模块化规范分类

在 ES6 模块化规范诞生之前，JavaScript 社区已经尝试并提出了 AMD、CMD、CommonJS 等模块化规范。
但是，这些由社区提出的模块化标准，还是存在一定的差异性与局限性、并不是浏览器与服务器通用的模块标准，例如：

-  AMD 和 CMD 适用于浏览器端的 Javascript 模块化
-  CommonJS 适用于服务器端的 Javascript 模块化
太多的模块化规范给开发者增加了学习的难度与开发的成本。因此，大一统的 ES6 模块化规范诞生了！

## 3. ES6 模块化规范  

ES6 模块化规范是浏览器端与服务器端通用的模块化开发规范。它的出现极大的降低了前端开发者的模块化学习成本，开发者不需再额外学习 AMD、CMD 或 CommonJS 等模块化规范。
ES6 模块化规范中定义：

- 每个 js 文件都是一个独立的模块
-  导入其它模块成员使用 import 关键字
- 向外共享模块成员使用 export 关键字   

## 4. node.js中体验ES6 模块化

node.js 中默认仅支持 CommonJS 模块化规范，若想基于 node.js 体验与学习 ES6 的模块化语法，可以按照如下两个步骤进行配置：
① 确保安装了 v14.15.1 或更高版本的 node.js
②在终端中输入`npm init -y`生成package.json，然后在  package.json 的根节点中添加 "type": "module" 节点 。

```json
{
  "type":"module",
  "name": "8.es6",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```

## 5. ES6 模块化的基本语法

ES6 的模块化主要包含如下 3 种用法：
① 默认导出与默认导入
② 按需导出与按需导入
③ 直接导入并执行模块中的代码  

### 5.1 默认导出

默认导出语法： `export default 需要导出的成员`

```js
let n1=10;
let n2=20;

function show(){

}

// 使用export default 默认导出语法 向外共享n1和show 2个成员
export default {
    n1,
    show
}
```

### 5.1默认导入

默认导入的语法： `import 接收名称 from '模块标识符'  `

```js
import  m1 from './01.默认导出.js'
// m1是自定义的名字
console.log(m1);
```

### 5.1 默认导出的注意事项  

每个模块中，只允许使用唯一的一次 export default，否则会报错！  

默认导入时的接收名称可以任意名称，只要是合法的成员名称即可 。

### 5.2按需导出

按需导出语法： `export 需要导出的成员` 

```js
export let s1 = 'aaa';
export let s2 = 'ccc';
export function say() {

}
```

### 5.2 按需导入

按需导入的语法：`import {s1} from '模块标识符'`

```js
import { s1, s2, say } from './03.按需导出.js'

console.log(s1);
console.log(s2);
console.log(say);
```

### 5.2按需导入和按需导出的注意事项

① 每个模块中可以使用多次**按需导出**
② **按需导入**的成员名称必须和**按需导出**的名称**保持一致**
③ 按需导入时，可以使用**as 关键字**进行重命名
④ 按需导入可以和默认导入一起使用  

```js
import info,{ s1, s2 as str2, say } from './03.按需导出.js'
// info 为默认导入

console.log(s1);
console.log(str2);
console.log(say);
console.log(info);
```

### 5.3 直接导入并执行模块中的代码

如果只想单纯地执行某个模块中的代码，并不需要得到模块中向外共享的成员。此时，可以**直接导入模块**即会执行模块代码，示例代码如下：  

```js
//05.直接运行模块中的代码
for(let i=0; i<3; i++){
    console.log(i);
}
```

```js
import './05.直接运行模块中的代码.js'
// 导入后便会直接执行
```

# Promise补充

## 1. 基于 then-fs 读取文件内容  

由于 node.js 官方提供的 fs 模块仅支持以回调函数的方式读取文件，不支持 Promise 的调用方式。因此，需要先运行如下的命令，安装 `then-fs` 这个第三方包，从而支持我们基于 Promise 的方式读取文件的内容。

```bash
npm install then-fs
```

调用 then-fs 提供的 readFile() 方法，可以异步地读取文件的内容，它的返回值是 **Promise 的实例对象**。因此可以调用 .then() 方法为每个 Promise 异步操作指定成功和失败之后的回调函数。

```js
// 默认导入
import thenFs from 'then-fs'

thenFs.readFile('./files/1.txt','utf-8').then(data=>{
    console.log(data);
})
thenFs.readFile('./files/2.txt','utf-8').then(data=>{
    console.log(data);
})
thenFs.readFile('./files/3.txt','utf-8').then(data=>{
    console.log(data);
})
```

注意：上述的代码无法保证文件的读取顺序，需要做进一步的改进！  

如果上一个 .then() 方法中返回了一个新的 Promise 实例对象，则可以通过下一个 .then() 继续进行处理。通过 .then() 方法的链式调用，就解决了回调地狱的问题。  

Promise 支持链式调用，从而来解决回调地狱的问题。示例代码如下  :

```js
import thenFs from 'then-fs'


thenFs.readFile('files/1.txt', 'utf8').then(r1 => {
    console.log(r1);
    // 返回一个新的promise对象
    return thenFs.readFile('./files/2.txt', 'utf8');
}).then(r2 => {
    // 获取第2次的读取结果
    console.log(r2);
    // 再次返回一个promise对象
    return thenFs.readFile('./files/3.txt', 'utf8');
}).then(r3 => {
    console.log(r3);
})
```

## 2.基于promise封装读取文件的方法

方法的封装要求：
① 方法的名称要定义为 getFile
② 方法接收一个形参 fpath，表示要读取的文件的路径
③ 方法的返回值为 Promise 实例对象  

```js
import fs from 'fs'

function getFile(fpath){
    return new Promise(function(resolve,reject){
        fs.readFile(fpath,'utf-8',(err,data)=>{
            if(err) return reject(err);
            resolve(data);
        })
    })
}

getFile('./files/1.txt').then((r1)=>{
    console.log(r1);
},(err)=>{
    console.log(err.message);
})
```

# EventLoop

## 1. JavaScript 是单线程的语言

JavaScript 是一门单线程执行的编程语言。也就是说，同一时间只能做一件事情。

![image-20210804175533250](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108041755351.png)

单线程执行任务队列的问题：
如果前一个任务非常耗时，则后续的任务就不得不一直等待，从而导致程序假死的问题。

## 2. 同步任务和异步任务

为了防止某个耗时任务导致程序假死的问题，JavaScript 把待执行的任务分为了两类：
① 同步任务（synchronous）
-  又叫做非耗时任务，指的是在主线程上排队执行的那些任务
-  只有前一个任务执行完毕，才能执行后一个任务

 ② 异步任务（asynchronous）
- 又叫做耗时任务，*异步任务由 JavaScript 委托给**宿主环境**进行执行*
- 当异步任务执行完成后，会通知 JavaScript 主线程执行异步任务的回调函数 。

## 3.同步任务和异步任务的执行过程

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108041813594.png" alt="image-20210804181315494" style="zoom:67%;" />

① 同步任务由 JavaScript 主线程次序执行。
② 异步任务**委托给**宿主环境执行。
③ 已完成的异步任务**对应的回调函数**，会被加入到任务队列中等待执行。
④ JavaScript 主线程的***执行栈***被清空后，会读取任务队列中的回调函数，次序执行。
⑤ JavaScript 主线程不断重复上面的第 4 步 。

## 4.EventLoop的基本概念

JavaScript 主线程从“任务队列”中读取异步任务的回调函数，放到执行栈中依次执行。这个过程是循环不断的，所以整个的这种运行机制又称为 EventLoop（事件循环）。  

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108041815433.png" alt="image-20210804181557376" style="zoom:67%;" />

输出顺序：ADCB

- A 和 D 属于同步任务。会根据代码的先后顺序依次被执行。
- C 和 B 属于异步任务。它们的回调函数会被加入到任务队列中，等待主线程空闲时再执行 。

# 宏任务和微任务

## 1. 什么是宏任务和微任务

JavaScript 把异步任务又做了进一步的划分，异步任务又分为两类，分别是：

![image-20210804182047617](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108041820682.png)

① 宏任务（macrotask）
- 异步 Ajax 请求、
- setTimeout、setInterval、
- 文件操作
- 其它宏任务

② 微任务（microtask）

- Promise.then、.catch 和 .finally
- process.nextTick
- 其它微任务  

## 2.宏任务和微任务的执行顺序

![image-20210804182243122](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202108041822151.png)

每一个宏任务执行完之后，都会检查**是否存在待执行的微任务**。

如果有，则执行完所有微任务之后，再继续执行下一个宏任务。  

**宏任务和微任务同时存在的情况下，会先执行微任务。**

## 3.生活举例

① 小云和小腾去银行办业务。首先，需要取号之后进行排队
- 宏任务队列

② 假设当前银行网点只有一个柜员，小云在办理存款业务时，小腾只能等待

- 单线程，宏任务按次序执行

③ 小云办完存款业务后，柜员询问他是否还想办理其它业务？

- 当前宏任务执行完，检查是否有微任务

④ 小云告诉柜员：想要买理财产品、再办个信用卡、最后再兑换点马年纪念币？

- 执行微任务，后续宏任务被推迟
  

⑤ 小云离开柜台后，柜员开始为小腾办理业务

- 所有微任务执行完毕，开始执行下一个宏任务  

## 4.代码举例

例子1：

```js
// 宏任务
setTimeout(() => {
    console.log(1);
}, 0);

// Promise本身是同步的，但它的then方法和catch方法是异步的
new Promise(function(resolve){
    console.log(2);
    resolve();
}).then(function(){
    console.log(3);
});

console.log(4);
```

值得注意：`Promise()`本身是同步的，尽管它的then方法和catch方法是异步的。

输出顺序：2431

分析：

1.先执行所有的同步任务

- new Promise()和console.log(4)

2.再执行微任务

- .then()

3.接着执行宏任务

- setTimeout()

例子2：

```js
// 1.同步 
console.log(1);

// 宏任务
setTimeout(() => {
    
    console.log(2);
    new Promise(function(resolve){
        console.log(3);
        resolve();
    }).then(function(){
        console.log(4);
    })
}, 0);


new Promise(function(resolve){
    // 2.Promise()是同步的
    console.log(5);
    resolve();
}).then(function(){
    // 3.then()方法是异步微任务
    console.log(6);
})

// 宏任务
setTimeout(() => {
    console.log(7);
    new Promise(function(resolve){
        console.log(8);
        resolve();
    }).then(function(){
        console.log(9);
    })
}, 0);
```



