---
title: JavaScript ES6 学习笔记
date: 2021-06-24 12:19:35
tags: JavaScript
---

# ES6简介

![image-20210530104137712](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210530104138.png)

![image-20210530104351721](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210530104352.png)

![image-20210530104439339](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210530104440.png)

# const

const就是为了那些一旦初始化就不希望重新赋值的情况设计的。使用 const声明常量，一旦声明，就必须立即初始化。

const声明的常量，允许在**不重新赋值**的情况下修改它的值。

```js
const person={gender:"male"};
person[gender]="female";
```

# let、const、var的区别

![image-20210530131645190](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210530131646.png)

1.重复声明：已经存在的变量，又声明了一次。

var允许重复声明，let、 const不允许重复声明

2.变量提升

var 会提升变量的声明到当前作用域顶部，但是赋值不会提升；let和const不会变量提升。

3.暂时性死区

只要作用域内存在 let、const，它们所声明的变量或常量就自动“绑定”这个区域，不再受到外部作用域的影响。

let、const存在暂时性死区，var不存在。

```js
let c=2;
let d=2;
function func(){
    console.log("d",d);//d
    //console.log("c",c);//  Cannot access 'c' before initialization
    let c=1;
}
func(c);
```

4.window 对象的属性和方法

在全局作用域中，var声明的变量，通过function声明的函数会自动变为window对象的属性和方法；let、const不会。

```js
var age=18;
var add=function(){}
console.log(window.add) //18
console.log(window.add===add);//true    
let height=158;
const grow=function(){}
console.log(window.height); //undefined
console.log(window.grow===grow)//false
```

5.块级作用域

var没有块级作用域，let和const有块级作用域。

## 非常经典的案例

```html
<button class="btn">0</button>
<button class="btn">1</button>
<button class="btn">2</button>
<script>
var btns=document.querySelectorAll(".btn");
//1.var
for(var i=0;i<btns.length;i++){
    btns[i].addEventListener("click",function(){
        console.log(i); //都会输出3
    },false)
} //执行for循环的时候，会给所有的btn添加监听(瞬间就能完成)，退出for循环的时候，全局变量i的值为3
//当点击的按钮的时候，自然会输出i=3.

//2.闭包
for(var i=0;i<btns.length;i++){
    (function(index){
        btns[index].addEventListener("click",function(){
        console.log(index); 
    },false)
    })(i)
} 

//3.let
// //此时的i不再是全局变量
for(let i=0;i<btns.length;i++){
    btns[i].addEventListener("click",function(){
        console.log(i); 
    },false)
}
```

1.使用var

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210530144638.png" alt="2.let 和 const 的应用-var" style="zoom: 67%;" />

2.使用闭包

![3.let 和 const 的应用-闭包](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210530144714.png)

3.使用let

![4.let 和 const 的应用-let](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210530144732.png)

# 模板字符串

```html
模板字符串使用反引号 (``) 来代替普通字符串中的用双引号和单引号。模板字符串可以包含特定语法（`${expression}`）的占位符。
```

```js
/ 1.认识模板字符串
// 模板字符串使用反引号 (` `) 来代替普通字符串中的用双引号和单引号。
const user1='alex';
const user2=`alex`;
console.log(user1,user2,user1===user2); //alex alex true

// 2.模板字符串和一般字符串的区别
const person={
    name:'alex',
    age:18,
    gender:"male"
}
// 一般字符串
// const info="name:"+person.name+", age:"+person.age+", gender:"+person.gender;
// 模板字符串
const info=`name:${person.name}, age:${person.age}, gender:${person.gender}`;
console.log(info);
```

![image-20210530154645752](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210530154647.png)

```js
//1.输出多行字符串
// 模板字符串中，所有的空格、换行或缩进都会保留在输出中。
    const info=`hello
world!`;
    console.log(info);
//  2. 输出`和\等特殊字符
console.log(`\``,`\\`); //` \

// 3. 模板字符串的注入
const name="alex";
const person={age:18,gender:"male"};
const getSex=function(gender){
    return gender==="male"?"男":"女";
}
const alex=`${name},${person.age+2},${getSex(person.gender)}`;
//alex,20,男
console.log(alex);
```

# 箭头函数

```js
// 1.认识箭头函数，箭头函数是匿名函数

const add=(x,y)=>{return x+y};

console.log(add(2,3));

// 2.箭头函数的结构

// 参数=>函数体
```

注意：

1.单个参数时可以去掉参数的圆括号；无参数或者多个参数时不能省略参数圆括号。

2.如果函数体只有return语句，可以直接同时省略函数体的花括号和return关键字。

3.返回值是单行对象时，可以省略return关键词，然后在对象的花括号外面加上圆括号。



```js
// 1.单个参数
const add=x=>{return x+1;}
console.log(add(2));

// 2.单行函数体
const add2=x=>x+1;
console.log(add2(2));

// 3.单行对象
const add3=(x,y)=>({value:x+y});
console.log(add3(2,2));
```

## this指向

1.非箭头函数中this指向问题

只有在函数调用的时候，this指向才能确定；this的指向和函数在哪调用无关，只和函数被谁调用有关

```js
function add(){
    console.log(this); 
}
// 只有在函数调用的时候，this指向才能确定
// this的指向和函数在哪调用无关，只和函数被谁调用有关
add(); // 非严格模式下this指向window，严格模式下是undefined
window.add() // window

const calc={
    add:add
}
calc.add(); //calc

const adder=calc.add;
adder();// 非严格模式下this指向window，严格模式下是undefined

document.onclick=function(){
    console.log(this); //this指向绑定的dom，此时为document
}

function Person(name){
    this.name=name;
    console.log(this);
}
var p=new Person("Alex"); // this指向实例化生产的对象
```

2.箭头函数中的this指向

 箭头函数没有自己的this，它会沿着作用域链向外查找。

```js
// 箭头函数没有自己的this
const calc={
    add:()=>{console.log(this)}
}
calc.add(); //window
// 因为箭头函数没有自己的this，所以它会通过作用域链向外查找至全局作用域，而全局作用中this指向window


const c={
    add:function(){
        const adder=()=>{
            console.log(this);
        }
        adder();
    }
};
c.add(); // 指向c对象

const addFn=c.add;
addFn(); // 指向window
```

3.箭头函数不适用的场景

```js
// 1.作为构造函数
// 箭头函数没有this
// const Person=()=>{};
// new Person();

// 2.需要this 指向调用对象的时候
document.onclick=()=>{
    console.log(this); //此时会指向window对象
}
// 3.需要使用arguments时
function add(){
    console.log(arguments);
}
add(1,2); //[1, 2, callee: ƒ, Symbol(Symbol.iterator): ƒ]
const addFn=()=>{
    console.log(arguments);
}
addFn(); //会报错
```

4.箭头函数的应用

如果代码写成下面这样，程序执行会有问题，因为`setInterval`中的回调函数中的`this`会指向`window`对象，导致无法进行加法。

```html
<button id="btn">开始</button>
<span id="result">0</span>
<script>
    const btn=document.getElementById("btn");
    const result=document.getElementById("result");
    const timer={
        time:0,
        start:function(){
            btn.addEventListener(
                "click",function(){
                    setInterval(function(){
                        console.log(this);
                        this.time++;
                        result.innerHTML=this.time;
                    },1000);
                }
            ,false);
        }
    }
    timer.start();
</script>
```

常规的解决是备份`this`。

```js
const timer={
    time:0,
    start:function(){
        var self =this;
        btn.addEventListener(
            "click",function(){
                setInterval(function(){
                    console.log(this);
                    self.time++;
                    result.innerHTML=self.time;
                },1000);
            }
        ,false);
    }
}
 timer.start();
```

但是可以使用**箭头函数**来解决这个问题，箭头函数本身没有`this`，所以它会向外层的作用域链查找`this`。

注意，此时也要把`addEventListener`中的匿名函数改为箭头函数，不然`setInterval`的箭头函数会向外到`addEventListener`中的匿名函数找`this`，而该函数this为绑定的dom节点，从而`setInterval`的箭头函数会把btn（绑定的dom）当做this。

```js
const timer={
    time:0,
    start:function(){
        btn.addEventListener(
            "click",()=>{
                setInterval(()=>{
                    console.log(this);
                    this.time++;
                    result.innerHTML=this.time;
                },1000);
            }
        ,false);
    }
}
 timer.start();
```

将这两个部分都改为箭头函数后，`setInterval`中的匿名函数最终会在`start()`方法找`this`。

而注意到最后会调用`timer.start()`，所以start()中的this就是指向`timer`, 因此 `this.time++;` 和`result.innerHTML=this.time;` 这两句中的`this` 指向的就是`timer`。 

# 解构赋值

## 数组解构赋值

![image-20210603194723759](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210603194725.png)

![image-20210603194752249](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210603194753.png)

![image-20210603194903992](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210603194905.png)

![image-20210603195036092](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210603195037.png)

### 1.解构赋值的定义

解析某一数据的结构，将想要的东西提取出来，赋值给变量或常量。

```javascript
const arr = [1, 2, 3];
// const a=arr[0];
// const b=arr[1];
// const c=arr[2];
const [a, b, c] = [1, 2, 3];
console.log(a, b, c); //1 2 3
```

```javascript
// 模式(结构)匹配,索引值相对应
let [a,b,c]=[1,2,3];

// 如果想要取出1,5,3
const [d,[,,e],f]=[1,[2,4,5],3];
console.log(d,e,f); //1 5 3
```

### 2.解构赋值的默认值

只有当一个数组成员严格等于（===） undefined时，它的的默认值才会生生效。如果默认值是表达式时，默认值表达式是惰性求值的。

```javascript
const [a, b] = []; //a=undefined,b=undefined
const [c = 1, d = 2] = []; //1 2

//只有当一个数组成员严格等于（===） undefined时，对应的默认值才会生生效。
const [e = 1, f = 2] = [3, 4] //3,4
const [i = 2, j = 6] = [5] //5 6
console.log(a, b, c, d, e, f, i, j); //undefined undefined 1 2 3 4 5 6

// 如果默认值是表达式时，默认值表达式是惰性求值的
const func = () => {
    console.log("我被执行了")
    return 2;
};
const [x = func()] = [1]; // 此时函数func并没有执行
const [y=func()]=[];
console.log(x,y); //1 2 
```

### 3.解构赋值的应用

#### 类数组可以进行解构赋值

```javascript
// 1.常见的类数组的解构赋值
// arguments
function func(){
    const [a,b]=arguments;
    console.log(a,b); //1 2
}
func(1,2); 

// NodeList
console.log(document.querySelectorAll('p')); //NodeList(3) [p, p, p]
const [p1,p2,p3]=(document.querySelectorAll("p"));
console.log(p1,p2,p3);
```

#### 函数参数的解构赋值

```javascript
const array=[1,2];
// 不用解构赋值参数的写法如下
// const add=arr=>arr[0]+arr[1];
// 使用解构赋值
const add=([x,y])=>x+y;
console.log(add(array)); //3 

//还可以给形参添加默认值
const add1=([x=1,y=3])=>x+y;
console.log(add1([])); //4
```

#### 交换变量的值

```javascript
let x=1 ,y=2;
//相当于[x,y] =[2,1]，而不是右边的y赋值给x，x赋值给y
[x,y]=[y,x];
console.log(x,y); //2 1
```

## 对象解构赋值

1.模式匹配，属性名相同的完成赋值，不需要按照顺序

```javascript
// 1.模式匹配，属性名相同的完成赋值，不需要按照顺序
//简写形式
const {age,name}={name:"alex",age:18};
console.log(age,name); //18 "alex"
// 完整形式
const {age:age1,name:name1}={name:"alex",age:18};
console.log(name1,age1); //alex 18
```

2.对象解构赋值的注意事项

```javascript
// 1.默认值的生效条件
// 对象的数值值严格等于undefined时，对应的默认值才会生效
const {name } = { name: "alex"};
console.log(name); //"alex"
// 注意默认值的赋值是用等号，而不是用冒号
const {name1="billie",age=0}={};
console.log(name1);
// 2.如果默认值是表达式，默认值表达式是惰性求值的

