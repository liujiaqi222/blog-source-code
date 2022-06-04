---
title: TypeScript学习笔记
date: 2022-01-21
tags: [TypeScript]
---



# TypeScript 简介

TypeScript **是一个开源的、渐进式包含类型的 JavaScript 超集**，由微软创建并维护。创建它的目的是让开发者增强 JavaScript 的能力并使应用的规模扩展变得更容易。

它的主要功能之一是为 JavaScript 变量提供类型支持。在 JavaScript 中提供类型支持可以实现静态检查，从而更容易 地重构代码和寻找 bug。最后，TypeScript 会被编译为简单的JavaScript 代码。

1. 要开始使用 TypeScript，我们需要用 npm 来安装它。

	```bash
	npm install -g typescript
	```

2. 使用tsc对ts文件进行编译

	```bash
	tsc xxx.ts
	```
	
	

一些开发者还是更习惯使用普通的 JavaScript 语言，而不是 TypeScript 来进行开发。但是在 JavaScript 中使用一些类型和错误检测功能也是很不错的。

好消息是 TypeScript 提供了一个特殊的功能，允许我们在编译时对代码进行错误检测和类型 检测！要使用它的话，需要在计算机上全局安装 TypeScript。使用时，只需要在 JavaScript 文件 的第一行添加一句`// @ts-check`。

![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20211204212613.png)

 TypeScript类型
===

通过类型声明可以指定TS中变量（参数、形参）的类型。指定类型后，当为变量赋值时，TS编译器会自动检查值是否符合类型声明，符合则赋值，否则报错。

 ```ts
let 变量: 类型;

let 变量: 类型 = 值;

//圆括号外面的类型是用来设置返回值的类型
function fn(参数: 类型, 参数: 类型): 类型{
...
}
 ```

 ```ts
 // 声明一个变量a，并且指定它的类型为 number
let a: number;

// a的类型设置为了number，在以后的使用过程中a只能是数字类型了
a = 19;


// 声明变量后直接赋值
let c: boolean = true;

// 也可以直接使用字面量进行类型声明

  
//JS 函数不考虑参数的类型和个数
// 有了ts就可以限制类型和个数了
// 指定返回值类型为number
function sum(a:number, b:number):number {
 return a + b;
}

sum(1,2); //如果不是number类型或者个数不为2的话，会报错。
 ```

 

 ## 自动类型推断

TS拥有自动的类型判断机制，所以如果你的变量的声明和赋值时同时进行的，可以省略掉类型声明。

```ts
let c: boolean = true;

// 上面的写法太啰嗦了，typescript有类型推断机制
// 它可以根据变量赋的值自动给变量设置一个类型

let d = true; //d 之后只能是布尔值了
```

## 基本类型
|  类型   |       例子        |              描述              |
| :-----: | :---------------: | :----------------------------: |
| number  |    1, -33, 2.5    |            任意数字            |
| string  | 'hi', "hi", `hi`  |           任意字符串           |
| boolean |    true、false    |       布尔值true或false        |
| 字面量  |      其本身       |  限制变量的值就是该字面量的值  |
|   any   |         *         |            任意类型            |
| unknown |         *         |         类型安全的any          |
|  void   | 空值（undefined） |     没有值（或undefined）      |
|  never  |      没有值       |          不能是任何值          |
| object  |  {name:'孙悟空'}  |          任意的JS对象          |
|  array  |      [1,2,3]      |           任意JS数组           |
|  tuple  |       [4,5]       | 元素，TS新增类型，固定长度数组 |
|  enum   |    enum{A, B}     |       枚举，TS中新增类型       |

### 字面量类型

 ![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20211204222628.png)
```ts
// 可以使用 | 来连接多个类型

let b: 'male' | 'female'
  
// 此时b可以赋值male或者female
b = 'male';

let c: boolean | string;
c = 'hello';
c = true;
```

### any类型

一个变量设置类型为 `any`后相当于对该变量关闭了 ts 类型检测。

```ts
// any表示任意类型
let d: any;
```

声明变量如果不指定类型，则ts解析器会自定判断变量为 any 类型。

```ts
let e; //隐式声明了any类型
e = 'hello';
e = 123;
```


### unknown 类型

unknown是类型安全的any，unkonwn类型的变量，不能直接赋值给其他变量。
![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20211204225412.png)

因为any类型的变量可以赋值给任意变量。

![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20211204225149.png)

### 类型断言

但是，如果想要把 unknown类型变量赋值给其他变量，可以使用类型断言（any类型也可以进行类型断言）：

1. 先进行 `typeof` 类型判断后，再赋值。

	 ![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20211204225751.png)

2. 给unknown 或者 any变量类型断言，告诉解析器变量的实际类型。

	```ts
	语法：
		变量 as 类型
	or  <类型>变量
	```

	 ```ts
	let m: unknown;
	m = 'hello';
	// 类型断言
	let s: string;
	s = m as string; //写法一
	s = <string>m; //写法二
	 ```

	```ts
	let hello: any = '嘉琪';
	let p: string = <string>hello;
	```

### void 和 never

void用来表示为空，以函数为例，表示没有返回值或者返回null或者undefined。

```ts
function fn():void {
 return null;
}
```

never 表示永远不会返回结果。
![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20211204233638.png)
![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20211204233843.png)

### object

一般设置对象的属性。

```ts
// Object 表示对象

let a: object; //用处不大

// 一般设置对象的属性
// {} 用来指定对象中可以包含哪些属性
// 语法：{属性名:属性类型}
let b: { name: string,age:Number };
b = { name: '嘉琪coder',age:22 };

// &表示同时
// j要有name和age两个属性
let j: { name: string } & { age: number };
j = {name:'jiaqi',age:12}
```

如果指定了对象属性类型，则之后需要完全按照指定的要求来写。