// 3.将一个已经声明的变量用于解构赋值
let x=1;
// {x}={x:3}; 会报错，和解决箭头函数的单行语句的返回值是对象一样，在整个外层加上括号
({x}={x:3});
console.log(x); //3

// 4.可以取到继承的属性
const {toString}={};
console.log(toString);// 并没有输出undefined
// toString的属性继承自Object
console.log(Object.prototype);
const {a}={};
console.log(a); //undefined
```

3.对象解构赋值的应用

```jsx
// 函数参数的解构赋值
//const info=user=>console.log(user.name,user.age);
const info=({age,name})=>console.log(age,name);
info({name:"hh",age:12});
```

## 其他数组类型的解构赋值

1.字符串的解构赋值

```jsx
//数组形式的解构赋值
const [a,b,,,c]="hello";
console.log(a,b,c); //h e o
// 对象形式解构赋值
const {0:x,1:y,length}="hello";
console.log(x,y,length); //h e 5
```

2.数值和布尔值的解构赋值(只能按照对象形式解构赋值), 会自动将右侧的数值或布尔值转换为对象

```jsx
const {aa,toString}=123;
console.log(aa,toString); //toString属性是继承而来的
```

3.undefined和null的解构赋值会报错

```jsx
// 由于undefined和null，无法转化为对象，所以对它们进行解构赋值都会报错
const {ff}=undefined;
console.log(ff); //报错
```



# 简介表示

## 属性和方法的简洁表示

### 1.属性的简洁表示

当键名和变量名(常量名)一样的时候，可以只写一个。

```javascript
const age=19;
const person1={
    // age:age, 直接写age
    age,
}        
console.log(person1.age);
```

### 2.方法的简洁表示

```javascript
const person2={
    // speak:function(){}
    speak(){}
}
```

## 方括号语法

```js
// 1.方括号语法
const prop="age";
const person={};
person[prop]=18; // {age:18}
// ES6新增如下
const person1={
    [prop]:19
}

// 2.方括号可以放[值或者计算可以得到的值(表达式)]
const prop1="age";
const func=()=>'gender';
const person2={
    [prop1]:18,
    [func()]:'female'
}
// {age: 18, gender: "female"}

// 3.方括号语法和点语法的区别
// 点语法是方括号语法的特殊形式
const person3=[];
// perosn.age 等价于 person['age']
```

# 函数默认参数值

**函数默认参数**允许在没有值或`undefined`被传入时使用默认形参。只有在①不传参数 ②明确传递undefined作为参数 这两种情况下,默认值才会生效。

### 1.基本概念

```js
// 1.函数参数的默认值
function multiply(a,b=1){
    return a*b;
}
console.log(multiply(3) ) //3

// 2.默认值的生效
// ①不传参数 ②明确传递undefined作为参数 只有这两种情况下,默认值才会生效
console.log(multiply(2,undefined)); //2
console.log(multiply(2,"")); //2*"" 最后结果会被隐形转换为0

// 3.默认值表达式
// 如果默认值是表达式，默认值表达式是惰性求值的


// 4.设置默认值的小技巧
// 函数参数的默认值最好从参数列表的右边开始设置。

const multiply2=(x=1,y)=>x*y;
// 此时如果想要使用第一个默认值，必须明确传递undefined
console.log(multiply(undefined,2))
```

### 2.函数默认参数的应用

```js
// 1.接收多参数，如果按照默认写法，传参的时候需要记住参数顺序
const user=(name="zhangsan",age=12,gender="female")=>
    console.log(name,age,gender);
user("alex",18,'male');

// 2. 法一：接收一个对象作为参数
const user1=option=>console.log(
    option.name,option.age,option.gender);
user1({
    name:'alex',
    age:18,
    gender:"male"
});

// 3.法二：解构赋值的默认值
const user2=({name="zhangsan",age=12,gender="female"})=>
console.log(name,age,gender);

user2({name:"alex"});
// 但是不能什么都不传，如果什么都不传就相当于传了undefined
// 而无法对undefined进行解构赋值
// user2();

// 4.法三：函数参数的默认值
// 把{name="zhangsan",age=12,gender="female"}看为option
// option的默认参数为{}，当无不传入参数或者传入undefined的时候
// option={} 会把空对象赋值给option，也就是下面一行的解构赋值
// {name="zhangsan",age=12,gender="female"}={}

const user3=({name="zhangsan",age=12,gender="female"}={})=>
console.log(name,age,gender);
user3(); //此时不会报错
```

# 剩余参数与展开语法

## 剩余参数

### 剩余参数定义

**剩余参数**语法允许我们将一个不定数量的参数表示为一个数组。

```js
// 1.认识剩余参数
// 当不知道参数有多少个，可以用省略号代替，省略号后面接参数名
const add1=(x,y,z,...args)=>{};

// 2.剩余参数的本质，剩余参数是一个数组，如果没有值则是空数组
const add2=(x,y,z,...args)=>{
    console.log(x,y,args);
};
add2(1); //1 undefined []
add2(1,2,3,4,5,6);  //1 2 [4, 5, 6]

// 3.箭头函数与剩余参数
// 箭头函数的参数部分即使只有一个剩余参数，也不能省略括号
const add3=(...args)=>{};

// 4.使用剩余参数替代arguments获取实际参数
const add4=function(){
    // 记住箭头函数没有arguments，因为它没有this
    // arguments是类数组，而剩余参数是个数组
    console.log(arguments);
}
add4(1,2);

const add5=(...args)=>{
    console.log(args);
}
// 5.剩余参数的位置
// 剩余参数只能作为最后一个参数
```

### 剩余参数应用

```js
// 1.add函数
const add=(...args)=>{
    let sum=0;
    for(let i=0 ;i<args.length;i++){
        sum+=args[i];
    }
    return sum;
};
console.log(add(1,2,3,4));
// 使用reduce方法
const add_reduce=(...args)=>{
    return args.reduce((total,currentValue)=>{ return total+currentValue});
};
console.log(add_reduce(1,2,3,4,5,9));


// 2.与解构赋值结合使用
// 当剩余参数不是作为函数的参数时，剩余参数叫做剩余元素(Rest element)

// 剩余元素+数组解构赋值
const [num,...args]=[1,2,3,4];
console.log(num,args); //1 [2, 3, 4]

// 剩余参数+数组解构赋值+箭头函数
const func=([num,...args])=>{};
func([1,2,3]);

// 剩余元素+对象解构赋值
// 此时剩余元素为数组
const {x,y,...z}={x:1,b:2,y:3,d:4};
console.log(x,y,z); //1 3 {b: 2, d: 4}

// 剩余参数+对象解构赋值+箭头函数
const fun=({m,n,...o})=>{
    console.log(m,n,o);
};
fun({m:1,n:3,p:3,z:8});
```

## 展开语法

**展开语法(Spread syntax),** 可以在函数调用/数组构造时, 将数组表达式或者string在语法层面展开。

### 基本概念

```js
// 1.展开语法
console.log(Math.min(...[3,1,2])); // 相当于Math.min(3,1,2);

// 2.展开语法和剩余参数的区别
// 展开语法[3,1,2]->3,1,2
// 剩余参数 (3,1,2)->[3,1,2]

// 剩余参数
const add=(...args)=>{
    // 展开语法
    console.log(...args);
};
add(1,2,3);
```

### 展开语法的应用

```js
// 1.复制数组
const a = [1, 2, 3];
const b = a;
console.log(a === b); //true 引用

const c = [...a]; //等价于const c=[1,2,3];
console.log(a === c); //false

// 2.合并数组
const m=[1,2];
const n=[3,4,5];
const i=[7,...m,...n]; 
console.log(i); //[7,1, 2, 3, 4, 5]

// 3.字符串转数组
console.log(..."app"); // console.log("a","b","c");
console.log([..."apple"]); // ["a", "p", "p", "l", "e"]

// 4.常见的类数组转数组
// arguments
function func(){
    console.log(...arguments); //console.log(1,2,3);
    console.log([...arguments]);
}
func(1,2,3);
// NodeList
console.log([...document.querySelectorAll("p")]);
```

### 对象展开

对象的展开就是相当于把对象的所有属性罗列出来。

```js
// 1.展开对象
// 对象的展开就是相当于把对象的所有属性罗列出来
// 对象必须在{}中展开，不能直接展开
const apple={
    color:"red",
    taste:"sweet"
}
console.log({...apple}); //{color:"red",taste:"sweet"}

// 2.合并对象
//新对象拥有全部的属性，相同属性，后者会覆盖前者 
const banana={
    color:"yellow",
    category:"fruit"
}
console.log({...apple,...banana});
// {color: "yellow", taste: "sweet", category: "fruit"}
```

### 对象展开的注意事项

```js
// 1.空对象的展开
// 如果展开空对象，是没有任何效果
// 对象的展开，相当于把所有对象的属性罗列出来
console.log({...{}}); //{}

// 2.非对象的展开
// 如果展开的不是对象，则自动会将其转为对象，再将其罗列出来
console.log({...1}); //{} 1转为对象后，并没有属性罗列
console.log({...null}); //{}
console.log({..."alex"}); //{0: "a", 1: "l", 2: "e", 3: "x"}
console.log({...[1,2]}); //{0: 1, 1: 2}


// 3.含对象属性的对象的
// 对象属性会继续被展开
const apple={
    feature:{
        taste:"甜"
    }
};
const pen={
    feature:{
        color:"black"
    },
    use:"写字"
}
console.log({...apple}); //{feature: {taste:'甜'}}
console.log({...apple,...pen}); //{feature: {color:'black'}, use: "写字"}
```

### 对象展开的应用

```js
// 1.复制对象
const a={x:1,y:2};
const b={...a};
console.log(a===b); //false

// 2.用户参数和默认参数
// 法一： 解构赋值+函数默认参数
const user1=({name="zhangsan",age=0,gender="male"}={})=>{
    console.log(name,age,gender);
};
// 法二: 对象展开 +解构赋值
const user2=userParm=>{
    const defaultParm={name:"zhangsan",age:0,gender:"male"};
    // 合并默认参数和用户参数，还可以对结果进行解构
    const {name,age,gender}={...defaultParm,...userParm};
    console.log(name,age,gender);
}
```

# Set与Map

## Set

Set是一系列**无序、没有重复值**的数据集合。 Set 对象允许你存储任何类型的**唯一值**，无论是原始值或者是对象引用。

```js
//创建Set
const s=new Set();
```

### Set实例的属性和方法

#### 1.add方法

在`Set`对象尾部添加一个元素，返回该`Set`对象。

```js
//创建Set
const s=new Set();
s.add(1);
s.add(3);
s.add(4).add(5);  //Set(4) {1, 3, 4, 5}
```

#### 2.has方法

返回一个布尔值，表示该值在`Set`中存在与否。

```js
console.log(s.has(1)); //true
```

#### 3.delete 方法

移除`Set`中指定的元素。

```js
s.delete(1); //删除元素1
```

#### 4.clear方法

移除`Set`对象内的所有元素。

```js
s.clear();
```

#### 5.forEach()方法

`forEach` 方法会根据集合中元素的插入顺序，依次执行提供的回调函数。

```js
// 按照成员添加进集合的顺序遍历的
// 回调函数后面的参数是用来改变上下文的
s.forEach(function(value,key,set){
    // 在set中，value与key是等价的
    console.log(value,key,set);
},document);

```

### 6.size属性

判断Set实例中有多少元素。

### Set的构造函数

```js
// 1.数组
const s =new Set([1,2,3,1]);
console.log(s);  //Set(3) {1, 2, 3}

// 2.字符串、arguments、NodeList、Set
//字符串做参数
console.log(new Set("hi")) //Set(2) {"h", "i"}