![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20211209154609.png)
但如果在赋值的时候不想写age类型，可以在属性名后面加上`?`来表示属性是可选的。

```ts
let b: { name: string, age?: number };
b = { name: 'jiaqi' };
```

如果要求必须写某个属性，但其他的属性可写可不写。

`[propName:string]:any` 表示属性名为字符串类型，属性值为任意类型。 `propName`为随便写的变量名。

```ts
let c: {name: string,[propName:string]:any};
c = { name: 'jiaqi', a: 1, b: 'hello' }
```

设置函数的类型：

```ts
// 设置函数的结构类型声明
let d: (a: number, b: number) => number;
d = (n1:number, n2:number) =>{
 return n1+ n2;
}
```

### array

用 `类型[]`或者`Array<类型>`来定义数组元素的类型。

```ts
// string[]表示字符串数组

let e: string[];

e = ['a', 'b'];

let f: number[];
let g: Array<number>;
```

### tuple

元组是固定长度的数组。(ts新增)

```ts
// 元组是固定长度的数组

let h: [number, number];
h = [1, 2];//固定长度，只能是2个元素

//info是一个数组，它里面的元素是元组
let info: [number, string][];

info = [
 [1, '嘉琪'],
 [2, '小王'],
 [3,'小黑']
]
```

### enum

枚举把所有可能的情况列举出来。(ts新增)

默认会给枚举的名称依次赋予0,1,2等这些值。不过可以给他们自行赋值。

```ts

// 枚举
enum Gender{
 Male,Female
}

console.log(Gender.Male); //0

let i: { name: string, gender: Gender }
i = {
 name: 'jiaqi',
 gender:Gender.Female,
}

enum Directions {
	up = 1,
	left = 4,
	right,
	down
}
//right的值为5，down的值为6
```


### union

union的意思是并集。

```ts
let pid: string | number
pid = '22';
pid = 22;

```


### type

可以用来给类型起别名，比如：

```js
// k的值是1~5中的一个
let k: 1 | 2 | 3 | 4 | 5;
// 与此同时p的取值也和k一样，因此可以给类型起个别名

type myType = 1 | 2 | 3 | 4 | 5;
let p: myType;
```


### 函数定义

可以定义函数接收的参数类型，和返回值的类型。

```ts
// 接收2个number类型的变量，且返回值也是number类型
function addNum(x: number, y: number):number {
 return x + y;
}
```

```ts
function log(msg: string | number):void {
 console.log(msg);
}
```


## interface

interface 不能用于定义原始值的类型，是用于定义对象类型（对象或者函数）。

1. 普通对象使用interface：

	```ts
	interface UserInterface {
	 id: number,
	 name:string
	}
	
	const user:UserInterface = {id:1,name:'jiaqi'}
	```

	可以同时定义多个  同名的`interface`，最终的结果就是把他们合在了一起。

	![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20220116224811.png)


2. 箭头函数使用interface：

	```ts
	interface MathFunc {
	  (x:number,y:number):number
	}
	
	const add: MathFunc = (x: number, y: number): number => x + y;
	const subtract: MathFunc = (x: number, y: number): number => x - y;
	```

3. 使用interface限制类的结构，只定义方法的结构，不考虑具体的实现。

	类实现接口使用的是 `implements` 关键字：

	```ts
	interface PersonInterface {
	  id: number,
	  name: string,
	  register():string,
	}
	
	class Person implements PersonInterface{
	  id: number;
	  name: string;
	  constructor(id:number,name:string) {
	    this.id = id;
	    this.name = name;
	  }
	  register(): string {
	      return `${this.name} is registered`
	  }
	}
	```

	如果说对象中的某个属性是可选的，则使用 `?` 来定义。

	```ts
	interface UserInterface {
	  id: number,
	  name: string,
	  age?:32,
	}
	const user1:UserInterface = {id:1,name:'jiaqi'}
	```

可以用 `readobly`修饰某个属性，表示只读，不可以修改其值。

![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20220115224542.png)


## 泛型

在定义函数或者类时，如果遇到类型不明确就可以使用泛型。

泛型的英文是`generics`，我觉得它的作用是定义可复用的“组件”。`<T>`就相当于一个占位符，之后使用该“组件”的时候再给它一个确定的类型。

```ts
function getArray<T>(items: T[]): T[]{
  return items;
}

const numArray = getArray<number>([1, 2, 3, 4]);
const strArray = getArray<string>(['jaiqi', 'string']);

```

```js
class MyClass<T>{
  name: T
  constructor(name: T) {
    this.name = name;
  }
}

const jiaqi = new MyClass<string>('jiaqi');
```

也可以同时指定多个泛型：

```ts
function fn<T,K>(a:T,b:K):T {
  return a;
}

fn<number, string>(20, 'h');

```

可以让泛型继承自某个接口（或者类），就能限制泛型的范围。

```ts
interface MyInterface{
  length:number
}
// 要求传入的参数必须有length属性
function fn2<T extends MyInterface>(a:T):number {
  return a.length;
}

fn2({ length: 2 });
fn2('hello'); // 字符串自带length属性
```



# 编译选项

## 自动编译文件

编译文件时，使用 -w 指令后，TS编译器会自动监视文件的变化，并在文件发生变化时对文件进行重新编译。

```bash
tsc xxx.ts -w
```

## 自动编译整个项目

**在添加 ` tsconfig.json` 配置文件后**（空的也可以，只要创建这个文件），然后使用 `tsc` 命令即可完成对整个项目的编译。

使用 `tsc --init`  即可自动生成配置文件，而且带了compilerOptions的全部配置项和解释。

![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20220115213620.png)

使用 `tsc -w` 即会监视所有的文件！

注意：一个要在添加 `tsconfig.json`文件后才能实现这个功能。

![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/tsc1.gif)