//arguments做参数
function fun(){
    console.log(new Set(arguments));
}
fun(1,2,1);
//NodeList做参数
console.log(new Set(document.querySelectorAll('p'))); 
//Set(3) {p, p, p}

// Set实例做参数
console.log(new Set(s));
//Set(3) {1, 2, 3}

// 和原来完全相同，但不等于原来的，相当于复制。
console.log(new Set(s)===s); 
//false
```

## Set重复判定方式

Set 对重复值的判断基本遵循严格相等(===)， 但是对于NaN的判断与===不同，Set中NaN等于NaN。

```js
// Set实例会把2个NaN看做相等的元素
console.log(NaN===NaN ); //false
console.log(new Set([NaN,2,NaN]));
//Set(2) {NaN, 2}


const s=new Set();
s.add({}).add({});
console.log(s); //此时s中有2个空对象
```

## Set应用

```js
// 1.数组或字符串去重时
//Set(3) {1, 2, 3}
const s=new Set([1,2,2,3,2,1]);
console.log(s);

// 再将Set实例转换为数组
// ①使用forEach
let arr=[];
s.forEach((value)=>arr.push(value));
// ②使用展开语法
console.log([...s]);

// 2.字符串去重
const s1=new Set('abbacd');
// 将s1转为数组后，再用数组的join方法转为字符串
console.log([...s1].join(""));

// 3.存放dom元素
const s2=new Set(document.querySelectorAll("p"));
// 使用forEach改变p标签文本颜色
s2.forEach((elem)=>elem.style.color='red');
```



## Map

### 定义

**`Map`** 对象保存键值对，并且能够记住键的原始插入顺序。任何值(对象或者[原始值](https://developer.mozilla.org/zh-CN/docs/Glossary/Primitive)) 都可以作为一个键或一个值。

```js
// 1.Map和对象本质上都是键值对的集合
// 对象
const person={
    name:"alex",
    age:19
}
// Map
const m=new Map();
m.set('name','alex');
m.set('age',18);
console.log(m);

// 2.Map和对象的区别
// 对象一般使用字符串当做键
const obj={
    name:'alex'
}

// Map的键可以为基本数据类型，也可以为引用数据类型。
// 基本数据类型：数字、字符串、布尔值、undefined、null
// 引用数据类型：对象、数组、Set、Map、函数等
const mm=new Map();
mm.set(true,'true');
mm.set({},'object');
mm.set(new Set([1,2]),'set');
console.log(mm);
// {true => "true", {…} => "object", Set(2) => "set"}
```

### 方法和属性

```js
// 1.set方法
// 使用set添加的新成员，键如果已经存在，后添加的键值对覆盖已有的
const m=new Map();
m.set("age",18).set(true,"true").set("age",20);
console.log(m);

// 2.get方法，用于获取指定成员
console.log(m.get('age')); //20
console.log(m.get(true)); //true

// 3.has方法 用于判断是否有指定的键
console.log(m.has('age'));

// 4.delete方法
// 删除不存在的成员，什么都不会发生，也不会报错
m.delete('age');
console.log(m);

// 5.clear方法 删除所有的成员
m.clear();

// 6.forEach()方法
const mm=new Map();
mm.set("age",12).set("gender","male").set("name","xiaoming");
mm.forEach((value,index,map)=>console.log(value,index,map==mm));
// 12 "age"  true
// male gender true
// xiaoming name true

// 7.size属性
console.log(mm.size); //3
```

### Map构造函数的参数

```js
// 1.只能传二维数组，必须体现键和值
const m1=new Map([["name","alex"],["age",18]]);
console.log(m1);
// Map(2) {"name" => "alex", "age" => 18}

// 2.Set, Map等
// Set中也必须体现键和值
const s=new Set([["gender","male"],["name","xiaoming"]]);
const m2=new Map(s);
console.log(m2); 
//Map(2) {"gender" => "male", "name" => "xiaoming"}

const m3=new Map(m1);
console.log(m3, m3==m1);
//Map(2) {"name" => "alex", "age" => 18} false
```

### Map注意事项

```js
// 1.Map中判断键名是否相同
// 基本遵循严格相等(===)
// 例外就是Map中NaN等于NaN

const m1=new Map();
m1.set(NaN,1).set(NaN,2);
console.log(m1); //Map(1) {NaN => 2}

// 2.什么时候使用Map什么使用对象
// 2.1如果只需要Key->Value结构
// 2.2或者需要除了字符串以为的值做键，使用Map更合适
// 只有模拟现实世界的实体的时候才会使用对象
```

```html
<p>1</p>
<p>2</p>
<p>3</p>
<script>
const [p1, p2, p3] = document.querySelectorAll('p');
// 二维数组做Map构造函数的参数
const m = new Map([
    [p1, { color: "red", backgroundColor: "yellow", fontSize: "20px" }],
    [p2, { color: "pink", backgroundColor: "orange", fontSize: "20px" }],
    [p3, { color: "green", backgroundColor: "blue", fontSize: "20px" }]
]);
m.forEach((value, key) => {
    for(const p in value){
        // 有点难理解，多想想还是能理解的
        key.style[p]=value[p];
    }
});
</script>
```

SeT/Map总结

![image-20210610175416433](https://i.loli.net/2021/06/10/xEzm7YcGRnAQhOs.png)

![image-20210610175506099](https://i.loli.net/2021/06/10/E1XHQBYURp8IgjC.png)

![image-20210610175655315](https://i.loli.net/2021/06/10/JNXSG9yTZjbPFdK.png)

![image-20210610175710050](https://i.loli.net/2021/06/10/aXh4TLvYSuf1qQE.png)

![image-20210610175745097](https://i.loli.net/2021/06/10/FOBbacWuGtU1I23.png)

![image-20210610175848667](https://i.loli.net/2021/06/10/drQIFyD9Gmx2YJ7.png)

# Iterator

![image-20210610180620847](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210610180623.png)

### 定义

Iterator是如下的过程：Symbol.iterator(可遍历对象的生成方法)->it(可遍历对象)->next()->next()->(知道done为true)

```js
// 数组的.__proto__原型中有Symbol.iterator这个方法
// 而这个方法不符合命名规范，因此用方括号来调用。

// Symbol.iterator 可遍历对象的生成方法
// it：可遍历对象 (可迭代对象)
const it = [1, 2][Symbol.iterator]();
console.log(it); //Array Iterator {}

// value表示值，done表示遍历还没有完成
console.log(it.next());//{value: 1, done: false}
console.log(it.next()); //{value: 2, done: false}
console.log(it.next()); //{value: undefined, done: true}


```

为什么需要Iterator遍历器

遍历数组： for循环、forEach方法 ，遍历对象：for in循环。Iterator 遍历器是一个统一的遍历方式

### for of 用法

for of将下面Iterator过程封装起来。

```js
const arr=[1,2,3];
const it =arr[Symbol.iterator]();
let next=it.next();
while(!next.done){
    console.log(next.value);
    next=it.next();
}
```

for of循环只会遍历出那些done为false时对应的value值

```js
for(let i of arr){
    console.log(i);
}
```

for of可以和break、continue一起使用

```js
const arr2=[4,5,6,7,8];
for (let i of arr2){
    if(i===7){
        break;
    }
    console.log(i);
}
```

在for of循环中获取索引值

```js
// keys()得到的是索引的可遍历对象，可以遍历出索引值
const arr3=['a','b','c','d'];
for(let keys of arr3.keys()){
    console.log(keys); //
}
// values()得到的是值的可遍历对象，可以遍历出值
for(let value of arr3.values()){
    console.log(value); // a b c d
}
// entries()可以得到索引和值组成的数组的可遍历对象
for(let entries of arr3.entries()){
    console.log(entries);
}
// 结合解构赋值
for(let [index, value] of arr3.entries()){
    console.log(index,value);
}
```

### 可遍历

只要有Symbol.iterator方法，并且这个方法可以生成可遍历对象，就是可遍历的。

只要可遍历，就可以使用for...of循环来统一遍历。

#### 原生可遍历

数组、字符串、Set、Map、arguments、NodeList这些原生可遍历。

```js
for(const i of [1,2,3]){
    console.log(i);
} // 1 2 3

for(const i of new Set([4,5,6])){
    console.log(i);
} // 4 5 6

for(const i of document.querySelectorAll('p')){
    console.log(i);
    i.style.color='red';
}
```

#### 非原生可遍历

没有Symbol.iterator属性的，可以为它手动添加该属性就可以使用for...of循环了。

1.一般的对象

```js
const person ={age:18,sex:'male'};
// 给一般的对象手动添加Symbol.iterator属性
person[Symbol.iterator]=()=>{
    let index=0;
    return {
        next(){
            index++;
            if(index===1){
                return{
                    value:person.age,
                    done:false
                }
            }else if(index===2){
                return{
                    value:person.sex,
                    done:false
                }
            }else{
                return{
                    value:undefined,
                    done:true
                }
            }
        }
    }
};
for(let item of person){
    console.log(item);
}
```

2.有length和索引值的对象

```js
const obj={
    0:'alex',
    1:'male',
    length:2
}
obj[Symbol.iterator]=()=>{
    let index=0;
    return {
        next(){
            let value,done;
            if(index<obj.length){
                value=obj[index];
                done=false
            }else{
                done=true;
            }
            index++;
            return{
                value,
                done
            }
        }
    }
}

// 或者直接使用数组原型链的Symbol.iterator属性
obj[Symbol.iterator]=Array.prototype[Symbol.iterator];

for(let i of obj){
    console.log(i);
}
```

### 使用Iterator的场合

 1.数组的展开运算符

只要是原生可遍历的，就可以使用数组的展开运算。数组、字符串、Set、Map、arguments、NodeList这些原生可遍历。

```js
console.log(...[1,2,3]); //1 2 3
console.log(..."str"); // s t r
console.log(...new Set([1,2,3]));
```

2.数组的解构赋值

只要是原生可遍历的，就可以使用数组的进行解构赋值。因为可以在解构赋值前，进行展开运算让其变为数组。

```js

const [a,b]=[1,2];
// 在解构赋值前，"hi"进行了展开运算 [..."hi"]
const [c,d]="hi";
const [e,f]=new Set([7,8]);
console.log(a,b,c,d,e,f);

```

### Iterator总结

![image-20210610222129343](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210610222130.png)

![image-20210610222156497](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210610222157.png)

![image-20210610222304196](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210610222305.png)

![image-20210610220808524](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210610222325.png)



# ES6新增方法

## 字符串的新增方法

### includes()

**`includes()`** 方法用于判断一个字符串是否包含在另一个字符串中，根据情况返回 true 或 false。

```js
// 1.基本用法
console.log('abc'.includes('a')); //true
console.log('abc'.includes('ab')); //true
console.log('abc'.includes('ac')); //false