## 配置选项：

**include**：

-   定义希望被编译文件所在的目录
-   默认为`["**/*"]` （`**`代表任意文件夹，`*`代表任意文件）

- 示例：

	```json
	"include":["src/**/*", "tests/**/*"]
	```

上述示例中，所有src目录和tests目录下的文件都会被编译。

**exclude**：
-   定义需要排除在外的目录
    
-   默认值：["node_modules", "bower_components", "jspm_packages"]

- 示例：
	-   "exclude": ``["./src/hello/**/*"]`
        
	-   上述示例中，src下hello目录下的文件都不会被编译


**extends**:
-   定义被继承的配置文件

- 示例：

	-   "extends": "./configs/base"
        
	-   上述示例中，当前配置文件中会自动包含config目录下base.json中的所有配置信息。

**files**

-   指定被编译文件的列表，只有需要编译的文件少时才会用到
    
-   示例：
    
    -   "files": `[  "core.ts",  "sys.ts",  "types.ts",  "scanner.ts",  "parser.ts",  "utilities.ts",  "binder.ts",  "checker.ts",  "tsc.ts"    ]`
        
    -   列表中的文件都会被TS编译器所编译

**compilerOptions**

编译选项是配置文件中非常重要也比较复杂的配置选项。具体见下文。



## compilerOptions

### target

用来设置ts代码编译的目标版本。

可选值有： ES3（默认）、ES5、ES6/ES2015、ES2016、ES2017、ES2018、ES2019、ES2020、ES2021，ESNext。

比如下面瞎写了一点代码：

```ts
async function getStuff(url:string) {
 const res = await fetch(url);
 const result = await res.json();
 return result;
}

let url = 'https://api.wmdb.tv/api/v1/top?skip=0&limit=10';
console.log(getStuff(url));
```

当把target设置为ES2018时，编译后的js如下：

![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20211217223746.png)

而当把target设置为ES3时（或者说不设置target，因为默认值是ES3）：

![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20211217223854.png)

### module

设置编译后代码使用的模块化系统。可选值为：none, commonjs, amd, system,  umd,  es6, es2015, es2020,  esnext。

```json
"compilerOptions": {
    "module": "CommonJS"
}
```

### lib
一般不用动！

指定代码运行时所包含的库（宿主环境）。可选值：ES5、ES6/ES2015、ES7/ES2016、ES2017、ES2018、ES2019、ES2020、ESNext、DOM、WebWorker、ScriptHost。

```json
"compilerOptions": {
    "target": "ES6",
    "lib": ["ES6", "DOM"],
    "outDir": "dist",
    "outFile": "dist/aa.js"
}
```

### outDir
编译后文件的所在目录。 默认情况下，编译后的js文件会和ts文件位于相同的目录，设置outDir后可以改变编译后文件的位置。

```js
"compilerOptions": {  
     "outDir": "dist"  
}
```

 设置后编译后的js文件将会生成到dist目录。

 ### outFile

将所有的文件编译为一个js文件。默认会将所有的编写在全局作用域中的代码合并为一个js文件，需要将module指定为None、System或AMD则会将模块一起合并到文件之中。

 ```json
"compilerOptions": {
	"target": "ES6"
}
 ```


### allowJS

是否对JS文件进行编译，默认为false。

```json
"compilerOptions": {
	"allowJS": false
}
```

### checkJS

是否检查JS代码是否符合ts语法规范，默认值为false。


### removeComments

编译后是否移除注释，默认为false。

```json
"compilerOptions": {
	"removeComments": false
}
```


### noEmit

是否不生成编译后的文件，默认为false。

```json
"compilerOptions": {
	"noEmit": false
}
```

### noEmitOnError

当有错误的时候就不生成编译文件，默认为false。

```json
"compilerOptions": {
	"noEmitOnError": false
}

```


### strict

所有严格检查的总开关，默认值为false。

### alwaysStrict

用来设置编译后的js文件是否使用严格模式（即自动加上“use strict”语句），默认为false。

```json
"compilerOptions": {
	"alwaysStrict": false
}
```

注意：当js文件有 import export等模块化语句时，就会自动进入到严格模式，ts编译器就不会多此一举写上`'use strict'`语句。

### noImplicitAny

不允许隐式的any类型，默认为false。

```ts
function fn(a,b){
	return a+b;
}
// 如果不开启该选项 a和b就会默认设置为any类型
```


### noImplicitThis

不允许不明确类型的this，默认为false。

### strictNullChecks

严格的空值检查，默认为false。可以检查出有可能为Null的变量。



# 使用WebPack进行打包

## 基本配置

下载相关依赖：

```bash
yarn add webpack webpack-cli typescript ts-loader -D
```

进行配置：

`webpack.config.js`

```js
const path = require('path');

// webpack中的所有配置信息都应该写在module.exports中
module.exports = {
  // 指定入口文件
  entry: './src/index.ts',
  
  // 指定输出位置
  output: {
    // 指定打包后的目录
    path: path.resolve(__dirname, 'dist'),
    //打包后的文件名
    filename:'bundle.js',
  },
  
  // 指定要使用的loader
  module: {
    // 指定加载的规则
    rules: [
      {
        test: /\.ts$/,
        use: 'ts-loader',
        // 要排除的文件夹
        exclude: /node-modules/
      }
    ]
  }
}

```

`tsconfig.json`

```json
//tsconfig.json
{
 "compilerOptions":{
 "module":"ES6",
 "target":"ES6",
 "strict":true
 }
}

```

`package.json`

```json
{
 "devDependencies": {
	省略
 },
 "scripts": {
  "build":"webpack"
 }
}
```

最后在终端中输入 `yarn build`，即可开始进行打包。


## 进阶配置

### html-webpack-plugin

可自动生成HTML文件，并自动引入js文件。

```bash
yarn add html-webpack-plugin -D
```

`webpack.config.js`

```diff

const path = require('path');

+ // 引入插件
+ const HtmlWebpackPlugin = require('html-webpack-plugin');

// webpack中的所有配置信息都应该写在module.exports中
module.exports = {
  // 指定入口文件
  entry: './src/index.ts',
  mode:'production',
  // 指定输出位置
  output: {
    // 指定打包后的目录
    path: path.resolve(__dirname, 'dist'),
    //打包后的文件名
    filename:'bundle.js',
  },
  
  // 指定要使用的loader
  module: {
    // 指定加载的规则
    rules: [
      {
        test: /\.ts$/,
        use: 'ts-loader',
        // 要排除的文件夹
        exclude: /node-modules/
      }
    ]
  },
+  // 插件配置
+  plugins: [
+    new HtmlWebpackPlugin(),
+  ]
}
```

还可以在该插件中进行一些配置：

```js
plugins: [
 new HtmlWebpackPlugin({
 // title: '自定义title',
 template:'./src/template.html', //以某某文件为模板来生成html文件
 }),
]
```

### webpack-dev-server

webpack开发服务器，可以监视代码更新，自动打开网页。

```bash
yarn add webpack-dev-server -D
```

`package.json`

```json
"scripts":{
	"start":"webpack serve --open"
}
```

### clean-webpack-plugin

每次编译前，自动清除dist目录下的文件。(如果不适用该插件将是新文件替代旧文件，可能存在部分旧文件没有被替代的情况)

```bash
yarn add clean-webpack-plugin -D
```

`webpack.config.js`

```diff
const path = require('path');

// 引入插件
const HtmlWebpackPlugin = require('html-webpack-plugin');
+ const { CleanWebpackPlugin } = require('clean-webpack-plugin');

// webpack中的所有配置信息都应该写在module.exports中
module.exports = {
  // 指定入口文件
  entry: './src/index.ts',
  mode: 'production',
  // 指定输出位置
  output: {
    // 指定打包后的目录
    path: path.resolve(__dirname, 'dist'),
    //打包后的文件名
    filename: 'bundle.js',
  },

  // 指定要使用的loader
  module: {
    // 指定加载的规则
    rules: [
      {
        test: /\.ts$/,
        use: 'ts-loader',
        // 要排除的文件夹
        exclude: /node-modules/
      }
    ]
  },
  // 插件配置
  plugins: [
    new HtmlWebpackPlugin({
      // title: '自定义title',
      template: './src/template.html', //以某某文件为模板来生成html文件
    }),
+    new CleanWebpackPlugin(),
  ]
}

```

### resolve

extension：用来指明哪些文件可以作为模块引入

```diff
const path = require('path');

// 引入插件
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

// webpack中的所有配置信息都应该写在module.exports中
module.exports = {
  // 指定入口文件
  entry: './src/index.ts',
  mode: 'production',
  // 指定输出位置
  output: {
    // 指定打包后的目录
    path: path.resolve(__dirname, 'dist'),
    //打包后的文件名
    filename: 'bundle.js',
  },

  // 指定要使用的loader
  module: {
    // 指定加载的规则
    rules: [
      {
        test: /\.ts$/,
        use: 'ts-loader',
        // 要排除的文件夹
        exclude: /node-modules/
      }
    ]
  },
  // 插件配置
  plugins: [
    new HtmlWebpackPlugin({
      // title: '自定义title',
      template: './src/template.html', //以某某文件为模板来生成html文件
    }),
    new CleanWebpackPlugin(),
  ],
  // 用来设置模块
+  resolve: {
+    extensions:['.ts','.js'], //以.ts和.js结尾的文件可以作为模块
+  }
}
```

 

## 配合babel

下载依赖

```bash
yarn add -D @babel/core @babel/preset-env babel-loader core-js
```

`webpack.config.js`

```js
//...略
// 指定要使用的loader
module: {
// 指定加载的规则
rules: [
  {
    test: /\.ts$/,
    // use: ['babel-loader','ts-loader'], 但还可以写一些配置信息
    use: [
      {
        loader: 'babel-loader', // 指定加载器
        options: { //设置babel
          // 指定预定义环境
          presets: [
            ['@babel/preset-env', //指定环境的插件
            {
              // 指定需要兼容的浏览器版本
              targets: {
                "chrome": '88',
                "ie":'9',
              },               
              "corejs": "3", //指定corejs的版本
              // 使用coreJS的方式，usage表示按需加载
              "useBuiltIns":'usage'
            }]
          ]

        }
      }
      , 'ts-loader'],
    // 要排除的文件夹
    exclude: /node-modules/
  }
]
},
//...略
```

另外，也得告诉webpack打包后不要用箭头函数和const

```diff
output: {
 // 指定打包后的目录
 path: path.resolve(__dirname, 'dist'),
 //打包后的文件名
 filename: 'bundle.js',
+ environment: {
+	 arrowFunction:false,
+    const:false,
+ }
},
```

## 其他配置

### 配置scss

首先安装相应的依赖：

```bash
yarn add -D sass sass-loader css-loader style-loader
```

记住，loader的处理顺序是从右往左。

```js
// 指定要使用的loader
module: {
  rules: [
    {
      test: /\.scss$/,
      use: [
        'style-loader',"css-loader",'sass-loader'
      ]
    }
  ]
},
```

接着需要在文件中引入样式：

```js
import './style/index.scss';
```

### 配置post-css

首先安装相应的依赖：

```bash
yarn add -D postcss postcss-loader postcss-preset-env
```

注意postcss-loader的配置放在css-loader的位置之后（执行的时候先执行postcss-loader）

```js
module: {
  rules: [
    {
      test: /\.scss$/,
      use: [
        'style-loader',
        "css-loader",
        {
          loader: 'postcss-loader',
          options: {
            postcssOptions: {
              plugins: [
                [
                  'postcss-preset-env',
                  {
                    browsers: 'last 2 versions'
                  }
                ],
              ]
            }
          }
        },
        'sass-loader'
      ]
    }
  ]
},
```



# 类与TS

## 定义类

一个类由属性和方法组成。使用class关键字即可声明一个类：

```js
class People{

}
```

使用 `new` 关键字可实例化该类，也即会返回一个新的对象：

```js
const jiaqi = new People();
```

### ts 定义属性：

类有两种属性，一是类属性和实例属性。实例属性是属于实例化对象的，而类属性则是属于类的。

实例属性直接写在类中，不需要在前面加上 `const let var` 等关键字，直接写！

类属性需要在声明前加上一个 `static` 关键字。

如果想要属性是只读的，可以在前面加上 `readonly`关键字。

```ts
// 使用class关键词定义一个类