// 2.第二个参数
// 表示开始搜索的位置，默认是0
console.log('abc'.includes('a',1));//false
```

### padStart()和padEnd()

**`padStart()`** 方法用另一个字符串填充当前字符串(如果需要的话，会重复多次)，以便产生的字符串达到给定的长度。从当前字符串的左侧开始填充。

> ```js
> str.padStart(targetLength [, padString])
> ```

`targetLength`

当前字符串需要填充到的目标长度。**如果这个数值小于当前字符串的长度，则返回当前字符串本身。**

`padString` 可选

填充字符串。如果字符串太长，使填充后的字符串长度超过了目标长度，则只保留最左侧的部分，其他部分会被截断。此参数的默认值为 " "（空格）。

```js
'abc'.padStart(10);         // "       abc"
'abc'.padStart(10, "foo");  // "foofoofabc"
'abc'.padStart(6,"123465"); // "123abc"
'abc'.padStart(8, "0");     // "00000abc"
'abc'.padStart(1);          // "abc"
```

**`padEnd()`** 方法会用一个字符串填充当前字符串（如果需要的话则重复填充），返回填充后达到指定长度的字符串。从当前字符串的末尾（右侧）开始填充。

### trimStart()和trimEnd()

**`trimStart()`** 方法从字符串的开头删除空格。`trimEnd() `方法从一个字符串的末端移除空白字符。**`trim()`** 方法会从一个字符串的两端删除空白字符。

`trimLeft()` 是`trimStart()`的别名。`trimRight()` 是`trimEnd() `的别名。

```js
const s="   abc  ";
console.log(s.trimStart()); //"abc  "
console.log(s.trimEnd()); //"   abc"
console.log(s.trim()); //"abc"
console.log(s); //"   abc  "
```

## 数组新增方法

### includes()

`includes()` 方法用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。

> ```js
> arr.includes(valueToFind[, fromIndex])
> ```

`valueToFind`

需要查找的元素值。**Note:** 使用 `includes()`比较字符串和字符时是区分大小写。

`fromIndex` 可选

从`fromIndex` 索引处开始查找 `valueToFind`。如果为负值，则按升序从 `array.length + fromIndex` 的索引开始搜 。如果计算出的索引小于 0，则整个数组都会被搜索。默认为 0。

```js
[1, 2, 3].includes(2);     // true
[1, 2, 3].includes(4);     // false
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true
[1, 2, NaN].includes(NaN); // true
```

### Array.from()

`Array.from()` 方法从一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。

> ```js
> Array.from(arrayLike[, mapFn[, thisArg]])
> ```

`arrayLike`

想要转换成数组的伪数组对象或可迭代对象。

`mapFn` 可选

如果指定了该参数，新数组中的每个元素会执行该回调函数。

`thisArg` 可选

可选参数，执行回调函数 `mapFn` 时 `this` 对象。

```js
//从 String 生成数组
Array.from('foo');
// [ "f", "o", "o" ]

//从Set生成数组
const set = new Set(['foo', 'bar', 'baz', 'foo']);
Array.from(set);
// [ "foo", "bar", "baz" ]
[...new Set([1,2,3])]; //使用展开语法会更方便

//从 Map 生成数组
const map = new Map([[1, 2], [2, 4], [4, 8]]);
Array.from(map);
// [[1, 2], [2, 4], [4, 8]]

const mapper = new Map([['1', 'a'], ['2', 'b']]);
Array.from(mapper.values());
// ['a', 'b'];

Array.from(mapper.keys());
// ['1', '2'];
```

拥有length的任意对象都可以通过Array.from()转换为数组。

```js
const obj={length:2};
console.log(Array.from(obj)); 
// [undefined, undefined]

const obj1={length:2,0:"liu",1:"jiaqi",3:"haha"};
console.log(Array.from(obj1)); //["liu", "jiaqi"]
```

在Array.from()中使用箭头函数

作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组中。

```js
// 在Array.from()中使用箭头函数
// 作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组中。
console.log([1,2,3].map((value)=>value*2)); //2 4 6

console.log(Array.from({length:4},(value,index)=>value=index*2));
// 0 2 4 6
```

### find()和findIndex()

 `find()` 方法返回数组中满足提供的测试函数的**第一个元素**的值，否则返回undefined。

`findIndex()`方法返回数组中满足提供的测试函数的第一个元素的**索引**。若没有找到对应元素则返回-1。

> ```js
> arr.find(callback[, thisArg])
> ```

`callback`在数组每一项上执行的函数，接收 3 个参数：

- `element`当前遍历到的元素。
- `index`可选 当前遍历到的索引。
- `array`可选 数组本身。

`thisArg`可选,执行回调时用作`this` 的对象。

```js
//返回找到的第一个质数
function isPrime(element, index, array) {
  var start = 2;
  while (start <= Math.sqrt(element)) {
    if (element % start++ < 1) {
      return false;
    }
  }
  return element > 1;
}

console.log([4, 6, 8, 12].find(isPrime)); // undefined, not found
console.log([4, 5, 8, 12].find(isPrime)); // 5

```

```js
//用对象的属性查找数组里的对象
var inventory = [
    {name: 'apples', quantity: 2},
    {name: 'bananas', quantity: 0},
    {name: 'cherries', quantity: 5}
];

function findCherries(fruit) {
    return fruit.name === 'cherries';
}

console.log(inventory.find(findCherries)); // { name: 'cherries', quantity: 5 }
```



## 对象的新增方法

### Object.assign()

#### 定义

`Object.assign()` 方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。

如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。后面的源对象的属性将类似地覆盖前面的源对象的属性。

> ```js
> Object.assign(target, ...sources)
> ```

```js
const apple = {
    color: "red",
    taste: "sweet"
}

const banana = {
    color: "yellow",
    category: "fruit"
}
// Object.assign直接合并到了第一个对象中，返回的就是合并后的对象
console.log(Object.assign(apple,banana));
console.log(Object.assign(apple,banana)===apple); 
//因为此时的apple 已经被改变了 true

// 对象是引用类型的，Object.assign会直接修改第一个对象。
// 如果想要原来的对象不被修改，可以第一个参数放空对象
console.log(Object.assign({},apple,banana));
```

#### 注意事项

```js
// Object.assign(目标对象，源对象);
// 1.基本数据类型作为源对象
// 与对象的展开类似，先转换为对象，再合并
console.log(Object.assign({},undefined)); //{}
console.log(Object.assign({},null)); //{}
console.log(Object.assign({},22)); //{}
console.log(Object.assign({},22)); //{}
console.log(Object.assign({},"str")); //{0: "s", 1: "t", 2: "r"}

// 2.同名属性的替换
// 后面的属性直接覆盖前面的属性
const apple = {
    color: ["蓝色","紫色"],
    taste: "sweet"
}
const banana = {
    color: ["红色","黄色"],
    category: "fruit"
}
console.log(Object.assign(apple,banana)); //{color:["红色", "黄色"], taste: "sweet", category: "fruit"}
```

#### 应用

```js
const user=useroptions=>{
    const defaults={
        name:"alex",
        age:0,
        gender:"male"
    }
    const options=Object.assign({},defaults,useroptions);
    console.log(options);
};
user();
```

### Object.keys()、Object.values()、Object.entries()

#### 用法

```js
const person={
    name:"Alex",
    age:18
}
console.log(Object.keys(person)); // ["name", "age"]
console.log(Object.values(person)); // ["Alex", 18]
console.log(Object.entries(person)); // [ ["name", "Alex"],["age", 18]]
```

#### 与数组的类似方法的区别

数组的keys(), values(), entries()等方法都是实例方法，返回的都是可遍历对象。对象的Object.keys(), Object.values(),Object.values()等方法返回的都是数组。

```js
// 2.与数组类似的方法的区别
console.log([1,2].keys()); // 返回可遍历对象
console.log([1,2].values()); // 返回可遍历对象
console.log([1,2].entries()); // 返回可遍历对象
```

#### for...of

```js
const person={
name:"Alex",
age:18
}

for (const [key,value] of Object.entries(person)){
    console.log(key,value)
}

// Object.keys(), Object.values(), Object.entires()
// 这三个方法并不能保证顺序，和for...in 一样无法保证顺序
```

## 新增方法总结

![image-20210611160231073](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210611160239.png)

![image-20210611160310273](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210611160311.png)

![image-20210611160444694](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210611160445.png)

![image-20210611160655803](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210611160657.png)

![image-20210611160733628](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210611160735.png)

![image-20210611160904401](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210611160905.png)



# Promise

### 定义

Promise 一般用来解决层层嵌套的回调函数(回调地狱callback hell)的问题。

### 基本用法

![image-20210611181350553](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210611181352.png)

```js
// 1.实例化构造函数生成实例对象
// Promise 解决的不是回调函数，而是用于解决回调地域的问题。
const p = new Promise((resolve, reject) => {
    resolve({ name: 'alex' });
    // reject(new Error("reason"));
});
console.log(p);
// 2.Promise的状态
// Promise的状态一旦完成变化，就不会再改变了 
// Promise有3种状态，一开始是pending(未完成),执行resolve,变成fulfilled(resolved)已成功, 执行reject，变成rejected，已失败
// 执行resolve();
// pending->fulfilled
// 执行reject();
// pending->rejected

// 3.then()方法
// 当Promise的状态变为fulfilled时，执行第一个then()方法
// 当Promise的状态变为rejected时，执行第二个then()方法
p.then((data) => {
    console.log("success", data);
}, (err) => {
    console.log("error",err);
});

// 4.resolve和reject函数的参数
// 执行resole或者reject函数所传的参数，可以被then中的回调函数接收
```

### then()

![image-20210611183459134](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210611183500.png)

![image-20210611183554286](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210611183555.png)

```js
// 1.then()什么时候执行
// pending->fulfilled时，执行then的第一个回调函数
// pending->rejected时，执行then的第二个回调函数

// 2.执行后的返回值
// then()执行后返回一个新的Promise对象，返回的新的Promise对象又可以继续调用then();
const p =new Promise((resolve,reject)=>{
    reject();
});
// 在then()的回调函数中，return后面的东西，会用Promise包装一下
// return undefined 等价于 return new Promise((resolve)=>{resolve(undefined);});
// then()执行后返回新的Promise对象，新的Promise对象调用then，默认会返回成功状态的Promise对象
// 如果想要返回失败状态的Promise对象，return的时候写完整。return new Promise((resolve,reject)=>{reject();})

p.then(
    ()=>{
        console.log('success1');
    },
    ()=>{
        console.log('error1');
        return 123;
    }
).then(
    (data)=>{
        console.log('success2',data);
    },
    ()=>{
        
        console.log('error2');
    }
);

// 3.then()方法返回的Promise对象的状态是如何改变的
```

### 使用Promise解决回调地狱

```js
// Promise 一般用来解决层层嵌套的回调函数(回调地狱callback hell)的问题
const move = (el, { x = 0, y = 0 } = {}, end = () => { }) => {
    el.style.transform = `translate3d(${x}px,${y}px,0)`;
    el.addEventListener(
        'transitionend', () => {
            end();
        }, false
    );
};
const box = document.getElementById("box");
// document.addEventListener('click',()=>{
//     move(box,{x:150},()=>{
//         move(box,{x:150,y:150},()=>{
//             move(box,{y:150},()=>{
//                 move(box,{x:0,y:0});
//             })
//         })
//     })
// },false);
const movePromise = (el, point) => {
    return new Promise((resolve) => {
        move(el, point, () => {
            resolve();
        })
    })
}

document.addEventListener('click', () => {
    movePromise(box, { x: 150 }).then(() => {
        return movePromise(box, { x: 150, y: 150 })
    }).then(() => {
        return movePromise(box, { x: 0, y: 150 })
    }).then(() => {
        return movePromise(box, { x: 0, y: 0 })
    });
}, false);
```

### catch()

```js
// 1.尽管then()方法的第一个回调函数可以传成功后执行的回调函数，第二个传失败后执行的回调函数
// 2.但为了更好地语义化，一般只在then()中传成功后执行的，catch()传reject()后执行的回调函数
// catch专门用来处理rejected的状态，catch的本质是then的特例
new Promise((resolve,reject)=>{
    // resolve(123);
    reject('reason');
}).then((data)=>{
    console.log(data);
}).catch((err)=>{
    console.log(err);
    // 同样会默认返回一个成功的Promise对象。
    // 如果想要返回一个错误的Promise对象
    // 可以throw一个错误
    throw new Error('errrrr');
});
// 3.catch()可以捕获前面的错误
// 一般总是Promise对象后面要跟着catch方法，这样可以处理Promise内部发生的错误
```

### Promise.resolve()和Promise.reject()

```js
// 1.Promise.resolve()
// 是成功状态Promise的一种简写形式
new Promise((resolve) => { resolve("foo"); });
// 简写
Promise.resolve("foo");

//① 参数：一般参数
Promise.resolve("foo").then(data => console.log(data));
//②当Promise.resolve()接收的是Promise对象时，直接返回的是这个Promise对象，什么也不做
const p1 = new Promise(resolve => {
    setTimeout(resolve, 1000, "我执行了");
});
Promise.resolve(p1).then(data => {
    console.log(data);
})
console.log(Promise.resolve(p1) === p1); //true
// 当resolve函数接收的是Promise对象时，后面的then会根据传递的Promise对象的状态变化决定执行哪一个回调
new Promise(resolve => resolve(p1)).then(data => { console.log(data) });