// 直接定义的属性是实例属性，只有实例才能使用
// 使用static关键词定义的属性，只有类才能使用
class People {
  // 定义实例属性 实例化后的对象都会有这些属性
  name: string = '嘉琪';
  age: number = 23;
  // 使用static定义类属性（静态属性），即不需要实例化就可以使用的属性，属于类的属性
  static personName: string = 'jiaqicoder';
  // 只读属性
  readonly hobby: string = 'coding';
}

console.log(People.personName); //jiaqicoder
const per = new People();
per.name = 'tom';
console.log(new People()); //{ name: '嘉琪', age: 23, hobby: 'coding' }
```

当然，readonly很显然是ts的语法，js并不存在。


### 定义方法

直接写上方法的名字，不需要用 `function` 来声明。

如果方法名字前面加上了`static`则是类的方法，否则为实例的方法。

```ts
class People {
 // 定义实例方法
 sayHello() {
 console.log('hello 大家好');
 }
 // 定义类方法
 static eat() {
 console.log('I love food!');
 }
}

People.eat(); //I love food!
new People().sayHello(); //hello 大家好
```

## 构造函数

之前其实存在一个问题：实例化对象时无法向类传参。

定义的类，实例属性都是写得死死的。

```js
class People{
	name = 'jiaqi';
}

new People().name; //jiaqi
new People().name; //jiaqi
new People().name; //jiaqi
```

所有`new People()`得到的name属性的值都是 `jiaqi`。

----

我们希望创建一个通用的类，每次实例化可以创建不同的人。

这时，我们就可以使用构造函数(constructor)，**它会在实例化对象时自动被调用，而且还可以接收到传进来的参数**。

在类中或者constuctor中存在一个`this`，它指向`new 类名()`创建出的对象。通过this就可以像新创建的对象添加属性。（this指向新创建的对象）

```js
class People{
	//constructor可以接收传进来的参数
	constructor(name,age){
		this.name = name; //将传进来的参数添加到this上
		this.age = age; //而this指向new出来的对象上
	}
}

const jiaqi = new People('嘉琪', 0);

```

<img src="https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20220102135834.png"/>

此时，我们终于没有把 name 和 age 属性写死，新创建的对象是使用我们传进来的参数。

除了构造函数可以用到this，其他的函数中也可以用到this，比如

```js
class People{
	//constructor可以接收传进来的参数
	constructor(name,age){
		this.name = name; //将传进来的参数添加到this上
		this.age = age; //而this指向new出来的对象上
	}
	eat(){
		console.log(this.name + '正在吃饭！')
	}
	
}

const jiaqi = new People('嘉琪', 0);
jiaqi.eat();
```

<img src="https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20220102140923.png"/>


当然，如果是静态方法（类的方法）this肯定不是指向新创建的对象，而是指向类本身。（类方法只能类调用，所以指向类本身）

<img src="https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20220102141441.png"/>

![实例的对象是不能调用类方法](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20220102141638.png)

使用ts：

```ts
class People{
 name: string;
 age: number;
 constructor(name:string, age:number) {
 this.name = name;
 this.age = age;
 }
}

const jiaqi = new People('嘉琪', 22);
console.log(jiaqi);
```


## 继承

子类使用 `extends` 关键字继承父类，子类将拥有父类所有的方法和属性。子类如果存在和父类相同的方法，则子类中的方法会覆盖父类的方法

```js
class Animal {
	eat(){
		console.log('eat something')
	}
}

class People extends Animal {

}
const jiaqi = new People();
jiaqi.eat(); // eat something
```

### super

子类的方法可以通过`super`关键字引用它们的原型。这个关键字只能在子类中使用，而且仅限于类构造函数、实例方法和静态方法内部。在类构造函数中使用super可以调用父类构造函数。

```js
 class Animal{
  name: string;
  age: number;
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
  eat() {
    console.log(`${this.name}正在吃`);
  }
}

class People extends Animal{
  gender: string;
  constructor(name:string,age:number,gender: string) {
    super(name, age); //super相当于父类，调用父类的构造函数
    // 此处写的constructor相当于重写了父类的constructor，因此先调用父类的构造函数
    this.gender = gender;
  }
  eat() {
    // super代表当前类的父类
    super.eat();//调用父类的eat方法

  }
}

const jiaqi = new People('jiaqi', 22, 'female');