// ③具有then方法的对象，对象中的then()方法和new Promise()差不多
const thenable={
    // 和new Promise()里的参数差不多
    then(resolve, reject){
        console.log("thenable");
        resolve("123");
    }
}
Promise.resolve(thenable).then(data=>console.log(data),err=>console.log(err));

// 2.Promise.reject();
// 是失败状态的一种简写形式
new Promise((resolve,reject)=>{
    reject('reason');
});
Promise.reject('reason');
// 不管什么参数都会原封不动的向后传递，作为后续方法的参数
Promise.reject(p1).catch((err)=>{
    console.log(err);
});
```

### Promise.all()

```js
// Promise.all()用来关注多个Promise对象的状态变化
// 可以传入多个Promise实例，包装成一个新的Promise对象返回
const delay=ms=>{
    return new Promise(resolve=>{
        setTimeout(resolve,ms);
    })
};
const p1=delay(1000).then(()=>{
    console.log('p1 finished');
    // return Promise.reject("uifsf");
    return '我是p1';
});
const p2=delay(2000).then(()=>{
    console.log('p2 finished');
    return '我是p2';
});
// Promise.all()的状态变化与所有传入的Promise实例对象状态变化有关
// 所有状态都变成了resolved，最终的状态才会变为resolved
// 只要有一个变成了rejected，最终的状态才会变为rejected

// 要给Promise.all()中传入数组，或者任何可遍历的参数
const p=Promise.all([p1,p2]);
p.then(data=>{
    console.log(data);
},(err)=>{
    console.log(err);
});
```

### Promise.race()和Promise.allSettled()

```js
// 1.Promise.race()
const delay=ms=>{
    return new Promise(resolve=>{
        setTimeout(resolve,ms);
    });
};
const p1=delay(1000).then(()=>{
    console.log('p1 finished');
    return "我是p1";
});
const p2=delay(2000).then(()=>{
    console.log('p2 finised');
    return "我是p2";
});
// Promise.race()的状态取决于第一个完成的Promise实例对象
// 如果第一个完成的成功了，就是最终的成功
// 如果第一个完成的失败了，就是最终的失败
const racePromise=Promise.race([p1,p2]);
racePromise.then(data=>{
    console.log(data);
},(err)=>{
    console.log(err);
});
```

```js
// 2.Promise.allSetted()
const delay=ms=>{
    return new Promise(resolve=>{
        setTimeout(resolve,ms);
    });
};
const p1=delay(1000).then(()=>{
    console.log("p1 finished");
    return Promise.reject("fjsf");
    // return "我是p1";
});

const p2=delay(200).then(()=>{
    console.log("p2 finished");
    return "我是p2";
});

const allSettedPromise=Promise.allSettled([p1,p2]);
// Promise.allSetted()的状态与传入的Promise的状态无关
// 永远都是成功的，它只会忠实的记录各个Promise的表现

allSettedPromise.then((data)=>{
    console.log("succeed",data);
},(err)=>{
    console.log("fail",err);
});
```

### 注意事项

![image-20210612210200175](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210612210208.png)

```js
// 1.resolve和reject函数执行后的代码
// 推荐在调用resolve或reject函数的时候加上return，不再执行它们后面的代码
new Promise((resolve, reject) => {
    resolve(123);
    // reject("reason");
    console.log("hi"); //还可以执行
});

// 2.Promise.all/race/allSettled的参数
// 参数如果不是Promise数组，会将不是Promise的数组元素转变成Promise对象
// Promise.all([1,2,3]).then(datas=>{
//     console.log(datas);
// });
// 等价于
Promise.all([
    Promise.resolve(1),
    Promise.resolve(2),
    Promise.resolve(3)
]).then(data => {
    console.log(data);
}) //[1,2,3]

// 除了数组，任何可遍历的都可以作为数组
// 数组、字符串、Set、Map、NodeList、arguments
Promise.all(new Set([1, 2, 3])).then(data => {
    console.log(data);
}) //[1,2,3]

// 3.Promise.all/race/allsettled错误处理
// 错误既可以单独处理，也可以统一处理
// 一旦被处理，就不会再处理一遍
const delay = ms => {
    return new Promise(resolve => {
        setTimeout(resolve, ms);
    });
};
const p1 = delay(1000).then(data => {
    console.log("p1 finished");
    return "我是P1";
});
const p2 = delay(2000).then(data => {
    console.log("p2 finished");
    return Promise.reject('rej');

});

const allPromise = Promise.all([p1, p2]);
allPromise.then((datas) => {
    console.log(datas);
}).catch(err => {
    console.log(err);
});
```

Promise应用异步加载图片

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #img{
            width: 80%;
            padding: 10%;
        }
    </style>
</head>
<body>
    <img id="img" src="https://img.mukewang.com/szimg/5feb016d097497d905400304.jpg" alt="">
    <script>
        // 1.异步加载图片
        const loadImgAsync=url=>{
            return new Promise((resolve,reject)=>{
                // Image()函数将会创建一个新的HTMLImageElement实例。
                // 它的功能等价于 document.createElement('img')
                const img=new Image();
                // Promise函数中不用写具体过程，只需要决定用resolve还是reject即可
                // onload 属性是一个事件处理程序用于处理Window, XMLHttpRequest, <img> 等元素的加载事件，当资源已加载时被触发。     
                img.onload=()=>{
                    console.log("sb");
                    resolve(img);
                };
                // 当一项资源（如<img>或<script>）加载失败，加载资源的元素会触发一个Event接口的error事件，并执行该元素上的onerror()处理函数。
                img.onerror=()=>{
                    reject(new Error(`couldn't load image at ${url}`));
                }
                img.src=url;
            });
        }
        let url1='https://img4.mukewang.com/szimg/60b9864a09995aa605400304.png';
        const imgDom=document.getElementById('img');
        loadImgAsync(url1).then(img=>{
            console.log(img.src);
            setTimeout(()=>{
                imgDom.src=img.src;
            },1000);
        }).catch(err=>{
            console.log(err);
        });
    </script>
</body>
</html>
```

### 总结

![image-20210613141548090](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613141556.png)

![image-20210613141645235](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613141646.png)

![image-20210613141717621](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613141718.png)

![image-20210613141849863](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613141850.png)

![image-20210613142007368](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613142008.png)

![image-20210613142050967](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613142051.png)

![image-20210613142135558](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613142136.png)

![image-20210613142150152](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613142151.png)

![image-20210613142216303](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613142217.png)

![image-20210613163042772](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613163043.png)

![image-20210613163153293](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613163154.png)

![image-20210613163219353](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613163220.png)

![image-20210613163245239](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613163246.png)

![image-20210613163303168](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210613163304.png)

# class

### 定义

**class 声明**创建一个基于原型继承的具有给定名称的新类。

要注意类的写法，并没有圆括号直接跟上了花括号，方法和方法之间不需要用逗号分隔。

```js
// 类可以看做是对象的模板，用一个类可以创建出许多不同的对象
// 类名一般来说，首字母是大写的
class Person{
    // 实例化时必须执行构造方法，所以必须有构造方法，但是可以不写出来
    constructor(name,age){
        this.name=name;
        this.age=age;
        // 一般只在构造方法中定义属性，方法不在构造方法中定义
    }
    // 各个实例共用的方法
    speak(){
        console.log('speak');
    }
}
// 实例对象
const zs=new Person('张三',18);
const ls=new Person('ls',58);
console.log(zs.speak===ls.speak); //true
```

声明的Person类本质上是一个函数，和构造函数差不多。

```js
console.log(typeof Person); //function
console.log(Person.prototype.speak);
```

对比一下Person构造函数。

```js
// 构造函数
function Person1(name,age){
    this.name=name;
    this.age=age;
}
Person1.prototype.speak=function(){
    console.log('speak');
}
```

### 2种定义形式

```js
// 1.声明形式
class Person{
    constructor(){

    }
    speak(){}
}

// 2.表达式形式
const Person1=class{
    constructor(){

    }
};
```

### 立即执行的类

类也可以像立即执行的匿名函数一样立即执行，但要记得在类前面加上关键字`new`，否则会报错。

```js
// 立即执行的类
new (class{
    constructor(){
        console.log("hahah");
    }
})();

// 立即执行函数
// (function(){

// })();
```

### 实例属性、静态方法、静态属性

#### 1.实例属性

实例属性一般可以用作默认值，它不能用`var`、`let`、`const`等关键字声明。

```js
// 1.实例属性
class Person{
    //constructor外 不能用关键字声明属性，也不能用this
    // 一般用作默认值
    name="zhangsan"
    age=18;
    // 实例方法 方法是值为函数的特殊属性
    getAge=function(){
        return this.age;
    }
    constructor(name){
        this.name=name;
    }
}
const p=new Person('alex');
console.log(p.name,p.age);// alex 18
console.log(p.getAge()); //18
```

#### 2.静态方法

静态方法是类的方法，不需要实例化类就能够调用。用关键字static来声明静态方法。

```js
class Person{
    constructor(name,sex){
        this.name=name;
        this.sex=sex;
    }
    static speak(){
        console.log('haha...');
        console.log(this); //this指向Person类
    }
    speak(){
        console.log("awsl");
        console.log(this); //this指向实例对象
    }
}
// 调用类的方法 静态方法
Person.speak();  //haha...
const xm=new Person("xm",12);
xm.speak(); //awsl

//也可以把类的方法写在

```

# module

### 定义

模块是一个一个的局部作用域的代码块。模块系统可以解决①模块化的问题②消除全局变量③管理加载顺序。

### 例子一

一个模块即使没有导出，也可以将其导入。要注意在`script`标签中 加上 `type="module"`，导入后代码会执行一遍，多次导入也仅仅会执行一遍。

> exp1.html

```html
<script type="module">
import './module.js'  //18
</script>
```

> module.js

```js
const age=18;
console.log(age);
```

没有导出，直接导入就相当于写成:

```html
<script scr='./module.js' type='module'></script>
```

### 例子二

一个模块只能有一个export default。

> exp2.html

```html
<script type="module">
//可以随便取名
import age from 'moudle.js';
console.log(age); //18

</script>
```
> module.js

```js
const age=18;
// 一个模块只能有一个export default
export default age;
```

### 例子三
> exp3.html

```html
<script type="module">
    // import age from './module.js'; 
    // 上面的是export default对应的import

    // 法一：普通导入
    // 不能随意命名，需要和导出的名字一样
    // 因此无法导出匿名函数、类等
    import {age,gender} from './module.js';
    
    // 法二：导入时起别名
    import {func,userName as person} from './module.js';
    
    // 法三： 整体导入(会同时导入export和export default导出的)，obj为别名
    import * as obj from './module.js';
    console.log(age,gender,func,person);
    console.log(obj);

    // 法四：同时导入export和export default导出的参数
    // 注意export default导出的要写在前面
    import weight,{height} from './module.js';
    console.log(weight,height); //56 168
</script>
```

> module.js

```js
// 法一：export后面接声明或语句
export const age =18;

// 法二： export后接 {argument};
const gender="male";
export {gender}; //√
// export gender; ×

// 注意不能导出匿名函数或者类等
function fn(){}
class Name{}


// 法三：导出为别名
export {fn as func,Name as userName};


export default 56;
export const height=168;
```

### 注意事项

1.模块顶层的`this`指向

顶层是值不在for块级、function函数作用域中，直接在模块中的作用域。

在模块中，顶层的this是指向undefined的。

```js
if(typeof this!=='undefined'){
    // 如果this不为undefined，说明并不是用模块的方式来加载的
    throw new Error("没有以模块的形式导入");
}
```

2.import和import()

import关键字具有提升效果，会提升到整个模块的头部、率先执行。也即import执行的时候，其他代码还没有开始执行。因此import和export命令只能在模块的顶层，**不能放在在代码块中执行**。

```js