```

### 抽象类

注意： 这是 ts 才有的语法！

以abstract开头的是抽象类，抽象类不能用于创建对象，其他的类可以继承它。

抽象类就是专门用于被继承的类。在抽象类中可以定义抽象方法，并且子类必须对其进行实现。

```ts
// 抽象类
abstract class Animal{
  name: string;
  age: number;
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
  eat() {
    console.log(`${this.name}正在吃`);
  }
  // 定义一个抽象方法 且没有方法的具体实现
  // 抽象方法必须定义在抽象类中，并且子类必须对其实现！
  abstract sayHello():void;
}
```


子类继承了抽象后，必须要对抽象方法进行具体实现。

```js
class People extends Animal{
  gender: string;
  constructor(name:string,age:number,gender: string) {
    super(name, age); //super相当于父类，调用父类的构造函数
    // 此处写的constructor相当于重写了父类的constructor，因此先调用父类的构造函数
    this.gender = gender;
  }
  eat() {
    // super代表当前类的父类
    super.eat();//调用父类的eat方法
  }
  // 子类必须对其进行实现
  sayHello(){
      console.log('hello');
  }
}
```


## 修饰符

注意： 这是 ts 才有的语法！

修饰符除了static，readonly，还有 `public`，`protected`, `private`。

- public：修饰的属性可以在任意位置（包括子类）访问和修改（默认值）

	`constructor`中显示声明public可以减少代码
	
	```js
	class C {
	  constructor(public name: string, public age: number) {
	  }
	}
	
	//就相当于
	
	class D {
	  name: string;
	  age: number;
	
	  constructor(name: string, age: number) {
	    this.name = name;
	    this.age = age;
	  }
	}
	```
- protected：该属性可以在当前类和它的子类中访问和修改

	![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20220116233547.png)

- private：私有属性只能在当前类内部进行访问和修改，通过添加方法让私有属性能够间接地被访问：

	![](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20220116231650.png)
	```ts
	class Person{
	  name: string;
	  private age: number;
	  constructor(name:string,age:number) {
	    this.name = name;
	    this.age = age;
	  }
	  getAge() {
	    return this.age;
	  }
	  setAge(age:number) {
	    this.age = age;
	  }
	}
	```
	
	除了在ts中使用这种方法，JS原生其实自带`get`和`set`，也可以实现同样的效果：
	
	```js
	class Person {
	  name: string;
	  _age: number;
	  constructor(name: string, age: number) {
	    this.name = name;
	    this._age = age;
	  }
	  get age() {
	    return this._age;
	  }
	  set age(age: number) {
	    if (age > 0) {
	      this._age = age;
	    }
	  }
	}
	
	const jiaqi = new Person('jiaqi', 22);
	console.log(jiaqi.age); //22
	```

# TS小项目

## Food类
### 定义Food类

```ts
class Food{
  // 定义一个属性表示食物所对应的DOM
  element: HTMLElement;
  // 屏幕宽度除以10的值
  screenWidth = 29;
  constructor() {
    // !告诉ts这个元素不可能为null
    this.element = document.getElementById('food')!;
  }
}
```

### Food类的方法

我们需要：

- 获取到食物的坐标，这样才能检测蛇是否吃了它（当蛇的坐标和食物的坐标相同的时候，我们认为蛇吃了食物）
- 随机改变食物的位置

```ts
// 定义食物类
class Food{
  // 略略
  // 获取食物X轴坐标 相对于游戏的stage的距离
  get X() {
    return this.element.offsetLeft;
  }
  // 获取食物X轴坐标 
  get Y() {
    return this.element.offsetTop;
  }

  // 修改食物位置的方法
  change() {
    // 随机生成一个随机数
    // 最小为0，最大为290
    // 蛇一次移动10像素 因此食物的位置也应该是是10的倍数
    // 随机生成一个0~29的数字，然后math.round取整0~29,接着再乘以10，实现坐标是10的倍数
    this.element.style.left = Math.round(Math.random() * this.screenWidth) * 10 +'px';
    this.element.style.top = Math.round(Math.random() * this.screenWidth) * 10 +'px';

  }
}

```

## 积分牌
### 定义积分牌类

```ts
// 定义表示积分牌的类
class ScorePanel{
  score = 0;
  level = 1;
  scoreELe: HTMLElement;
  levelEle: HTMLElement;
  // 设置最大等级
  maxLevel: number;
  // 设置多少分升一级
  scoreToLevelUp: number;
  constructor(maxLevel:number = 10,scoreToLevelUp:number =10) {
    this.scoreELe = document.getElementById('score')!;
    this.levelEle = document.getElementById('level')!;
    this.maxLevel = maxLevel;
    this.scoreToLevelUp = scoreToLevelUp;
  }
}
```

### 积分牌类的方法

```ts
export default class ScorePanel{
  // 略
  // 加分
  addScore() {
    this.scoreELe.textContent = ++this.score + '';
    // 每加scoreToLevelUp分升一级
    if (this.score % this.scoreToLevelUp === 0) {
      this.levelUp();
    }
  }
  // 升级
  levelUp() {
    // 设置上限为10级
    if (this.level < this.maxLevel) {
      this.levelEle.textContent = ++this.level + '';
    }
  }
}
```

## 蛇类

### 定义蛇类

```ts
class Snake {
  snake: HTMLElement; //装蛇的容器
  head: HTMLElement; //表示蛇头
  bodies: HTMLCollection;  //表示蛇的身体（包括头部）
  constructor() {
    // 获取蛇的容器
    this.snake = document.getElementById('snake')!;
    this.head = document.querySelector('#snake div:first-child')!;
    this.bodies = this.snake.children;
  }
}
```

由于蛇的身体可以不断添加，所以应该用getElementById。

> 所有的 `"getElementsBy*"` 方法都会返回一个 **实时的（live）** 集合。这样的集合始终反映的是文档的当前状态，并且在文档发生更改时会“自动更新”。


### 蛇类的方法

```ts
class Snake {
  snake: HTMLElement; //装蛇的容器
  head: HTMLElement; //表示蛇头
  bodies: HTMLCollection;  //表示蛇的身体（包括头部）
  constructor() {
    // 获取蛇的容器
    this.snake = document.getElementById('snake')!;
    this.head = document.querySelector('#snake div:first-child')!;
    this.bodies = this.snake.children;
  }
  // 获取蛇头坐标
  get X() {
    return this.head.offsetLeft;
  }
  get Y() {
    return this.head.offsetTop;
  }
  // 设置蛇头的坐标
  set X(value:number) {
	// 如果值没有变则不用修改
    if (this.X === value) return;
    this.head.style.left = value + 'px';
  }
  set Y(value:number) {
	if (this.Y === value) return;
    this.head.style.top = value + 'px';
  }
  // 蛇身体增长
  snakeGrow() {
    this.snake.insertAdjacentHTML('beforeend','<div></div>');
  }
}
```


## GameControl 类

游戏控制类，控制其他的所有类。

### 定义GameControl类

```ts
import ScorePanel from './ScorePanel';
import Food from './Food';
import Snake from './Snake';

// 游戏控制器
class GameControl{
  snake: Snake;
  food: Food;
  scorePanel: ScorePanel;
  //记录游戏是否结束（是否蛇死了）
  isLive = true;
  constructor() {
    this.snake = new Snake();
    this.food = new Food();
    this.scorePanel = new ScorePanel();
  }
}

```


### GameControl的方法

首先在document范围监听键盘的按下事件，在这里要注意 `this`的指向问题。

```ts
// 游戏控制器
class GameControl{
  snake: Snake;
  food: Food;
  scorePanel: ScorePanel;
  // 存储按键方向
  direction: string = '';
  // 记录游戏是否结束（是否蛇死了）
  isLive = true;
  constructor() {
    this.snake = new Snake();
    this.food = new Food();
    this.scorePanel = new ScorePanel();
    this.init();
  }
  // 游戏初始化 调用后游戏开始
  init() {
    // 键盘按下的事件
    document.addEventListener('keydown', this.keyDownHandler);
  }
  // 创建键盘按下后的回调
  // 使用箭头函数，之后的this.direction中的this就是指向上一级
  keyDownHandler=(e: KeyboardEvent)=> {
    // ArrowUp ArrowLeft ArrowRight ArrowDown
    // IE Up Left Right Down
    this.direction = e.key;
  }
}
```

接着定义一个 run方法让蛇进行移动，注意这里是通过延时器+调用自身来实现run方法的循环调用。

另外，只有当蛇活着的情况下，才会调用run方法。

```ts
class GameControl {
  //==略==
  // 让蛇进行移动
  run() {
    // 根据this.direction让蛇进行移动
    // 向上：top减小 向下：top增加 向左：left减少 向右：left增加
    let X = this.snake.X;
    let Y = this.snake.Y;
    // 根据按键方向修改X和Y的值
    switch (this.direction) {
      case 'ArrowUp':
      case 'Up':
      case 'w':
        Y -= 10;
        break;
      case 'ArrowDown':
      case 'Down':
      case 's':
        Y += 10;
        break;
      case 'ArrowLeft':
      case 'Left':
      case 'a':
        X -= 10;
        break;
      case 'ArrowRight':
      case 'Right':
      case 'd':
        X += 10;
        break;
    }
    // 修改蛇的X和Y值
    this.snake.X = X;
    this.snake.Y = Y;

    //蛇🐍活着的情况下 开启定时调用
    this.isLive && setTimeout(
      this.run.bind(this)
      , 300 - (this.scorePanel.level - 1) * 30)
  }
}
```


## 判断是否撞墙

判断X和Y是否处于合法位置（0~290之间），如果不是，则蛇死了，我们主动`抛出异常`。

```ts
//蛇类
class Snake {
  // 省略
  // 设置蛇头的坐标
  set X(value: number) {
    // 如果值没有变则不用修改
    if (this.X === value) return;
    // X的值合法范围在0~290之间
    if (value < 0 || value > 290) {
      // 蛇撞墙了 抛出异常
      throw new Error('蛇撞墙了');
    }
    this.head.style.left = value + 'px';
  }
  set Y(value: number) {
    if (this.Y === value) return;
    // X的值合法范围在0~290之间
    if (value < 0 || value > 290) {
      // 蛇撞墙了
      throw new Error('蛇撞墙了');
    }
    this.head.style.top = value + 'px';
  }
}

```

同时要在GameControl类中捕获到异常：

```ts
class GameControl {

  // 略
  run() {
    try {
      // 修改蛇的X和Y值
      this.snake.X = X;
      this.snake.Y = Y;
    }
    catch (e) {
      // 出现了异常，游戏结束
      alert(e);
      // 将isLive设置为false
      this.isLive = false;
    }
  }
}
```


## 蛇吃食物

```ts
class GameControl {