// 会直接报错！！！
if (PC){
    import 'pc.js';
}else if(mobile){
    import 'mobile.js';
}
```

而import()可以按条件导入，且会返回promise对象。

```js
if (PC){
    import('pc.js').then().catch();
}else if(mobile){
    import('mobile.js').then().catch();
}
```

3.先导入再导出的复合写法

```js
// 把age从别的模块导入，再又导出，相当于一个中转站
// export {age} from './module.js'; 
// 复合写法导出的，无法在当前模块使用
// 相当于下面的import和export语句，但是上面的复合写法age无法正常输出
import {age} from './module.js';
console.log(age); // 可以正常输出
export {age} from './module.js';
console.log(age); // 可以正常输出
```

### 总结

![image-20210615232937033](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210615232944.png)

![image-20210615233123778](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210615233124.png)

![image-20210615233157075](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210615233158.png)

![image-20210615233228936](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210615233229.png)

![image-20210615233313046](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210615233314.png)

![image-20210615233403786](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210615233404.png)

![image-20210615233451256](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210615233452.png)

![image-20210615233557251](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210615233558.png)

![image-20210615233651225](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210615233652.png)

# node入门

笔记内容源自：https://youtu.be/TlB_eWDSMt4

在powershell中输入`code .`会用VS Code打开当前文件夹。

![image-20210616170534238](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210616170542.png)

 首先进入到要执行js的文件夹，然后输入 node +要运行的程序名，即可运行js。

在node中，没有window和document对象，但在node中有其他的对象可以操作文件、操作系统、网络等。

![image-20210616171058446](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210616171059.png)

在浏览器中中运行JavaScript，可以通过window.setTimeout()来调用setTimeout全局函数。在node中，则可以通过global.setTimeout()来调用，变量和函数不会添加到global对象中。

```js
var message=''; 
//在浏览器中变量message会被添加到window对象中
//在node中变量message不会被添加到global对象中

console.log(global.message); //undefined
```

在node中任何文件都被视为模块(module)，在文件中定义的变量或函数的作用域限制在了该文件。如果需要使用这些私有变量或函数，需要明确地export它。

![image-20210616172237280](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210616172238.png)

任何node应用程序都至少有一个main 模块(文件)，我们称之为main module。

使用require()来加载模块，require函数的参数时是目标加载模块的路径。当导出不止一个参数时，require会返回一个从目标模块导出的对象。

> app.js

```js
const logger = require('./logger')
//{ log: [Function: log], url: 'http://mylogger.io/log' }
console.log(logger);

//调用另外一个模块的函数
logger.log("message")
```

> logger.js

```js
var  url='http://mylogger.io/log';

function log(message){
    // send an http request
    console.log(message);
}

//输出的名字可以自定义
module.exports.log=log;
module.exports.url=url;
```

最后，console.log(logger)会输出一个对象。

![image-20210617112257229](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210617112306.png)

如果只需要导出一个参数，导出时可以不自定义名字，这样require函数返回的不是导出模块的参数对象，而是返回的导出模块导出的变量名或常量名。

> app.js

```js
const log = require('./logger')

log("message");
```

> logger.js

```js
var  url='http://mylogger.io/log';

function log(message){
    // send an http request
    console.log(message);
}


module.exports=log;
```

# babel

babel官网： [https://babeljs.io/](https://babeljs.io/)。

主要用于将采用 ECMAScript 2015+ 语法编写的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中。

Babel本身可以编译ES6的大部分语法，比如let、 const、箭头函数、类。但是对于ES6新增的API，比如Set、Map、 Promise等全局对象都不能直接编译，需要借助其它的模块。Babel一般需要配合 Webpack来编译模块语法。

![image-20210618115716149](https://img-blog.csdnimg.cn/img_convert/c3b181a491acea93cf6d8ef6a58c848d.png)

## 初始化

要在项目目录文件下，在powershell中安装babel所需要的包。首先要`npm init`，初始化项目，回车后，会提示包名，注意包名不能是中文。

![image-20210618120620517](https://img-blog.csdnimg.cn/img_convert/833503aaf604e173ec462d091e6c9f2a.png)
![image-20210618120938972](https://img-blog.csdnimg.cn/img_convert/18f61389a04283749f3fe5fec2fd72fc.png)

一系列回车后，项目中会多了一个package.json的文件，执行`npm init`就是为了得到这个json文件。这个json文件会记录安装的其他的包。

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210618121541.png" alt="image-20210618121540287" style="zoom:33%;" />

## 安装babel

安装babel的命令：

```bash
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

`--save-dev`：`save`表示要把它们的信息写入 package.json, `dev`表示是开发模式下。

` @babel/core @babel/cli @babel/preset-env`表示三个一般都要安装的babel包。


> PS：只要有`package.json`，就可以直接在命名行中输入`npm install`安装所有的该json文件中提到的所有包。