  // 让蛇进行移动
  run() {
    // 根据this.direction让蛇进行移动
    // 向上：top减小 向下：top增加 向左：left减少 向右：left增加
    let X = this.snake.X;
    let Y = this.snake.Y;
    // 根据按键方向修改X和Y的值
    switch (this.direction) {
      case 'ArrowUp':
      case 'Up':
      case 'w':
        Y -= 10;
        break;
      case 'ArrowDown':
      case 'Down':
      case 's':
        Y += 10;
        break;
      case 'ArrowLeft':
      case 'Left':
      case 'a':
        X -= 10;
        break;
      case 'ArrowRight':
      case 'Right':
      case 'd':
        X += 10;
        break;
    }
    // 检测蛇是否吃到了食物（食物的位置和蛇的位置是否一致）
    this.checkEat(X, Y);
    try {
      // 修改蛇的X和Y值
      this.snake.X = X;
      this.snake.Y = Y;
    }
    catch (e) {
      // 出现了异常，游戏结束
      alert(e);
      // 将isLive设置为false
      this.isLive = false;
    }

    //蛇🐍活着的情况下 开启定时调用
    this.isLive && setTimeout(
      this.run.bind(this)
      , 300 - (this.scorePanel.level - 1) * 30)
  }
  // 检测蛇是否迟到了食物
  checkEat(X: number, Y: number) {
    if (X === this.food.X && Y === this.food.Y) {
      // 食物位置进行重置
      this.food.change();
      // 分数增加
      this.scorePanel.addScore();
      // 蛇身增加一节
      this.snake.snakeGrow();
    }
  }
}
```

## 完善蛇身


蛇身应该从最后一节开始改，让最后一节找倒数第二节的位置，依次类推。

```ts
moveBody() {
  // 第4节 = 第3节的位置
  // 第3节 = 第2节的位置
  // 第2节 = 蛇头的位置
  // 不用管蛇头，蛇头的位置是由set来改变的
  for (let i = this.bodies.length - 1; i > 0; i--){
    let X = (this.bodies[i - 1] as HTMLElement).offsetLeft;
    let Y = (this.bodies[i - 1] as HTMLElement).offsetTop;
    // 将位置设置到当前身体上
    (this.bodies[i] as HTMLElement).style.left = X + 'px';
    (this.bodies[i] as HTMLElement).style.top = Y + 'px';
  }
}
```

然后，每次修改蛇头的位置的时候，调用moveBody即可。

```ts
// 设置蛇头的坐标
set X(value: number) {
  // 如果值没有变则不用修改
  if (this.X === value) return;

  // X的值合法范围在0~290之间
  if (value < 0 || value > 290) {
    // 蛇撞墙了 抛出异常
    throw new Error('蛇撞墙了');
  }
	// 移动身体
    this.moveBody();
  this.head.style.left = value + 'px';
  
}
set Y(value: number) {
  if (this.Y === value) return;
  // X的值合法范围在0~290之间
  if (value < 0 || value > 290) {
    // 蛇撞墙了
    throw new Error('蛇撞墙了');
  }
	// 移动身体
    this.moveBody();
  this.head.style.top = value + 'px';
  
}
```

此外要考虑到蛇是很多节的，因此蛇向左移动的时候不能向右移动，向上移动的时候不能向下移动，反之亦然。

如果蛇头（第一节）移动后的位置和第二节的位置相同，则蛇发生了掉头。当然，当只有蛇头的时候不用考虑这些。


```ts
// 设置蛇头的坐标
set X(value: number) {
  // 如果值没有变则不用修改
  if (this.X === value) return;

  // X的值合法范围在0~290之间
  if (value < 0 || value > 290) {
    // 蛇撞墙了 抛出异常
    throw new Error('蛇撞墙了');
  }
  // 蛇存在第二节的情况下，不能掉头
  if (this.bodies[1] && (this.bodies[1] as HTMLElement).offsetLeft === value) {
  //  如果发生了掉头，让蛇向反方向继续移动
    if (value > this.X) {
      // 则说明蛇在向右走，发生掉头
      value = this.X - 10;
    } else {
      value = this.X + 10;
    }
  }
  this.head.style.left = value + 'px';
  // 移动身体
  this.moveBody();
  
}
set Y(value: number) {
  if (this.Y === value) return;
  // X的值合法范围在0~290之间
  if (value < 0 || value > 290) {
    // 蛇撞墙了
    throw new Error('蛇撞墙了');
  }
  // 蛇存在第二节的情况下，不能掉头
  if (this.bodies[1] && (this.bodies[1] as HTMLElement).offsetTop === value) {
    //  如果发生了掉头，让蛇向反方向继续移动
      if (value > this.Y) {
        // 则说明蛇在向下走，发生掉头
        value = this.Y - 10;
      } else {
        value = this.Y + 10;
      }
    }
  this.head.style.top = value + 'px';
  // 移动身体
  this.moveBody();
}
```



## 判断蛇是否撞到自己

```ts
// 检查头和身体是否相撞
// 第5节身体开始就可能和头相撞
checkHeadBody(X: number, Y: number) {
  for (let i = 4; i < this.bodies.length; i++){
    if ((this.bodies[i] as HTMLElement).offsetLeft === X && ((this.bodies[i] as HTMLElement).offsetTop === Y) ){
      return true;
    }
  }
}
```

```ts
// 设置蛇头的坐标
set X(value: number) {
  // 如果值没有变则不用修改
  if (this.X === value) return;

  // X的值合法范围在0~290之间
  if (value < 0 || value > 290) {
    // 蛇撞墙了 抛出异常
    throw new Error('蛇撞墙了');
  }
  // 蛇存在第二节的情况下，不能掉头
  if (this.bodies[1] && (this.bodies[1] as HTMLElement).offsetLeft === value) {
    //  如果发生了掉头，让蛇向反方向继续移动
    if (value > this.X) {
      // 则说明蛇在向右走，发生掉头
      value = this.X - 10;
    } else {
      value = this.X + 10;
    }
  }
  // 移动身体
  this.moveBody();
  this.head.style.left = value + 'px';
  if (this.checkHeadBody(this.X, this.Y)) {
    throw new Error('蛇咬到了自己');
  }
}
set Y(value: number) {
  if (this.Y === value) return;
  // X的值合法范围在0~290之间
  if (value < 0 || value > 290) {
    // 蛇撞墙了
    throw new Error('蛇撞墙了');
  }
  // 蛇存在第二节的情况下，不能掉头
  if (this.bodies[1] && (this.bodies[1] as HTMLElement).offsetTop === value) {
    //  如果发生了掉头，让蛇向反方向继续移动
    console.log('hh');
    if (value > this.Y) {
      // 则说明蛇在向下走，发生掉头
      value = this.Y - 10;
    } else {
      value = this.Y + 10;
    }
  }
  // 移动身体
  this.moveBody();
  this.head.style.top = value + 'px';
  if (this.checkHeadBody(this.X, this.Y)) {
    throw new Error('蛇咬到了自己');
  }
}
```