## 配置babel
![image-20210618122619509](https://img-blog.csdnimg.cn/img_convert/f1c3c2ba0abdba32f6fbda1eda7926fe.png)
需要在根目录`package.json`文件中添加下面的代码。
```json
"scripts": {
  "build": "babel src -d dist"
}
```
![image-20210618123402148](https://img-blog.csdnimg.cn/img_convert/703d2d400cda8e83635ec81522c221bb.png)
`babel src -d lib`是`babel src --out-dir dist`的缩写，也就是从src目录输出到lib目录。

---

在项目的根目录中创建名为 `babel.config.json`的配置文件。

如果想要转换`let` 、`const`，必须要在`target`中写`ie:10`，因为目前除了ie不支持几乎所有版本的浏览器都支持。不写的话就代表不把ie10作为目标浏览器，就不会转换`const`、`let`、`箭头函数`等这些几年前就出来的语法。

```json
{
  "presets": [
    [
      "@babel/env",
      {
        "targets": {
            "ie":"10",
          "edge": "17",
          "firefox": "60",
          "chrome": "67",
          "safari": "11.1"
        },
        "useBuiltIns": "usage",
        "corejs": "3.6.5"
      }
    ]
  ]
}
```
当然，如果缺省`target`，直接写如下代码，默认会转换为ES5。

```json
{
  "presets": ["@babel/env"]
}
```

## 编译

```bash
npm run build
```

build就是`package.json`script中自定义的名字，通过npm run build开始执行。可以在项目中看到多了名为lib的文件夹，里面有babel编译后的js文件。

![image-20210618124246687](https://img-blog.csdnimg.cn/img_convert/a52e2d447d3daf8b9d66240b53e3bdad.png)

**最后，我走了很多很多弯路，因为我tm没有看官方文档**。要是我看看文档，我也就知道了`const`、`let`为什么没有转换。因为要定义target，要把ie也作为目标浏览器。

[中文官方文档](https://babel.docschina.org/docs/en/usage/)拜托我自己多看官方文档，再到处提问。

# Webpack

[webpack中文官网](https://www.webpackjs.com/)

webpack是静态模块打包器，当使用webpack处理应用程序时，会将这些模块打包成一个或多个文件。

它可以处理js/css/图片/字体/图标等文件，用于处理静态(本地)文件。

模块化是一种将系统分离成独立功能部分的方法，严格定义模块接口、模块间具有透明性。

## 概念

### 入口(entry)

**入口起点(entry point)** 指示 webpack 应该使用哪个模块，来作为构建其内部 [依赖图(dependency graph)](https://webpack.docschina.org/concepts/dependency-graph/) 的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

默认值是 `./src/index.js`，但你可以通过在 [webpack configuration](https://webpack.docschina.org/configuration) 中配置 `entry` 属性，来指定一个（或多个）不同的入口起点。例如：

> **webpack.config.js**

```js
module.exports = {
  entry: './path/to/my/entry/file.js',
};
```

#### 多个入口

用法：`entry: { <entryChunkName> string | [string] } | {}`

> **webpack.config.js**

```javascript
module.exports = {
  entry: {
    app: './src/app.js',
    adminApp: './src/adminApp.js',
  },
};
```

#### 描述入口的对象 

用于描述入口的对象。你可以使用如下属性：

- `dependOn`: 当前入口所依赖的入口。它们必须在该入口被加载前被加载。
- `filename`: 指定要输出的文件名称。
- `import`: 启动时需加载的模块。
- `library`: 指定 library 选项，为当前 entry 构建一个 library。
- `runtime`: 运行时 chunk 的名字。如果设置了，就会创建一个以这个名字命名的运行时 chunk，否则将使用现有的入口作为运行时。
- `publicPath`: 当该入口的输出文件在浏览器中被引用时，为它们指定一个公共 URL 地址。请查看 [output.publicPath](https://webpack.docschina.org/configuration/output/#outputpublicpath)。

`runtime` 和 `dependOn` 不应在同一个入口上同时使用，所以如下配置无效，并且会抛出错误：

> **webpack.config.js**

```javascript
module.exports = {
  entry: {
    a2: './a',
    b2: {
      runtime: 'x2',
      dependOn: 'a2',
      import: './b',
    },
  },
};
```

确保 `runtime` 不能指向已存在的入口名称，例如下面配置会抛出一个错误：

```javascript
module.exports = {
  entry: {
    a1: './a',
    b1: {
      runtime: 'a1',
      import: './b',
    },
  },
};
```

另外 `dependOn` 不能是循环引用的，下面的例子也会出现错误：

```javascript
module.exports = {
  entry: {
    a3: {
      import: './a',
      dependOn: 'b3',
    },
    b3: {
      import: './b',
      dependOn: 'a3',
    },
  },
};
```



### 输出(output)

**output** 属性告诉 webpack 在哪里输出它所创建的 *bundle*，以及如何命名这些文件。主要输出文件的默认值是 `./dist/main.js`，其他生成文件默认放置在 `./dist` 文件夹中。

注意，即使可以存在多个 `entry` 起点，但只能指定一个 `output` 配置。

你可以通过在配置中指定一个 `output` 字段，来配置这些处理过程：

```javascript
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    // path:绝对路径
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js',
  },
};
```

在上面的示例中，我们通过 `output.filename` 和 `output.path` 属性，来告诉 webpack bundle 的名称，以及我们想要 bundle 生成(emit)到哪里。在代码最上面导入的 path 模块是什么，它是一个 [Node.js 核心模块](https://nodejs.org/api/modules.html)，用于操作文件路径。

如果有多个入口文件，出口文件需要改名，否则就会覆盖。

```js
const path=require('path');

module.exports={
    entry:{
        "main":'./src/index.js',
        "app":'./src/module.js'
    },
    output:{
        // [name]表示入口的名字
        filename:'[name].bundle.js',
        path:path.resolve(__dirname,'dist'),  
        // 清除没有用到的文件
        clean:true
        
    },
    mode:'development',
};
```

### [loader](https://www.webpackjs.com/loaders/babel-loader/)

webpack 只能理解 JavaScript 和 JSON 文件，这是 webpack 开箱可用的自带能力。**loader** 让 webpack 能够去处理其他类型的文件，并将它们转换为有效 [模块](https://webpack.docschina.org/concepts/modules)，以供应用程序使用，以及被添加到依赖图中。

> ##### Warning
>
> 注意，loader 能够 `import` 导入任何类型的模块（例如 `.css` 文件），这是 webpack 特有的功能，其他打包程序或任务执行器的可能并不支持。我们认为这种语言扩展是很有必要的，因为这可以使开发人员创建出更准确的依赖关系图。

在更高层面，在 webpack 的配置中，**loader** 有两个属性：

1. `test` 属性，识别出哪些文件会被转换。
2. `use` 属性，定义出在进行转换时，应该使用哪个 loader。

> **webpack.config.js**

```javascript
const path = require('path');

module.exports = {
  output: {
    filename: 'my-first-webpack.bundle.js',
  },
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
};
```

以上配置中，对一个单独的 module 对象定义了 `rules` 属性，里面包含两个必须属性：`test` 和 `use`。这告诉 webpack 编译器(compiler) 如下信息：

> “嘿，webpack 编译器，当你碰到「在 `require()`/`import` 语句中被解析为 '.txt' 的路径」时，在你对它打包之前，先 **use(使用)** `raw-loader` 转换一下。”

> ##### Warning
>
> 重要的是要记住，在 webpack 配置中定义 rules 时，要定义在 `module.rules` 而不是 `rules` 中。为了使你便于理解，如果没有按照正确方式去做，webpack 会给出警告。

### [插件(plugin)](https://www.webpackjs.com/plugins/)

loader 用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。包括：打包优化，资源管理，注入环境变量。

想要使用一个插件，你只需要 `require()` 它，然后把它添加到 `plugins` 数组中。多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 `new` 操作符来创建一个插件实例。

> **webpack.config.js**

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件

module.exports = {
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],
};
```

在上面的示例中，`html-webpack-plugin` 为应用程序生成一个 HTML 文件，并自动注入所有生成的 bundle。

### 模式(mode)

通过选择 `development`, `production` 或 `none` 之中的一个，来设置 `mode` 参数，你可以启用 webpack 内置在相应环境下的优化。其默认值为 `production`。

```javascript
module.exports = {
  mode: 'production',
};
```

### 浏览器兼容性(browser compatibility) 

webpack 支持所有符合 [ES5 标准](https://kangax.github.io/compat-table/es5/) 的浏览器（不支持 IE8 及以下版本）。webpack 的 `import()` 和 `require.ensure()` 需要 `Promise`。如果你想要支持旧版本浏览器，在使用这些表达式之前，还需要 [提前加载 polyfill](https://webpack.docschina.org/guides/shimming/)。

## 入门

https://webpack.docschina.org/guides，用这个入门非常棒，讲解非常详细，内容也超级丰富，只练一遍是记不住的。

### 极简教程

1.首先要先在项目中初始化，`npm init -y`，会生成一个package.json文件，-y表示全部默认，省去回车过程。

2.在powershell中输入`npm install --save-dev webpack webpack-cli`来安装webpack。

3.项目根目录下创建一个名为`webpack.config.js`的配置文件，在这个配置文件中用module.exports来导出配置。

下面的代码的意思为入口为'./src/indec.js'，出口为'dist'文件夹，文件名为'bundle.js'。

```js
const path=require('path');

module.exports={
    entry:'./src/index.js',
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:"bundle.js"
    }
};
```

4.在`package.json`文件中的scrpits下添加如下代码：

```json
"scripts": {
  "build":"webpack"
}
```

5.在终端中输入`npm run build`，webpack即会开始编译，可以在dist目录下看到编译结果。

如果在webpack配置文件中(默认为webpack.config.js)中将模式改为**development**(默认为production)，编译后的文件会看起来更加清晰。

```diff
const path=require('path');

module.exports={
    entry:'./src/index.js',
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:"bundle.js"
    },
 +  mode:'development',
};
```

## [插件plugins](https://www.webpackjs.com/plugins/)

loader被用于帮助webpack处理各种模块，而插件则可用于执行范围更广的任务。

### [HtmlWebpackPlugin](https://www.webpackjs.com/plugins/html-webpack-plugin/)

如果我们更改了一个入口起点的名称，甚至添加了一个新的入口，会发生什么？会在构建时重新命名生成的 bundle，但是我们的 `index.html` 文件的`script`标签仍然引用旧的名称。让我们用 [`HtmlWebpackPlugin`](https://webpack.docschina.org/plugins/html-webpack-plugin) 来解决这个问题。

有了这个插件，就不用在`npm run build`改变了js的名字后手动去改变html中`script`标签引入的js路径。

#### 单入口配置

首先安装插件，并且调整 `webpack.config.js` 文件：

```bash
npm install --save-dev html-webpack-plugin
```

> **webpack.config.js**

```diff
 const path = require('path');
+ const HtmlWebpackPlugin = require('html-webpack-plugin');

 module.exports = {
   entry: {
     index: './src/index.js',
   },
+  plugins: [
+    new HtmlWebpackPlugin({
+      //指定一个html文件作为模板
+      template:"./index.html"
+    }),
+  ],
   output: {
     filename: '[name].bundle.js',
     path: path.resolve(__dirname, 'dist'),
   },
   mode:'development'
 };
```

虽然在 `dist/` 文件夹我们已经有了 `index.html` 这个文件，然而 `HtmlWebpackPlugin` 还是会默认生成它自己的 `index.html` 文件。也就是说，它会用新生成的 `index.html` 文件，替换我们的原有文件。

#### 多html配置

配置含有2个html和2个js的项目。

```diff
const path=require('path');
const HtmlWebpackPlugin=require('html-webpack-plugin');

module.exports={
    mode:'development',
    // 多入口
+    entry:{
+        index:'./src/index.js',
+        search:'./src/search.js'
+    },
    output:{
        filename:'[name].js',
        path:path.resolve(__dirname,"dist"),
    },
    // 多入口 有几个入口就实例化几次
+    plugins:[
+        new HtmlWebpackPlugin({
+            //指定一个html文件作为模板
+           template:"./index.html",
+            //多个html必须要命名，否则默认为index.html
+            //同名文件，后生成的文件会覆盖前面的
+            filename:'index.html',
+            //指定要引入的js文件，否则会引入所有的js文件
+            chunks:['index'] //写entry中的名字
+        }),
+        new HtmlWebpackPlugin({
+            template:"./search.html",
+            filename:'./search.html',
+            // 同时引入index和search两个文件
+            chunks:['index','search'],
+        })
+    ]
}
```

#### 其他功能

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

 module.exports = {
   entry: {
     index: './src/index.js',
   },
  plugins: [
   new HtmlWebpackPlugin({
     //指定一个html文件作为模板
     template:"./index.html",
+     minify:{
+         //删除index.html中的注释
+         removeComments:true,
+         // 删除index.html中的空格
+         collapseWhitespace:true,
+         //删除html标签属性值的双引号
+         removeAttributeQuotes:true
+     }
    }),
  ],
   output: {
     filename: '[name].js',
     path: path.resolve(__dirname, 'dist'),
   },
   mode:'development'
 };
```

## [loader](https://www.webpackjs.com/loaders/babel-loader/)

loader可以用webpack能够处理非JS文件(css、图片、字体等)的模块。

### 使用loader

在你的应用程序中，有两种使用 loader 的方式：

- [配置方式](https://webpack.docschina.org/concepts/loaders/#configuration)（推荐）：在 **webpack.config.js** 文件中指定 loader。
- [内联方式](https://webpack.docschina.org/concepts/loaders/#inline)：在每个 `import` 语句中显式指定 loader。

#### 配置方式

[`module.rules`](https://webpack.docschina.org/configuration/module/#modulerules) 允许你在 webpack 配置中指定多个 loader。 这种方式是展示 loader 的一种简明方式，并且有助于使代码变得简洁和易于维护。同时让你对各个 loader 有个全局概览：

loader **从右到左（或从下到上）**地取值(evaluate)/执行(execute)。在下面的示例中，从 sass-loader 开始执行，然后继续执行 css-loader，最后以 style-loader 为结束。查看 [loader 功能](https://webpack.docschina.org/concepts/loaders/#loader-features) 章节，了解有关 loader 顺序的更多信息。

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          // [style-loader](/loaders/style-loader)
          { loader: 'style-loader' },
          // [css-loader](/loaders/css-loader)
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          },
          // [sass-loader](/loaders/sass-loader)
          { loader: 'sass-loader' }
        ]
      }
    ]
  }
};
```

#### 内联方式(不推荐)

可以在 `import` 语句或任何 [与 "import" 方法同等的引用方式](https://webpack.docschina.org/api/module-methods) 中指定 loader。使用 `!` 将资源中的 loader 分开。每个部分都会相对于当前目录解析。

```js
import Styles from 'style-loader!css-loader?modules!./styles.css';
```

### 已废用file-loader

**v5 版本已废弃**file-loader: 请向 [`asset modules`](https://webpack.docschina.org/guides/asset-modules/) 迁移。资源模块(asset module)是一种模块类型，它允许使用资源文件（字体，图标等）而无需配置额外 loader。

在 webpack 5 之前，通常使用：

- [`raw-loader`](https://webpack.docschina.org/loaders/raw-loader/) 将文件导入为字符串
- [`url-loader`](https://webpack.docschina.org/loaders/url-loader/) 将文件作为 data URI 内联到 bundle 中
- [`file-loader`](https://webpack.docschina.org/loaders/file-loader/) 将文件发送到输出目录

资源模块类型(asset module type)，通过添加 4 种新的模块类型，来替换所有这些 loader：

- `asset/resource` 发送一个单独的文件并导出 URL。之前通过使用 `file-loader` 实现。
- `asset/inline` 导出一个资源的 data URI。之前通过使用 `url-loader` 实现。
- `asset/source` 导出资源的源代码。之前通过使用 `raw-loader` 实现。
- `asset` 在导出一个 data URI 和发送一个单独的文件之间自动选择。之前通过使用 `url-loader`，并且配置资源体积限制实现。

### babel-loader

先要安装babel，毕竟活是babel干的，webpack只是打包的。

```bash
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

接着安装babel-loader这个插件。

```bash
npm install --save-dev babel-loader
```

接下来，配置babel，在根目录下创建`babel.config.json`的文件，并写下如下代码。

```json

{
  "presets": ["@babel/env"]
}
```

下一步，便是在`webpack.config.js`文件中配置loader。

```diff
const path = require('path');
module.exports = {
    entry: {
        "index": './src/module.js'
    },
    output: {
        filename: '[name].bundle.js',
        path: path.resolve(__dirname, 'dist'),
    },
+    module: {
+       rules: [
+            {
+                test: /\.js$/,
+                // 排除node_modules里面的js文件
+                exclude: /node_modules/,
+                use: "babel-loader"
+            },
+        ],
+    },
    mode: 'development',
   
};
```

如果想要转换promise这些，需要安装babel的[垫片插件](https://babeljs.io/docs/en/babel-polyfill)。

第一步要先安装`core-js`。

![image-20210620124715403](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210620124723.png)

```bash
npm install --save-dev core-js
```

接着在源文件夹下(src)的js代码中引入该模块。

```js
import "core-js/stable";
```

值得注意的是安装babel-loader和core-js的版本，尝试了很多次发现，他们需要低版本。

```json
"devDependencies": {
    "@babel/core": "^7.11.0",
    "@babel/preset-env": "^7.11.0",
    "babel-loader": "^8.1.0",
    "core-js": "^3.6.5",
    "webpack": "^4.44.1",
    "webpack-cli": "^3.3.12"
  }
```

## 综合应用

### 加载css

#### style-loader+css-loader

首先要在src文件夹下的某个js文件中导入css文件。

```js
import './src/style.css';
```

为了在 JavaScript 模块中 `import` 一个 CSS 文件，你需要安装 [style-loader](https://webpack.docschina.org/loaders/style-loader) 和 [css-loader](https://webpack.docschina.org/loaders/css-loader)，并在 [`module` 配置](https://webpack.docschina.org/configuration/module) 中添加这些 loader：

```bash
npm install --save-dev style-loader css-loader
```

> **webpack.config.js**

```diff
 const path = require('path');

 module.exports = {
   entry: './src/index.js',
   output: {
     filename: 'bundle.js',
     path: path.resolve(__dirname, 'dist'),
   },
+  module: {
+    rules: [
+      {
+        test: /\.css$/i,
+        use: ['style-loader', 'css-loader'],
+      },
+    ],
+  },
 };
```

模块 loader 可以链式调用。链中的每个 loader 都将对资源进行转换。链会逆序执行。第一个 loader 将其结果（被转换后的资源）传递给下一个 loader，依此类推。最后，webpack 期望链中的最后的 loader 返回 JavaScript。

应保证 loader 的先后顺序：[`'style-loader'`](https://webpack.docschina.org/loaders/style-loader) 在前，而 [`'css-loader'`](https://webpack.docschina.org/loaders/css-loader) 在后。loader数组是从右到左执行，先通过'css-loader'识别css文件，再通过`style-loader`将css代码嵌入到style标签中。（在控制台可以看到）

![image-20210620190450697](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210620190458.png)

#### 插件+loader

和上面的`style-loader`在style标签中内联不同，插件`mini-css-extract-plugin`会让html文件通过`link`标签引入css文件

首先要在src文件夹下的某个js文件中导入css文件。

```js
import './src/style.css';
```

接着安装插件`mini-css-extract-plugin`和`css-loader`。

```bash
npm install --save-dev css-loader mini-css-extract-plugin
```

配置`webpack.config.js`

```diff
 const path = require('path');
+ const MiniCssExtractPlugin=require('mini-css-extract-plugin');

 module.exports = {
   entry: './src/index.js',
   output: {
     filename: 'bundle.js',
     path: path.resolve(__dirname, 'dist'),
   },
+  module: {
+    rules: [
+      {
+        test: /\.css$/i,
+        //注意导入顺序
+        use: [MiniCssExtractPlugin.loader, 'css-loader'],
+      },
+    ],
+  },
+	plugins: [
+        new MiniCssExtractPlugin({
+            //指定生成的css的文件名
+            filename:'[name].css'
+        }),
+    ],
 };
```

最后，可以在dist文件夹下看到生成的css文件。

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210620194806.png" alt="image-20210620194803569" style="zoom:33%;" />

而且在控制台可以看到样式是通过`link`标签引入的。

![image-20210620194914627](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20210620194915.png)

### 加载图片

如果是远程图片，则可以直接显示。而本地图片通过webpack打包，需要额外处理。

> **webpack.config.js**

```diff
 const path = require('path');

 module.exports = {
   entry: './src/index.js',
   output: {
     filename: 'bundle.js',
     path: path.resolve(__dirname, 'dist'),
   },
   module: {
     rules: [
       {
         test: /\.css$/i,
         use: ['style-loader', 'css-loader'],
       },
+      {
+        test: /\.(png|svg|jpg|jpeg|gif)$/i,
+        type: 'asset/resource',
+      },
     ],
   },
 };
```

现在，在 `import MyImage from './my-image.png'` 时，此图像将被处理并添加到 `output` 目录，*并且* `MyImage` 变量将包含该图像在处理后的最终 url。在使用 [css-loader](https://webpack.docschina.org/loaders/css-loader) 时，如前所示，会使用类似过程处理你的 CSS 中的 `url('./my-image.png')`。loader 会识别这是一个本地文件，并将 `'./my-image.png'` 路径，替换为 `output` 目录中图像的最终路径。而 [html-loader](https://webpack.docschina.org/loaders/html-loader) 以相同的方式处理 `<img src="./my-image.png" />`。

#### js中使用图片

如果需要在js文件中使用图片，也可以用`asset module`这个loader，无需额外安装其他的loader。只需要在js中使用`import`引入图片即可。

```js
// 把图片当做模块引入
import logo from './image/img.png';

console.log(logo); // 会输出图片的路径
const myLogo = new Image();
myLogo.src = logo;
document.body.appendChild(myLogo)
```

#### 自定义文件名

默认情况下，`asset/resource` 模块以 `[hash][ext][query]` 文件名发送到输出目录。

可以通过在 webpack 配置中设置 [`output.assetModuleFilename`](https://webpack.docschina.org/configuration/output/#outputassetmodulefilename) 来修改此模板字符串：

>  **webpack.config.js**

```diff
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
+   assetModuleFilename: 'images/[hash][ext][query]'
  },
  module: {
    rules: [
      {
        test: /\.png/,
        type: 'asset/resource'
      }
    ]
  },
};
```

如可以设置为`assetModuleFilename: 'images/[name][ext]'`，此时的[name]指图片本身的名字，而是entry中的名字，[ext]是指"filename extension"，用它表示文件原来的后缀。

另一种自定义输出文件名的方式是，将某些资源发送到指定目录：

```diff
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
+   assetModuleFilename: 'images/[hash][ext][query]'
  },
  module: {
    rules: [
      {
        test: /\.png/,
        type: 'asset/resource'
-     }
+     },
+     {
+       test: /\.html/,
+       type: 'asset/resource',
+       generator: {
+         filename: 'static/[hash][ext][query]'
+       }
+     }
    ]
  },
};
```

使用此配置，所有 `html` 文件都将被发送到输出目录中的 `static` 目录中。

`Rule.generator.filename` 与 [`output.assetModuleFilename`](https://webpack.docschina.org/configuration/output/#outputassetmodulefilename) 相同，并且仅适用于 `asset` 和 `asset/resource` 模块类型。

#### 添加公共路径

当使用` MiniCssExtractPlugin`生成的css在dist下一个文件夹时(如`dist/css/style.css`)，此时如果不设置生成的css的公共路径(`publicPath`)，图片不会正常显示。

因为`css-loader`以为生成的`style.css`文件直接在dist目录下，所以图片路径会默认设置为`url(./随机名字.png)`，而事实上图片的正确的路径为`url(../随机名字.png)`。

```diff
  img-demo
  |- package.json
  |- webpack.config.js
  |- /dist
    |- index.js
    |- index.html
+   |- 随机字符.png
+   |- /css
+    	|- style.css
  |- /src
    |- icon.png
    |- style.css
    |- index.js
  |- /node_modules
```

因此，需要在`options`中设置`publicPath`为`../`，这样该css文件引入的所有url前面都会加上`../`的前缀。

> **webpack.config.js**

```diff
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin=require('mini-css-extract-plugin');

module.exports = {
    entry: {
        index: './src/index.js',
    },
    output: {
        filename: '[name].js',
        path: path.resolve(__dirname, 'dist'),
    },
    module:{
        rules:[
            {
                test:/\.css$/i,
                //此时生成的css在dist/css的文件夹下，要设置公共路径
                //这样该css文件所有的url前面都会加上../的前缀。
+                use:[{
+                    loader:MiniCssExtractPlugin.loader,
+                    options:{
+                        publicPath:'../'
+                    }
+                }, 'css-loader']},
            {
                test:/\.(png|svg|jepg|gif)/,
                type:'asset/resource'
            }
        ]  
    },
    plugins: [
        new HtmlWebpackPlugin({
            //指定一个html文件作为模板
            template: "./index.html"
        }),
+        new MiniCssExtractPlugin({
+            //在dist/css文件夹下创建一个index.css文件
+            filename:'css/[name].css'
+        })
    ],
    mode: 'development'
};
```

#### html中的图片

css中引入的图片可以用`type:asset/resource;` ，html中的图片则需要额外安装loader`html-withimg-loader`

```bash
npm install --save-dev html-withimg-loader
```

此时，我只写出新增的代码，其他的如图片loader，html-webpack-plugin和mini-css-extract-plugin插件用法参考上面的代码。

**注意**:这个`html-withimg-loader`必须要配合处理图片的loader——`asset  module` 一起使用，因为真正能够加载图片的还是`asset module`，`html-withimg-loader`用来处理路径问题。

```js
module:{
    rules:[
        {
            test:/\.(html|htm)/i,
            use:'html-withimg-loader'
        }
    ]  
},
```

#### asset/inline

asset/inline输出的 data URI，默认是呈现为使用 Base64 算法编码的文件内容。

**webpack.config.js**

> 关于[rules.parser](https://webpack.docschina.org/configuration/module/#ruleparserdataurlcondition)

```diff
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
+      {
+       test: /\.svg/,
+       type: 'asset/inline',
+		parser:{
+			dataUrlCondition:
+				maxSize:10000
+      			//表示10kb
+       }
+     },

    ]
  }
};
```

maxSize为10000，表示小于10kb将其转换为base64格式，大于则按照asset/resource来处理。一般只对较小的图片进行这种操作，因为转为base64格式后，图片会以base64格式来存在js中。

### 配置开发环境

#### 使用source map 追踪错误

当 webpack 打包源代码时，可能会很难追踪到 error(错误) 和 warning(警告) 在源代码中的原始位置。例如，如果将三个源文件（`a.js`, `b.js` 和 `c.js`）打包到一个 bundle（`bundle.js`）中，而其中一个源文件包含一个错误，那么堆栈跟踪就会直接指向到 `bundle.js`。你可能需要准确地知道错误来自于哪个源文件，所以这种提示这通常不会提供太多帮助。

为了更容易地追踪 error 和 warning，JavaScript 提供了 [source maps](http://blog.teamtreehouse.com/introduction-source-maps) 功能，可以将编译后的代码映射回原始源代码。如果一个错误来自于 `b.js`，source map 就会明确的告诉你。

source map 有许多 [可用选项](https://webpack.docschina.org/configuration/devtool)，请务必仔细阅读它们，以便可以根据需要进行配置。

对于本指南，我们将使用 `inline-source-map` 选项，这有助于解释说明示例意图（此配置仅用于示例，不要用于生产环境）：

**webpack.config.js**

```diff
 const path = require('path');
 const HtmlWebpackPlugin = require('html-webpack-plugin');

 module.exports = {
   mode: 'development',
   entry: {
     index: './src/index.js',
     print: './src/print.js',
   },
+  devtool: 'inline-source-map',
   plugins: [
     new HtmlWebpackPlugin({
       title: 'Development',
     }),
   ],
   output: {
     filename: '[name].bundle.js',
     path: path.resolve(__dirname, 'dist'),
     clean: true,
   },
 };
```

### 使用 webpack-dev-server 

`webpack-dev-server` 为你提供了一个基本的 web server，并且具有 live reloading(实时重新加载) 功能。设置如下：

```bash
npm install --save-dev webpack-dev-server
```

修改配置文件，告知 dev server，从什么位置查找文件：

**webpack.config.js**

```diff
 const path = require('path');
 const HtmlWebpackPlugin = require('html-webpack-plugin');

 module.exports = {
   mode: 'development',
   entry: {
     index: './src/index.js',
     print: './src/print.js',
   },
   devtool: 'inline-source-map',
+  devServer: {
+    contentBase: './dist',
+  },
   plugins: [
     new HtmlWebpackPlugin({
       title: 'Development',
     }),
   ],
   output: {
     filename: '[name].bundle.js',
     path: path.resolve(__dirname, 'dist'),
     clean: true,
   },
 };
```

以上配置告知 `webpack-dev-server`，将 `dist` 目录下的文件 serve 到 `localhost:8080` 下。（serve，将资源作为 server 的可访问文件）

**package.json**

```diff
 {
   "name": "webpack-demo",
   "version": "1.0.0",
   "description": "",
   "private": true,
   "scripts": {
     "test": "echo \"Error: no test specified\" && exit 1",
     "watch": "webpack --watch",
+    "start": "webpack serve --open",
     "build": "webpack"
   },
   "keywords": [],
   "author": "",
   "license": "ISC",
   "devDependencies": {
     "html-webpack-plugin": "^4.5.0",
     "webpack": "^5.4.0",
     "webpack-cli": "^4.2.0",
     "webpack-dev-server": "^3.11.0"
   },
   "dependencies": {
     "lodash": "^4.17.20"
   }
 }
```

现在，在命令行中运行 `npm start`，我们会看到浏览器自动加载页面。如果你更改任何源文件并保存它们，web server 将在编译代码后自动重新加载。试试看！

`webpack-dev-server` 具有许多可配置的选项。关于其他更多配置，请查看 [配置文档](https://webpack.docschina.org/configuration/dev-server)。

> ###### Warning
>
> webpack-dev-server 在编译之后不会写入到任何输出文件。而是将 bundle 文件保留在内存中，然后将它们 serve 到 server 中，就好像它们是挂载在 server 根路径上的真实文件一样。如果你的页面希望在其他不同路径中找到 bundle 文件，则可以通过 dev server 配置中的 [`publicPath`](https://webpack.docschina.org/configuration/dev-server/#devserverpublicpath-) 选项进行修改。
