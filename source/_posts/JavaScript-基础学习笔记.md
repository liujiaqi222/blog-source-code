---
title: JavaScript 基础学习笔记
date: 2021-09-13 20:30:12
tags: [JavaScript]
---



此为早期的学习笔记，顺序是从下写到上的，有点奇怪。

### 继承

![image-20210514173152887](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514173152887.png)

![image-20210514173008842](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514173008842.png)

![image-20210514173046027](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514173046027.png)

![image-20210514173456308](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514173456308.png)

![image-20210514173658942](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514173658942.png)

```js
// 父类
function People(name,age,sex){
    this.name=name;
    this.age=age;
    this.sex=sex;
}
People.prototype.sayHello=function(){
    console.log("我是"+this.name+"我今年"+this.age+"岁了。");
}
// 关键语句，实现语句
Student.prototype=new People();
// 子类
function Student(name,age,sex,school,studentNumber){
    this.name=name;
    this.age=age;
    this.sex=sex;
    this.school=school;
    this.studentNumber=studentNumber;
}
Student.prototype.study=function(){
    console.log(this.name+"正在学习。");
}
// 实例化Studnet
var hanmeimei=new Student("hanmeimei",12,"female","小学",2112);
hanmeimei.sayHello();
hanmeimei.study();
```

### 红绿灯小案例

![image-20210523210759559](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210523210801.png)

![image-20210523211055855](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210523211059.png)

![image-20210523211228945](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210523211233.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #box img{
            width: 80px;
        }
    </style>
</head>
<body>
    <div id="box"></div>
    <script>
        var box=document.getElementById("box");
        // 定义红绿灯类
        function TrafficLight(){
            // 红1、黄2、绿3；一开始默认是红色
            this.color=1;
            // 调用自己的初始化方法
            this.init();
            // 绑定监听
            this.bandEvent();
        }
        TrafficLight.prototype.init=function(){
            // 创建自己的dom
            this.dom=document.createElement('img');
            // 设置img的src属性
            this.dom.src="img/hong ("+this.color+").jpg";
            box.appendChild(this.dom);
        }
        TrafficLight.prototype.bandEvent=function(){
            // 备份上下文，这里的this指的是JS实例
            var self=this;

            // 当自己的dom被点击时，更改颜色
            this.dom.onclick=function(){
                // 事件处理函数的上下文是绑定事件的D0M元素
                // 所以如果直接写this，指的是this.dom，因此需要备份
                self.changeColor();
            }
        }
        // 改变颜色
        TrafficLight.prototype.changeColor=function(){
            this.color++;
            if(this.color==4){
                this.color=1;
            }
            // 还要更改dom的src属性
            this.dom.src="img/hong ("+this.color+").jpg";
        }
        // 实例化100个
        var count=100;
        while(count--){
            new TrafficLight();
        }
    </script>
</body>
</html>
```

### 炫彩小球

![image-20210525000452880](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210525000501.png)

![image-20210525000827824](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210525000829.png)



### 包装类

![image-20210525204639793](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210525204649.png)

![image-20210525205355774](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210525205357.png)

```js
var a=new Number(123);
var b=new String('慕课网');
var c=new Boolean(true);
console.log(a);
console.log(typeof a); //object
console.log(b);
console.log(typeof b);//object
console.log(c);
console.log(typeof c);//object
console.log(5+a); //128
console.log(b.slice(0,2)); //'慕课'
console.log(c &&true); //true

var d=123;
console.log(d.__proto__==Number.prototype); //true

var e='慕课网';
console.log(String.prototype.hasOwnProperty('toLowerCase')); //true
```

![image-20210525211311085](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210525211312.png)

### Math数学对象

![image-20210525211533279](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210525211534.png)

![image-20210525211545589](C:\Users\liujiaqi\AppData\Roaming\Typora\typora-user-images\image-20210525211545589.png)



### call和apply

![image-20210514100907845](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514100907845.png)

call和apply 都是用来修改函数中this的指向问题。

其次就是它们不同的传参方式：注意上一句话中说他们的作用时有两个关键词 ‘函数’和‘this’，想要修改this 的指向，那么必然有一个this修改后的指向，而函数必然后关系到传参问题：call方法可以传给该函数的参数分别作为自己的多个参数，而apply方法必须将传给该函数的参数合并成一个数组作为自己的一个参数：


![image-20210514101032131](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514101032131.png)

```js
function sum(){
    alert(this.c+this.m+this.e);
}
// var xiaoming={
//     c:100,
//     m:90,
//     e:80,
//     sum:sum
// }
// xiaoming.sum();
var xiaoming={
    c:100,
    m:90,
    e:80,
}
// 用call或者apply指定了上下文，不然直接使用sum(),this指的是window对象
sum.call(xiaoming);
sum.apply(xiaoming);
```

### call和apply的区别

![image-20210514101640663](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514101640663.png)

```js
function sum(b1,b2){
    alert(this.c+this.m+this.e+b1+b2);
}
var xiaoming={
    c:100,
    m:90,
    e:80
}
sum.call(xiaoming,10,20);
sum.apply(xiaoming,[10,20]);
```



![image-20210514102541446](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514102541446.png)

```js
function fun1(){
    fun2.apply(this,arguments);
}
function fun2(a,b){
    console.log(a+b);
}
fun1(33,44);
// 因为fun1是直接调用，所以fun1中的this指代window对象
// arguments 是类数组对象，因此只能用apply方法因为[33,44]刚好组成数组
```

### new操作符调用函数

![image-20210514113306796](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514113306796.png)

![image-20210514113430404](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514113430404.png)

![image-20210514113651439](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514113651439.png)

![image-20210514113707453](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514113707453.png)

![image-20210514113755873](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514113755873.png)

![image-20210514113904986](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514113904986.png)

![image-20210514114046769](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514114046769.png)



### 上下文规则总结

![image-20210514114804916](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514114804916.png)

### 构造函数

![image-20210514114959742](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514114959742.png)

![image-20210514115122199](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514115122199.png)

![image-20210514115131221](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514115131221.png)

![image-20210514115249795](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514115249795.png)

![image-20210514115349388](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514115349388.png)

![image-20210514115424829](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514115424829.png)

![image-20210514115645773](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514115645773.png)

此时调用People函数，this指向window对象，执行函数会给window对象增加属性。

![image-20210514115846373](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514115846373.png)

### 类与实例

![image-20210514144158196](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514144158196.png)

![image-20210514143717398](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514143717398.png)

![image-20210514144057270](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514144057270.png)

### prototype

![image-20210514144341395](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514144341395.png)

![image-20210514144425779](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514144425779.png)

```js
function sum(a,b){
    return a+b;
}
// 任何函数都有prototype属性
console.log(sum.prototype); //{constructor: ƒ}
console.log(typeof sum.prototype); //object
console.log(sum.prototype.constructor===sum);//true
```

![image-20210514145006344](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514145006344.png)

![image-20210514145249629](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514145249629.png)

```js
function People(name,age,sex){
    this.name=name;
    this.age=age;
    this.sex=sex;
}
var xiaoming=new People("小明",13,"男");
console.log(xiaoming.__proto__===People.prototype); //true
```

### 原型链查找

![image-20210514150037890](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514150037890.png)

![image-20210514150159619](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514150159619.png)

```js
function People(name,age,sex){
    this.name=name;
    this.age=age;
    this.sex=sex;
}
People.prototype.nationality="中国";
var xiaoming=new People("小明",12,"男");
console.log(xiaoming.nationality); //中国
```

### hasOwnProperty

![image-20210514151255674](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514151255674.png)

### in

![image-20210514151732238](C:\Users\24734\AppData\Roaming\Typora\typora-user-images\image-20210514151732238.png)

```js
function People(name, age, sex) {
    this.name = name;
    this.age = age;
    this.sex = sex;
}
People.prototype.nationality = "中国";
var xiaoming = new People("小明", 12, "男");

console.log(xiaoming.hasOwnProperty("name")); //true
console.log(xiaoming.hasOwnProperty("age"));//true
console.log(xiaoming.hasOwnProperty("sex"));//true
console.log(xiaoming.hasOwnProperty("nationality")); //false

console.log('name' in xiaoming);//true
console.log('age' in xiaoming);//true
console.log('sex' in xiaoming);//true
console.log('nationality' in xiaoming);//true
```

### 在prototype上添加方法

![image-20210514164655981](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514164655981.png)

![image-20210514164616849](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514164616849.png)

```js
function People(){
    this.sayHello=function(){

    };
}
var xiaoming=new People();
var xiaohong=new People();
console.log(xiaoming.sayHello===xiaohong.sayHello); //false
```

不是同一个函数，会造成内存浪费

![image-20210514165403771](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514165403771.png)

![image-20210514165517802](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514165517802.png)

![image-20210514165542680](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514165542680.png)

```js
function People(name,age,sex){
    this.name=name;
    this.age=age;
    this.sex=sex;
}
People.prototype.sayHello=function(){
    console.log("你好，我是"+this.name+"，我今年"+this.age);
}

var xiaoming=new People("明",12,"male");
var xiaohong=new People("小红",12,"female");
console.log(xiaoming.sayHello===xiaohong.sayHello);//true
xiaoming.sayHello();
```

### 原型链的终点

![image-20210514170702986](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514170702986.png)

```js
function People(){
}
var xiaoming=new People();
console.log(xiaoming.__proto__.__proto__===Object.prototype);
//true People.prototype的原型是objec.prototype
console.log(Object.prototype.__proto__); 
//null Object.prototype是原型的终点，之后没有原型了
console.log(Object.prototype.hasOwnProperty('toString')); //true
console.log(Object.prototype.hasOwnProperty('hasOwnProperty')); //true
```

### 数组的原型链

![image-20210514172115376](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210514172115376.png)

```js
var arr=[32,43,89,23];
console.log(arr.__proto__===Array.prototype); //true
console.log(arr.__proto__.__proto__===Object.prototype);//true
console.log(Array.prototype.hasOwnProperty('push')); //true
console.log(arr.__proto__.hasOwnProperty('push'));//true
```

### this关键字

![image-20210512161554242](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512161554242.png)

![image-20210512162007652](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512162007652.png)

![image-20210512162204748](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512162204748.png)

此时 this指向window对象

![image-20210512162507140](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512162507140.png)

![image-20210512162552392](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512162552392.png)

**函数只有被调用时，this指向的对象才能被确定。**

![image-20210512164031960](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512164031960.png)

### 上下文规则

#### 规则1

规则1：对象打点调用它的方法函数，则函数的上下文是这个打点的对象

![image-20210512164133505](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512164133505.png)

![image-20210512164416166](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512164416166.png)

![image-20210512164828850](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512164828850.png)

![image-20210512165205479](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512165205479.png)

![image-20210512165422669](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512165422669.png)

#### 规则2

规则2：圆括号直接调用函数，则函数的上下文是 window对象

![image-20210512165517807](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512165517807.png)

![image-20210512170057424](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512170057424.png)

![image-20210512170413819](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512170413819.png)

#### 规则3

规则3：数组（类数组对象）枚举出函数进行调用，上下文是这个数组（类数组对象）

![image-20210512170513429](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512170513429.png)

![image-20210512170659401](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512170659401.png)

![image-20210512170934757](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512170934757.png)

![image-20210512171645322](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512171645322.png)

#### 规则4

规则4：IFE中的函数，上下文是 window对象

![image-20210512171752464](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512171752464.png)

![image-20210512172333001](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512172333001.png)

```js
// 规则4：IFE中的函数，上下文是 window对象
var a=1;
var obj={
    a:2,
    fn:(function(){
        var a=this.a; 
        //因为是立即执行函数，所以此处将window对象的全局变量a赋值给了a
        return function fn(){ 
            // 该立即执行函数的返回值是一个函数
            console.log(a+this.a);
        };
    })()
}
obj.fn();
```

#### 规则5

规则5：定时器、延时器调用函数，上下文是 window对象

![image-20210512172822842](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512172822842.png)

![image-20210512174433974](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512174433974.png)

![image-20210512174627203](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512174627203.png)

```js
// 规则5：定时器、延时器调用函数，上下文是 window对象
var obj={
    a:1,
    b:2,
    fn:function(){
        console.log(this.a+this.b);
    }
}
var a=6;
var b=4;
// 定时器一
setTimeout(obj.fn,1000); //此时会输出10

// 定时器二
setTimeout(function(){
    obj.fn(); //此时会输出3
},1200);
```

#### 规则6

规则6：事件处理函数的上下文是绑定事件的D0M元素

![image-20210512175301546](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512175301546.png)

![image-20210512180239206](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512180239206.png)

```html
<div id="box1"></div>
<div id="box2"></div>
<div id="box3"></div>
<script>
// 规则6：事件处理函数的上下文是绑定事件的DOM元素
function setColorToRed(){
    // 备份上下文
    var self =this;
    setTimeout(function(){ 
        //如果不备份，延时器的this指向的是window对象
        self.style.backgroundColor="red";
    },2000);
}
var box1=document.getElementById("box1");
var box2=document.getElementById("box2");
var box3=document.getElementById("box3");
box1.onclick=setColorToRed;
box2.onclick=setColorToRed;
box3.onclick=setColorToRed;
</script>
```

### 对象的语法

![image-20210510210902921](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510210902921.png)

![image-20210510211118946](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510211118946.png)

![image-20210510211436972](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510211436972.png)

#### 属性的访问

![image-20210510211531638](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510211531638.png)

![image-20210510211600733](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510211600733.png)

![image-20210510211730480](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510211730480.png)

#### 属性的更改

![image-20210510212307778](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510212307778.png)

#### 属性的创建

![image-20210510212341538](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510212341538.png)

#### 属性的删除

![image-20210510212519734](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510212519734.png)

```js
var obj={
    a:10,
    b:10
}
// 属性的更改
obj.b=30;
console.log(obj.b);

// 属性的增加
obj.c=49;
console.log(obj.c);

// 属性的删除
delete obj.c;
console.log(obj);
```

### 对象的方法

![image-20210510213226954](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510213226954.png)

![image-20210510213334215](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510213334215.png)

### 对象的遍历

![image-20210510213925343](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510213925343.png)

![image-20210510214039759](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510214039759.png)

```js
var obj={
    a:11,
    b:22,
    c:88
};
for(var k in obj){
    console.log(k+"的值是"+obj[k]);
}
```

注意：这里写的是obj[k]，而不是obj.k，因为obj并没有k属性，是通过k的引用来获取的。



### 对象的浅克隆

![image-20210510214637721](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510214637721.png)

![image-20210510214910285](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210510214910285.png)

![image-20210512150012885](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512150012885.png)

```js
var obj1 = {
    a: 1,
    b: 2,
    c: [22, 33, 44]
}
var obj2 = {};
for (var i in obj1) {
    //注意两个都是用[]获取属性值的，而不是打点调用
    //每遍历一个k属性，就给obj2也添加一个同名的k属性
    //obj2的值和obj1的k属性值相同
    obj2[i] = obj1[i];
}
console.log(obj2);
```

### 对象的深克隆

![image-20210512151514670](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210512151514670.png)

```js
var obj1={
    a:1,
    b:2,
    c:[22,33,{
        m:44,
        n:66,
        p:[77,88]
    }]
};
function deepClone(o){
    if(Array.isArray(o)){
        // 判断是否为数组
        var result=[];
        for(var i=0;i<o.length;i++){
            result.push(deepClone(o[i]));
        }

    }else if(typeof o=="object"){
        // 判断是否为对象
        var result={};
        for(var k in o){
            result[k]=deepClone(o[k]);
        }
    }else{
        // 基本类型
        var result=o;
    }
    return result;
}
var obj2=deepClone(obj1);
console.log(obj2);
```







### BOM

![image-20210509121109156](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509121109156.png)

![image-20210509121151521](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509121151521.png)

### window对象

![image-20210509121225354](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509121225354.png)

```js
var a=3;
console.log(window.hasOwnProperty("a"));//true
console.log(window.a); //3
```

![image-20210509161051280](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509161051280.png)

### 窗口尺寸

![image-20210509161422715](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509161422715.png)

```js
console.log("包含滚动条的内宽",window.innerWidth);
console.log("不含滚动条的内宽",document.documentElement.clientWidth);
console.log("窗口外宽",window.outerWidth);
```

### resize事件

![image-20210509162306564](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509162306564.png)

```js
window.onresize=function(){
    var root=document.documentElement;
    console.log("窗口改变尺寸",root.clientWidth,root.clientHeight);
}
```

### 已滚动高度

![image-20210509162749574](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509162749574.png)

![image-20210509162908429](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509162908429.png)

### scroll 事件

![image-20210509163422591](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509163422591.png)

### navigator对象

![image-20210509163723801](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509163723801.png)

```js
console.log("浏览器名称",window.navigator.appName);
console.log("浏览器版本",navigator.appVersion);
console.log("用户代理",navigator.userAgent);
console.log("操作系统",navigator.platform);
```

### history对象

![image-20210509164520393](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509164520393.png)

```html
<h1>我是history方法</h1>
<button id="btn">返回</button>
<br>
<a href="javascript:history.back();">返回</a>
<script>
    var btn=document.getElementById("btn");
    btn.onclick=function(){
        // history.back();
        // 也可以用history.go(-1);
        history.go(-1);
    }
</script>
```

### location对象

![image-20210509165423038](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509165423038.png)

![image-20210509165814691](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509165814691.png)

```html
<button id="btn1">去慕课</button>
<button id="btn2">重新加载</button>
<script>
    console.log(location);
    var btn1 = document.getElementById("btn1");
    var btn2 = document.getElementById("btn2");
    btn1.onclick = function () {
        location.href = "https://imooc.com";
    }
    btn2.onclick = function () {
        location.reload(true);
        // true 表示重新加载
    }

</script>
```

![image-20210509170103070](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509170103070.png)

### 返回顶部按钮

![image-20210509170300064](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509170300064.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <style>
        body{
            height: 5000px;
            background-image: linear-gradient(to bottom,white,black);
        }
        .backToTop{
            width: 80px;
            height: 40px;
            background-color: pink;
            position: fixed;
            bottom: 50px;
            right: 50px;
            color: white;
            line-height: 40px;
            text-align: center;
            /* 改变鼠标形状 */
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="backToTop" id="backToTop">返回顶部</div>
    <script>
        var timer
        var btn=document.getElementById("backToTop");
        btn.onclick=function(){
        //设表先关
            clearInterval(timer);
            timer=setInterval(() => {
                // 不断让scrollTop减少
                document.documentElement.scrollTop-=300;
                // 定时器需要停止
                if(window.scrollY==0){
                    clearInterval(timer);
                }
            },20);
        }
    </script>
</body>
</html>
```

### 楼层导航效果

![image-20210509171726760](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210509171726760.png)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .content-part {
            width: 1000px;
            margin: 0 auto;
            margin-bottom: 30px;
            background-color: gray;
            font-size: 30px;
        }

        .floornav {
            top: 50%;
            margin-top: -60px;
            position: fixed;
            right: 20px;
            height: 120px;
            width: 40px;
            background-color: #666;
            line-height: 24px;
            text-align: center;
        }

        .floornav ul {
            list-style: none;
            color: white;
            cursor: pointer;
        }
        .floornav ul li{
            line-height: 24px;
            height: 24px;
        }
        .floornav ul li.current {
            background-color: #111;
        }
    </style>
</head>

<body>
    <nav class="floornav">
        <ul id="list">
            <li data-n="科技" class="current">科技</li>
            <li data-n="体育">体育</li>
            <li data-n="新闻">新闻</li>
            <li data-n="娱乐">娱乐</li>
            <li data-n="视频">视频</li>
        </ul>
    </nav>
    <section class="content-part" style="height: 674px;" data-n="科技">科技栏目</section>
    <section class="content-part" style="height: 674px;" data-n="体育">体育栏目</section>
    <section class="content-part" style="height: 574px;" data-n="新闻">新闻栏目</section>
    <section class="content-part" style="height: 624px;" data-n="娱乐">娱乐栏目</section>
    <section class="content-part" style="height: 844px;" data-n="视频">视频栏目</section>
    <script>
        // 使用事件委托给li添加监听
        var list = document.getElementById("list");
        var contentParts = document.querySelectorAll(".content-part");
        var lis = document.querySelectorAll("#list li");
        list.onclick = function (e) {
            if (e.target.tagName.toLowerCase() == "li"){
                // 获取自定义属性值
                var n = e.target.getAttribute("data-n");
            // 用属性选择器选择有相同data-n的content-part
            var contentPart = document.querySelector(".content-part[data-n=" + n + "]");
            // 让页面滚动
            document.documentElement.scrollTop = contentPart.offsetTop;
            }
        }
        // 将所有的conten-part盒子的offsetTop推入数组中
        var offsetTopArr = [];

        for (var i = 0; i < contentParts.length; i++) {
            offsetTopArr.push(contentParts[i].offsetTop);
        }
        // 为了可以方便得到最后一层，推入无穷大
        offsetTopArr.push(Infinity);
        console.log(offsetTopArr);
        // 当前楼层 设置为-1，方便第一次也能输出值
        var nowFloor = -1;
        //窗口的滚动
        window.onscroll = function () {
            // 当前窗口卷动值
            var scrollTop = document.documentElement.scrollTop;
            // 为了得到当前楼层
            for (var i = 0; i < offsetTopArr.length; i++) {
                if (scrollTop >= offsetTopArr[i] && scrollTop < offsetTopArr[i + 1]) {
                    break;
                }
            }
            if (nowFloor != i) {
                console.log(i);
                nowFloor = i;
                for (var j = 0; j < lis.length; j++) {
                    if (j == i) {
                        lis[j].className = "current";
                    } else {
                        lis[j].className = "";
                    }
                }
            }
        }
    </script>
</body>

</html>
```



![image-20210426233600830](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210426233600830.png)

```js
var ul=document.getElementById("list");
var lis=document.getElementsByTagName("li");
for(i=0;i<lis.length;i++){
    lis[i].onclick=function(){
        // 不知道为什么此处使用lis[i]会报错，只能使用this
        this.style.backgroundColor="blue";
    }
}
```

![image-20210426234936537](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210426234936537.png)



![image-20210427000510771](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210427000510771.png)

### 事件委托

![image-20210427000856702](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210427000856702.png)

![image-20210427000930553](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210427000930553.png)

```js
<ul id="list">
    <li>列表项</li>
    <li>列表项</li>
    <li>列表项</li>
</ul>
<script>
var list=document.getElementById("list");
//将事件监听添加到祖先项，而不是元素
list.onclick=function(e){
    //e.target表示用户真正点击的元素（最早触发词事件的元素）
    e.target.style.color="red";
    // e.currentTarget 则会使ul和所有的子元素的背景色变蓝
    //事件处理程序附件到的元素
    e.currentTarget.style.backgroundColor="blue"
}
```

![image-20210427002215972](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210427002215972.png)

![image-20210427003616423](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210427003616423.png)

```js
<ul id="list">
    <li>列表项</li>
    <li>列表项</li>
    <li>列表项</li>
</ul>
<script>

list.onmouseenter=function(e){
    e.target.style.color = "red";
}
</script>
```

onmouseenter不冒泡，没有冒泡过程，因此e.target无法识别最早触发事件的元素。所以，一触碰会使例子中ul元素和ul里的子元素变红。

### 定时器延时器

![image-20210427004350325](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210427004350325.png)

![image-20210427004855323](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210427004855323.png)

![image-20210427004957353](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210427004957353.png)

![image-20210427005147286](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210427005147286.png)

```html
<h1 id="info">0</h1>
<button id="btn1">开始</button>
<button id="btn2">停止</button>
<script>
    var info=document.getElementById("info");
    var btn1=document.getElementById("btn1");
    var btn2=document.getElementById("btn2");
    var a=0;
    var timer;
    btn1.onclick=function(){
        clearInterval(timer);
        // 为了防止定时器叠加，应该在设置定时器之前先清除定时器
        // timer需要成为全局变量，不然下面的函数无法使用timer变量
        timer=setInterval(function(){
        info.innerHTML=++a;
    },1000);
    }
    btn2.onclick=function(){
        clearInterval(timer);
    }
</script>
```

为了防止定时器叠加，应该在设置定时器之前先清除定时器。如果不清除定时器，用户多次点击时会创建多个定时器。

![image-20210508145500526](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210508145500526.png)

![image-20210508145523493](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210508145523493.png)

### 异步语句

![image-20210508150207492](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210508150207492.png)

![image-20210508150338194](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210508150338194.png)

### 定时器实现动画

![image-20210508150723680](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210508150723680.png)

### js和css3实现动画

![image-20210508152155375](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210508152155375.png)

![动画](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/%E5%8A%A8%E7%94%BB.gif)

点击按钮后实现盒子左右移动

```html
<button id="btn">开始运动</button>
<div id="box"></div>
<script>
var btn=document.getElementById("btn");
var box=document.getElementById("box");

// 标示量 指示盒子是在左边还是右边
var pos=1;  //1表示左边 2表示右边
btn.onclick=function(){
    // 加上过渡
    box.style.transition="left 3s linear 0s";
    if(pos==1){
        // 由于有过渡，所以是动画，不是瞬间移动
        box.style.left="1000px";
        pos=2;
    }else if(pos==2){
        // 由于有过渡，所以是动画，不是瞬间移动
        box.style.left="100px";
        pos=1;
    }
}
</script>
```

### 函数节流

![image-20210508154141227](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210508154141227.png)

![image-20210508154251078](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210508154251078.png)

### 无缝连续滚动

![image-20210508174625810](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210508174625810.png)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        #box {
            width: 1000px;
            height: 130px;
            border: 1px solid #000;
            margin: 50px auto;
            overflow: hidden;
        }

        #box ul {
            list-style-type: none;
            /* 把宽设置大一点，li才能并排 */
            width: 5000px;
            position: relative;
        }

        #box ul li {
            float: left;
            margin-right: 10px;
        }
    </style>
</head>

<body>
    <div id="box">
        <ul id="list">
            <li><img src="img/0.png" alt=""></li>
            <li><img src="img/1.png" alt=""></li>
            <li><img src="img/2.png" alt=""></li>
            <li><img src="img/3.png" alt=""></li>
            <li><img src="img/4.png" alt=""></li>
            <li><img src="img/5.png" alt=""></li>
        </ul>
    </div>
    <script>
        var box = document.getElementById("box");
        var list = document.getElementById("list");
        // 复制一次所有的li
        list.innerHTML += list.innerHTML;

        // 设置当然list的left值
        var left = 0;
        // 设置定时器
        var timer;
        move();
        function move() {
            // 先清除定时器，防止累积
            clearInterval(timer);
            timer = setInterval(() => {
                left -= 4;
                if (left <= -1260) {
                    left = 0;
                }
                list.style.left = left + "px";
            }, 20);
        }
        // 鼠标进入 停止定时器
        box.onmouseenter = function () {
            clearInterval(timer);
        }

        // 鼠标离开 定时器继续执行
        box.onmouseleave=function(){
            move();
        }
    </script>
</body>
</html>
```

### 轮播图

```htm
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .carousel{
            width: 650px;
            height: 360px;
            border: 1px solid #000;
            margin: 50px auto;
            position: relative;
            overflow: hidden;
        }
        .carousel ul{
            list-style: none;
            /* 把宽设置尽可能大，这样li才能排成一排 */
            width: 4000px;
            position: relative;
            left: 0;
            transition: left 0.2s linear 0s;
        }
        .carousel ul li{
            float: left;
        }
        .carousel .btn{
            position: absolute;
            height: 50px;
            width: 50px;
            background-color: rgba(0, 153, 255,0.5);
            border-radius: 50%;
            top: 50%;
            margin-top: -25px;
        }
        .carousel .leftbtn{
            left: 20px;
        }
        .carousel .rightbtn{
            right: 20px;
        }
    </style>
</head>
<body>
    <div class="carousel">
        <ul id="list">
            <li><img src="img/beijing/0.jpg" alt=""></li>
            <li><img src="img/beijing/1.jpg" alt=""></li>
            <li><img src="img/beijing/2.jpg" alt=""></li>
            <li><img src="img/beijing/3.jpg" alt=""></li>
            <li><img src="img/beijing/4.jpg" alt=""></li>
        </ul>
        <!-- 让a标签不刷新 -->
        <a href="javascript:;" class="leftbtn btn" id="leftbtn"></a>
        <a href="javascript:;" class="rightbtn btn" id="rightbtn"></a>
    </div>
    <script>
        var list=document.getElementById("list");
        var lbtn=document.getElementById("leftbtn");
        var rbtn=document.getElementById("rightbtn");
        // 克隆第一张图片 需要写true才能克隆li里面的img标签
        var clone0=list.firstElementChild.cloneNode(true);
        list.appendChild(clone0);
        // 当前是第几张图片
        var id=0;
        // 给右按钮设置动画
        var lock=true;
        rbtn.onclick=function(){
            if(!lock) return;
            // 最后一张图片取消了过渡，要加回来
            list.style.transition="left 0.2s linear 0s"
            id++;
            if(id>4){
                // 设置一个延时器(因为之前的过渡动画用时0.2s)，将ul的left瞬间变为0
                setTimeout(() => {
                    // 此时要瞬间移动，不要用动画过渡回去
                    list.style.transition="none";
                    id=0;
                    list.style.left=0;
                }, 200);
            }
            list.style.left=-id*650+"px";
            lock=false;
            setTimeout(() => {
                lock=true;
            }, 400);
        }
        // 给左按钮设置动画
        
        leftbtn.onclick=function(){
            if(!lock)return;
            if(id==0){
                // 判断是否为第0张，如果是的，瞬间用最后一张假的替换第一张
                list.style.transition="none";
                list.style.left=-650*5+"px";

                // 然后再切换到真正的最后一张图（beijing/4）
                //设置一个延时器，这个延时器的延时时间可以是0毫秒，虽然是0毫秒，但是可以让我们过渡先是瞬间取消，然后再加上
                setTimeout(() => {
                    list.style.left=-650*4+"px";
                    list.style.transition="left 0.2s linear 0s"

                }, 0);
                id=4;
            }
            else{
                id--;
                list.style.transition="left 0.2s linear 0s"
                list.style.left=-id*650+"px";
            }      
            lock=false;
            setTimeout(()=>{
                lock=true;
            },400);      
        }
    </script>
</body>
</html>
```



![image-20210424211412615](C:\Users\24734\AppData\Roaming\Typora\typora-user-images\image-20210424211412615.png)

![image-20210424211636507](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424211636507.png)

![image-20210424211837616](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424211837616.png)

![image-20210424212039029](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424212039029.png)

![image-20210424212112609](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424212112609.png)

![image-20210424212354417](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424212354417.png)

![image-20210424221619105](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424221619105.png)

![image-20210424221810265](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424221810265.png)

![image-20210424222754200](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424222754200.png)

给window添加onload事件监听

![image-20210424223445993](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424223445993.png)

![image-20210424223514608](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424223514608.png)

```html
<div id="box1">
    <p>我的段落</p>
    <p>我的段落</p>
    <p>我的段落</p>
    <p>我的段落</p>
</div>
<div id="box2">
    <p>我的段落</p>
    <p>我的段落</p>
    <p>我的段落</p>
    <p>我的段落</p>
</div>
<script>
    // 会得到全部的P标签
    var ps = document.getElementsByTagName("p");
    
    // 可以先得到box1
    var box1=document.getElementById("box1");
    //得到box1里面的p标签
    var pInBox1=box1.getElementsByTagName("p");
</script>
```

![image-20210424224417448](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424224417448.png)

![image-20210424224438247](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424224438247.png)

![image-20210424224551111](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424224551111.png)

![image-20210424224702206](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424224702206.png)

![image-20210424225256867](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424225256867.png)

### 父节点子节点

![image-20210424225709892](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424225709892.png)

![image-20210424225741728](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424225741728.png)

![image-20210424225851251](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424225851251.png)

![image-20210424225936093](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210424225936093.png)

```html
<body>
    <div id="box">
        <p>我是段落</p>
        <p id="para">我是段落</p>
        <p>我是段落</p>
    </div>
    <script>
        var box=document.getElementById("box");
        var para=document.getElementById("para");
        // 所有的子节点
        console.log(box.childNodes); //NodeList(7) [text, p, text, p#para, text, p, text]
        // 所有元素子节点
        console.log(box.children);//HTMLCollection(3) [p, p#para, p, para: p#para]
        // 第一个子节点
        console.log(box.firstChild); //#text
        console.log(box.firstChild.nodeType) //3
        // 第一个元素子节点
        console.log(box.firstElementChild); //<p>我是段落</p>
        // 父节点
        console.log(para.parentNode);
        // 前一个兄弟节点
        console.log(para.previousSibling);//#text
        // 前一个元素兄弟节点
        console.log(para.previousElementSibling);//<p>我是段落</p>
        // 后一个兄弟节点
        console.log(para.nextSibling);//#text
        // 后一个元素兄弟节点
        console.log(para.nextElementSibling);//<p>我是段落</p>
    </script>
</body>
```

封装节点关系的常用函数

```js
// 封装返回所有子元素节点的的函数，类似children
function getChildren(node){
    // 结果数组
    var children=[];
    // 遍历node下的所有子节点的nodeType属性是否为1
    // nodeType=1 表示为元素
    for(var i=0;i<node.childNodes.length;i++){
        if(node.childNodes[i].nodeType==1){
            children.push(node.childNodes[i]);
        }
    }
    return children;
}

// 可以得到前一个兄弟元素节点，类似previousElementSibling
function getElementPrevious(node){
    var o=node;
    while(o.previousSibling!=null){
        if(o.previousSibling.nodeType==1){
            return o.previousSibling;
        }
        o=o.previousSibling;
    }
}

// 返回元素所有的元素兄弟节点
function getAllElementSibling(node){
    var prevs=[];
    var nexts=[];
    var o=node;
    while(o.previousSibling!=null){
        if(o.previousSibling.nodeType==1){
            prevs.unshift(o.previousSibling);
        }
        o=o.previousSibling;
    }
    var o=node;
    while(o.nextSibling!=null){
        if(o.nextSibling.nodeType==1){
            nexts.push(o.nextSibling);
        }
        o=o.nextSibling;
    }
    return prevs.concat(node,nexts)
}
```

![image-20210425012724520](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425012724520.png)

![image-20210425124524341](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425124524341.png)

```js
document.getElementById("box").style.backgroundColor="red";
var oBox=document.getElementById("box");
oBox.style.backgroundColor="red";
```

设置的是元素的行内样式

![image-20210425125801427](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425125801427.png)

```js
var a=document.getElementsByTagName("a");
a[0].href="https://www.mi.com";
a[0].innerText="去小米"
```

![image-20210425130732587](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425130732587.png)

```js
var box=document.getElementById("box");
box.setAttribute("data-n",10);
var n=box.getAttribute("data-n");
alert(n);
```

![image-20210425132505214](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425132505214.png)

![image-20210425132545102](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425132545102.png)

![image-20210425132704353](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425132704353.png)

![image-20210425132730793](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425132730793.png)

```js
var oBox=document.getElementById("box");
// 创建孤儿节点
var oP=document.createElement("p");
oP.innerText="段落3";
// 添加到DOM树上 appendChild
oBox.appendChild(oP);
var pp=document.createElement("p");
pp.innerText="我是插进来的"
// 添加到DOM树上 insertBefore
oBox.insertBefore(pp,oBox.firstChild);
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <style>
        td {
            width: 120px;
            height: 30px;
            border: 1px solid black;
            text-align: center;
            line-height: 30px;
        }
        table{
            /* 让边框重叠，注意是加在table标签上的 */
            border-collapse: collapse;
        }
    </style>
</head>

<body>
    <table id="mytable"></table>
    <script>
        // 创建九九乘法表
        var mytable = document.getElementById("mytable");
        // 外层循环创建行，内层循环创建td
        for (var i = 1; i <= 9; i++) {
            var tr = document.createElement("tr");
            mytable.appendChild(tr);
            for (j = 1; j <=i; j++) {
                var td = document.createElement("td");
                // 设置td内部的文字
                td.innerText=j+"*"+i+"="+i*j;
                tr.appendChild(td);
            }
        }
    </script>
</body>
</html>
```

![image-20210425140413994](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425140413994.png)

![image-20210425140803675](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425140803675.png)

```html
<div id="box1">
    <p id="para">我是段落</p>
</div>
<div id="box2">
</div>
<script>
    var box1=document.getElementById("box1");
    var box2=document.getElementById("box2");
    var para=document.getElementById("para");
    box2.appendChild(para);
</script>

```

![image-20210425143430030](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425143430030.png)

```html
<div id="box">
    <p>我是p节点</p>
    <p>我是p2节点</p>

</div>
<script>
    var box=document.getElementById("box");
    box.removeChild(box.firstElementChild);
    box.remove(); //自己删除自己
</script>
```

![image-20210425144940421](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425144940421.png)

```html
<div id="box1">
    <ul>
        <li>牛奶</li>
        <li>咖啡</li>
        <li>巧克力</li>
    </ul>
</div>
<div id="box2"></div>
<script>
    var box1=document.getElementById("box1");
        var box2=document.getElementById("box2");
        var ul=document.getElementsByTagName("ul")[0];
    // 克隆ul标签
    var new_ul=ul.cloneNode();
    box2.appendChild(new_ul);
</script>
```

![image-20210425145809152](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425145809152.png)

![image-20210425145924019](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425145924019.png)

![image-20210425145954659](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425145954659.png)

### 鼠标事件

![image-20210425150543512](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425150543512.png)

onclick表示鼠标点击了并且松开了鼠标，onmousedown只需要点击，不要求鼠标抬起。使用onclick，如果鼠标点击了但未抬起，离开了监听区域再抬起，onclick事件不会被触发 。

![image-20210425151939871](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425151939871.png)

![image-20210425152734651](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425152734651.png)

```html
<form id="myform">
    <p>姓名：
        <input type="text" name="nameField">
    </p>
    <p>年龄：
        <input type="text" name="ageField">
    </p>
    <p>
        <input type="submit">
    </p>
</form>
<script>
    var myform=document.getElementById("myform");
    // 可以通过表单节点的属性直接得到input
    var nameField=myform.nameField;
    var ageField=myform.ageField;
    nameField.oninput=function(){
        console.log("正在修改姓名");
    }
    nameField.onchange=function(){
        // 鼠标离开输入框，并且不在输入框时才会被触发
        console.log("已经修改了姓名");
    }
    nameField.onfocus=function(){
        console.log("得到焦点");
    }
    nameField.onblur=function(){
        console.log("失去了焦点");
    }
    myform.onsubmit=function(){
        console.log("你正在尝试提交表单");
    }
</script>
```

![image-20210425154321740](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425154321740.png)

![image-20210425154627554](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425154627554.png)

![image-20210425170449205](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425170449205.png)

![image-20210425170616058](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425170616058.png)

![image-20210425170931589](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425170931589.png)

```html
<div id="box1">
    <div id="box2">
        <div id="box3"></div>
    </div>
</div>
<script>
    var box1=document.getElementById("box1");
    var box2=document.getElementById("box2");
    var box3=document.getElementById("box3");
    
    box1.addEventListener('click',function(){
        console.log('我是box1的捕获阶段');
    },true);
    box2.addEventListener('click',function(){
        console.log('我是box2的捕获阶段');
    },true);
    box3.addEventListener('click',function(){
        console.log('我是box3的捕获阶段');
    },true);
    box1.addEventListener('click',function(){
        console.log('我是box1的冒泡阶段');
    },false);
    box2.addEventListener('click',function(){
        console.log('我是box2的冒泡阶段');
    },false);
    box3.addEventListener('click',function(){
        console.log('我是box3的冒泡阶段');
    },false);
</script>
```

![image-20210425171644291](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425171644291.png)

**注意** ：最内层的盒子不再区分冒泡和捕获的顺序，因此如果先写onclick，会先执行onclick，先写addEventListener会先执行addEventListener。

![image-20210425172323398](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425172323398.png)

![image-20210425174106649](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425174106649.png)



### 鼠标位置

client ：相对于浏览器的位置；page相对于整个网页的位置；offset相对于事件源的位置。

![image-20210425174306992](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425174306992.png)

![image-20210425174613199](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425174613199.png)

![image-20210425174702163](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425174702163.png)

![image-20210425174718752](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425174718752.png)



```js
var box=document.getElementById("box");
var info=document.getElementById("info");
box.onmousemove=function(e){
    info.innerHTML="offsetX/Y: "+e.offsetX+", "+e.offsetY;
}
```

![头 分](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/%E5%A4%B4%20%E5%88%86.gif)

注意：监听鼠标相对元素的的位置时，如果元素内部还有元素，则鼠标进入元素内部的元素时是相对元素内部的元素位置。

![image-20210425181302457](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425181302457.png)

![image-20210425181343152](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425181343152.png)

![image-20210425181502749](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425181502749.png)

使小方块移动：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <style>
        #box{
            position: absolute;
            top: 200px;
            left: 200px;
            width: 100px;
            height: 100px;
            background-color:orangered;
        }
    </style>
</head>
<body>
    <div id="box"></div>
    <script>
        var box=document.getElementById("box");
        // 全局变量t和l表示box的top和left值
        var t=200;
        var l=200;
        //监听document对象的键盘按键
        document.onkeydown=function(e){
            switch(e.keyCode){
                case 37:
                    l-=6;
                    break;
                case 38:
                    t-=6;
                    break;
                case 39:
                    l+=6;
                    break;
                case 40:
                    t+=6;
                    break;
            }
            box.style.top=t+"px";
            box.style.left=l+"px";
        } 
    </script>
</body>
</html>
```

![image-20210425184317001](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425184317001.png)

![image-20210425184330354](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425184330354.png)

```html
<p>
    <input type="text" id="field">
</p>
<script>
    var field = document.getElementById("field");
    field.onkeypress = function (e) {
        // 用e.charCode判断用户是否输入的小写字母或者数字
        // 数字0~9，字符码是48~57
        // 小写字母，字符码97~122
        if (!(e.charCode >= 48 && e.charCode <= 57 || e.charCode >= 97 && e.charCode <= 122)) {
            e.preventDefault();
            // 默认是在input按键会输入文字，用preventEvent()方法阻止默认
        }
    }
</script>
```

ps:只能阻止英文键盘时默认操作。

![image-20210425185814339](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425185814339.png)

```html
<div id="box">
</div>
<h1 id="info">0</h1>
<script>
    var oBox=document.getElementById("box");
    var oinfo=document.getElementById("info");
    // 全局变量就是info中显示的数字
    var a=0;
    // 给box盒子添加鼠标滚轮事件监听
    oBox.onmousewheel=function(e){
        // 阻止默认事件：就是说当用户在盒子里面滚动鼠标滚轮的时候，此时不会引发页面的滚动条的滚动
        e.preventDefault();
        if(e.deltaY>0){
            a++;
        }else{
            a--;
        }
        oinfo.innerText=a;
    }
</script>
```

![image-20210425191934115](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425191934115.png)

```html
<div id="box">
    <button id="btn">按我</button>
</div>
<script>
    var box=document.getElementById("box");
    var btn=document.getElementById("btn");
    // box.onclick=function (){
    //     console.log("我是盒子")
    // }
    // btn.onclick=function (e){
    //     e.stopPropagation();
    //     console.log("我是按钮")
    // } //此时会显示 我是按钮
    box.addEventListener("click",function(e){
        e.stopPropagation();
        console.log('我是盒子');
    },true)
    btn.addEventListener("click",function(){
        console.log('我是按钮');
    },true) //此时会显示我是盒子
```

注意stopPropagation()方法添加的位置，不是被阻止的元素，而是需要被阻止的元素的上一个元素。

![image-20210425193253686](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210425193253686.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <style>
        .modal{
            width: 400px;
            height: 140px;
            background-color: #333;
            position: absolute;
            top: 50%;
            left: 50%;
            margin-top: -70px;
            margin-left: -200px;
            display: none;
        }
    </style>
</head>
<body>
    <button id="btn">点我弹出弹出层</button>
    <div class="modal" id="modal"></div>
    <script>
        var btn=document.getElementById("btn");
        var modal=document.getElementById("modal");
        // 点击按钮的时候，弹出层出现
        btn.onclick=function(e){
            modal.style.display="block";
            e.stopPropagation();
            // 如果不添加的该方法，点击事件会传播到document.onclick,会导致modal显示后又瞬间关闭

        }
        // 点击页面任何的时候，弹出层关闭
        document.onclick=function(){
            modal.style.display="none";
            
        }
        //点击弹出层内部是不能关闭弹出层的
        modal.onclick=function(e){
            // 阻止事件传播到document
            e.stopPropagation();
        }
    </script>
</body>
</html>
```

![image-20210422145249895](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422145249895.png)

![image-20210422145322365](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422145322365.png)

```javascript
fun(); //hahahaha
function fun(){
    console.log('hahahah');
}
fun2();//Uncaught TypeError: fun2 is not a function
var fun2=function(){
    console.log('hahahah');
}
```

将匿名函数赋值给了fun2()，但赋值是不能被hoist，所以调用fun2();相当于undefined。

![image-20210422150254424](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422150254424.png)

![image-20210422150955082](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422150955082.png)

![image-20210422151023439](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422151023439.png)

```js
function add(a,b){
    var sum=a+b;
    console.log(sum);
}
add(2,3);
add(3,7);
add(3); //NaN
```

![image-20210422151811503](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422151811503.png)

```js
function fun(){
    console.log(arguments[0]); //12
    console.log(arguments[1]);//22
}
fun(12,22,29);

//不管用户输入了多少数据都能计算和
function fun(){
    var sum=0;
    for(var i=0,sum=0; i<arguments.length;i++){
        sum+=arguments[i];
    }
    console.log(sum);
}

fun(1,2,3,4,5);
```

![image-20210422154029087](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422154029087.png)

![image-20210422154730077](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422154730077.png)

```js
// 喇叭花数 一个三位数abc=a!+b!+c!

//求阶乘
function factorial(b){
    for(var i=1,res=1;i<=b;i++){
        res*=i;
    }
    return res;
}
function labahua(){
    for(var i=100;i<=999;i++){
        var a=factorial(Number(i.toString().charAt(0)));
        var b=factorial(Number(i.toString().charAt(1)));
        var c=factorial(Number(i.toString().charAt(2)));
        if (i==(a+b+c)){
            console.log(i);
        }
    }
}
labahua();//145
```

![image-20210422160633867](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422160633867.png)

![image-20210422160742824](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422160742824.png)

```js
var arr=[12,42,43,53,25];
// 排序
arr.sort(
    function(a,b){
        return a-b;
    }
)
```

![image-20210422161609340](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422161609340.png)

![image-20210422161929411](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422161929411.png)

![image-20210422162035276](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422162035276.png)



```js
function factorial(n) {
    if (n == 1) {
    //递归的出口
        return 1;
    }
    return n * factorial(n - 1);
}

// 斐波拉契数列
function Fibonacci(n){
    if(n==1||n==2){
        return 1;
    }
    return Fibonacci(n-1)+Fibonacci(n-2);
}

console.log(Fibonacci(5));
```

![image-20210422164917380](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422164917380.png)

```js
// 原数组
var arr1=[33,44,11,77,[13,39]]; 
// 函数递归
function deepClone(arr){
    var arr2=[];
    for(var i=0;i<arr.length;i++){
        if(!Array.isArray(arr[i])){
            arr2.push(arr1[i]);
        }
        else{
            arr2.push(deepClone(arr[i]));
        }
    }
    return arr2;
}
var arr2=deepClone(arr1);
console.log(arr2);
```

![image-20210422195409072](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422195409072.png)



```js
// 全局变量
var a=10;
function fun(){
    var a=5;
    a++;
    console.log(a);
}
fun();//6
console.log(a);//10

// 全局变量
var m=10;
function fun(){
    m++; // 下方的m声明被提升了 m为undefined m++为NaN
    var m; 
    console.log(a);
}
fun();//NaN
console.log(m);//10
```

![image-20210422200146588](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422200146588.png)

![image-20210422202157590](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422202157590.png)

```js
var a=10;
var b=20;
function fun(){
    var c=30;
    function inner(){
        var a=40;
        var d=50;
        console.log(a,b,c,d);
    }
    inner(); // 此时需要在内部调用inner();
}
fun();
```

![image-20210422203136098](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422203136098.png)

需要调用函数fun(),才会定义a。

![image-20210422204520582](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422204520582.png)

![image-20210422204555571](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422204555571.png)

```js
function fun(){
    var name="慕课网";
    function innerFun(){
        alert(name);
    }
    // 返回一个局部函数,注意并没有加括号
    return innerFun;
}
// 调用外部函数，用变量inn来接受返回值
var inn=fun();
var name="hahaha";
// 执行inn函数，相当于在fun函数的外部执行了内部函数
inn();  //  “慕课网”弹窗
```

![image-20210422204720475](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422204720475.png)

![image-20210422204834706](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422204834706.png)

![image-20210422204905121](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422204905121.png)

![image-20210422205042419](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422205042419.png)

```js
function checkTemp(standard){
    function temp1(n){
        if(n<=standard){
            alert("你的体温正常");
        }else{
            alert("体温偏高")
        }
    }
    return temp1;
}
var checkA=checkTemp(37.2);
var checkB=checkTemp(37.3);
checkA(37.1); //会记住standard的值 体温正常
checkB(37.3); //会记住standard的值 体温正常
```

![image-20210422210317329](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422210317329.png)

```js
// 封装一个私有化变量的函数
function fun(){
    // 定义局部变量a
    var a=0;
    return {
        getA:function(){
            return a;
        },
        add:function(){
            a++;
        }
    }
}
var obj=fun();
console.log(obj.getA());
obj.add();
console.log(obj.getA());
```

![image-20210422223822932](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422223822932.png)

![image-20210422224248221](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422224248221.png)

![image-20210422224359706](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422224359706.png)

![image-20210422224618067](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422224618067.png)

```js
var age=12;
var sex='男';
var title=(function(){
    if(age<18){
    return "小朋友";
}
else{
    if(sex=='男'){
        return "先生";
    }
    else{
        return "女士";
    }
}
})();
alert(title);
```

![image-20210422225524799](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422225524799.png)

变量i真的是全局变量。

![image-20210422230323224](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210422230323224.png)

```js
var arr=[];
for(var i=0;i<5;i++){
    arr.push(function(){
        alert(i);
    });
}
arr[2](); //弹窗5

var arr=[];
for(var i=0;i<5;i++){
    (function(i){
        arr.push(
            function(){
                alert(i);
            }
        )
    })(i);
}
//若for循环里的第一层函数不写成IIFE，是不会被执行的，会被直接跳过
var arr=[];
for(var i=0;i<5;i++){
    function a(i){
        arr.push(
            function(){
                alert(i);
            }
        )
    };
    a(i);
}
//不过可以写成这样，总之函数一定要被调用才会执行。不会因为它在for循环里，就自动执行了。
```







![image-20210408105225337](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210408105225337.png)

![image-20210408105438682](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210408105438682.png)

![image-20210408105802666](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210408105802666.png)

![image-20210408105843556](C:\Users\24734\AppData\Roaming\Typora\typora-user-images\image-20210408105843556.png

![image-20210410105159714](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410105159714.png)

![image-20210410105247667](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410105247667.png)

![image-20210410105620746](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410105620746.png)

![image-20210410105542856](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410105542856.png)

靠谱方法是第二和第四个

![image-20210410105813366](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410105813366.png)

![image-20210410110013827](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410110013827.png)

盒子上下边距塌陷

![image-20210410110353802](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410110353802.png)

![image-20210410111501896](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410111501896.png)

![image-20210410112002227](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410112002227.png)

使用clear:both; 后面的盒子设置margin无法对设置了浮动的盒子生效。

![image-20210410112423369](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410112423369.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        p{
            float: left;
            background-color: yellow;
            width: 200px;
            height: 200px;
            margin:0 10px 10px 0;
        }
        
        .clearfix::before{
            content: "";
            clear: both;
            /* 转为块级元素 */
            display: block;
        }
        
    </style>
</head>
<body>
    <div>
        <p></p>
        <p></p>
    </div>
    <div class="clearfix">
        <p></p>
        <p></p>
    </div>
</body>
</html>
```

此处是第2个div对第1个div清除浮动，所以在第二个div使用before伪元素，如果是第1个div对第2个div清除浮动，则在第一个div使用after伪元素。

![image-20210410113016497](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410113016497.png)

```html
<div>
    <p></p>
    <p></p>
</div>
<div style="clear:both"></div>
<div>
    <p></p>
    <p></p>
</div>
```

![image-20210410113532801](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410113532801.png)

![image-20210410115353142](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410115353142.png)

```html
<style>
    div{
        width: 400px;
        height: 400px;
        border: 1px solid black;
        margin: 40px auto;
    }
    p{
        width: 100px;
        height: 100px;
        background-color: orange;
        position: relative;
        top: 100px;
        left: 100px;
    }
</style>
<body>
    <div>
        <p></p>
    </div>
</body>
```

<img src="https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410115528112.png" alt="image-20210410115528112" style="zoom:25%;" />

![image-20210410115641404](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410115641404.png)

相对定位不会脱离标准流

![image-20210410115908962](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410115908962.png)

![image-20210410121414043](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410121414043.png)

非常酷，值得再试

![image-20210410121514541](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410121514541.png)

![image-20210410122025627](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410122025627.png)

![image-20210410122545843](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410122545843.png)

![image-20210410122906749](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410122906749.png)

当然父元素也可以是绝对定位。

![image-20210410140559231](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410140559231.png)

```css
width: 80px;
height: 80px;
background-color: orange;
position: absolute;
top:50%; /*上边线居中了*/
margin-top: -40px; /*往上移盒子高度的一半*/
left:50%;
margin-left: -40px;
```

![image-20210410145827212](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410145827212.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            width: 300px;
            height: 300px;
            background-color: orange;
            position: absolute;
            top: 100px;
            left: 100px;
            z-index: 999;/*可以随便设置，大的index压住小的index*/
        }
        .box2{
            width: 300px;
            height: 300px;
            background-color:blue;
            position: absolute;
            top: 200px;
            left: 200px;
            z-index: 99;
        }
    </style>
</head>
<body>
    <div class="box1"></div>
    <div class="box2"></div>
</body>
</html>
```

两个盒子同时为绝对定位时，设置的z-index属性较大的盒子会在上方。



![image-20210410194920209](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410194920209.png)

当有元素压在其他元素的上方时，绝对定位很好用。

![image-20210410194856699](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410194856699.png)

![image-20210410195053040](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210410195053040.png)

![image-20210412093529799](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412093529799.png)

当宽高相同时，设置border-radius为50%或者宽高的一半可以得到正圆。

![image-20210412094710227](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412094710227.png)

![image-20210412094726695](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412094726695.png)

![image-20210412095050316](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412095050316.png)

![image-20210412095129948](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412095129948.png)

![image-20210412095343406](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412095343406.png)

![image-20210412095923905](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412095923905.png)

同时向4周延展

![image-20210412100245107](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412100245107.png)

![image-20210412100318323](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412100318323.png)

![image-20210412100511868](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412100511868.png)

用边框制作三角形

![image-20210412103058413](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412103058413.png)

将盒子的宽高设为0，其他边框设置为透明

```css
.box{
    border: 20px solid transparent;
    border-top-color: red;
    width: 0;
    height: 0;
}
```

![image-20210412111341819](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412111341819.png)

| 值        | 意义               |
| --------- | ------------------ |
| repeat    | x、y均平铺（默认） |
| repeat-x  | x平铺              |
| repeat-y  | y平铺              |
| no-repeat | 不平铺             |



![image-20210412112022699](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412112022699.png)

注意：这是用来设置背景图片的尺寸的，而不是设置背景尺寸

![image-20210412112506763](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412112506763.png)

![image-20210412120538266](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412120538266.png)

![image-20210412121554589](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412121554589.png)

![image-20210412122026775](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210412122026775.png)

![image-20210413201252361](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413201252361.png)

/* 距离盒子左边100px 上边距20px*/

      background-position: 100px 20px;

![image-20210413202242665](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413202242665.png)

用PS切片工具选中想要的图标，然后双击可以可以显示位置和宽高。

![image-20210413204514669](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413204514669.png)

![image-20210413205319488](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413205319488.png)

```css
.pic{
    position: absolute;
    width: 80px;
    height: 69px;           
    background-image: url(img/sprites.png);
    background-position: 0 -1820px;
}
```

注意background-position要设置为-x和-y

![image-20210413213251345](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413213251345.png)

![image-20210413213527110](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413213527110.png)

![image-20210413213637273](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413213637273.png)

![image-20210413213612993](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413213612993.png)

![image-20210413214527297](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413214527297.png)

```css
.box2{
    width: 200px;
    height: 200px;
    background-image: -webkit-linear-gradient(45deg,black,white);
}
```

![image-20210413214903802](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413214903802.png)

![image-20210413222007678](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413222007678.png)

默认情况，变形的原点在元素的中心点，或者是元素X轴和Y轴的50%处。

![image-20210413223029940](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413223029940.png)

transform-origin: 20% 40%; (把距离盒子左边20%，上边40%的点作为旋转点)

transform-origin: 0 0 ; 以左上角作为旋转中心



![image-20210413223755796](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413223755796.png)

![image-20210413224137454](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210413224137454.png)

注意单位是deg而不是px！

![image-20210414100538234](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210414100538234.png)

![image-20210414101814959](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210414101814959.png)

![image-20210414102015136](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210414102015136.png)

给父标签设置perspective属性，旋转才会有透视，才能看到旋转。

transform: rotateX(30deg) rotateY(50deg); 同时绕着x轴和y轴旋转。

![image-20210414103153740](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210414103153740.png)

![image-20210414103223875](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210414103223875.png)

此时是沿着旋转后方向进行移动，而不是沿着原先的坐标轴。

![1111软](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/1111%E8%BD%AF.gif)

z轴本来是垂直于屏幕且正方向指向我的，盒子旋转后此时的Z轴为垂直于盒子的。因此transform: translateZ(100px); 会让盒子靠近沿着此时的Z轴移动100px，更加地靠近我，因此会觉得盒子变大了！。

![1111软给](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/1111%E8%BD%AF%E7%BB%99.gif)

![image-20210414110507545](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210414110507545.png)

![image-20210414110612229](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210414110612229.png)



![image-20210415103901991](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210415103901991.png)

![image-20210415112633454](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210415112633454.png)

![image-20210415104451595](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210415104451595.png)

注意即使是0也要写单位。

```css
.box1{
    width: 200px;
    height: 200px;
    background-color: orange;
    transition: width 2s linear 0s; /*注意是把transition写在这里而不是伪类那儿*/
}
.box1:hover{
    width: 800px;
}
.box2 p{
    width: 200px;
    height: 200px;
    background-color: orange;
    position: relative;
    transition: left 3s linear 0s;
    left: 0; /*注意要写left:0;*/
}

.box2:hover p{
    left: 1000px;
}

```

![image-20210415105023187](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210415105023187.png)



尽量把要变换的属性前后的值都写上。

![image-20210415112139993](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210415112139993.png)

all不要滥用，会降低运行效率。

![image-20210415112808926](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210415112808926.png)

https://cubic-bezier.com/可以生成贝塞尔曲线，可以自定义动画缓动参数。

![image-20210415164801312](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210415164801312.png)

![image-20210415164927269](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210415164927269.png)

![image-20210415164951475](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210415164951475.png)

![image-20210415165053776](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210415165053776.png)

![image-20210415170343293](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210415170343293.png)

百分数表示运行时间。

![image-20210415175222343](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210415175222343.png)

有点难度



百度搜索YUI reset 清除html标签的默认样式。如ul标签的小原点，h1的加粗。

![image-20210415215639610](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210415215639610.png)

定义字体

```css
@font-face {
    font-family: "myFont";
    src: url(../fonts/PingFangSCRegular.ttf) format('truetype');
}
```

format ：字体的格式，主要用于浏览器识别，一般有以下几种——truetype, opentype, truetype-aat, embedded-opentype, avg等。

**TrueType格式(.ttf)**
Windows和Mac上常见的字体格式，是一种原始格式，因此它并没有为网页进行优化处理。

文本框的outline

![image-20210419214950750](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210419214950750.png)

后一项是前一项多乘以了一个数

水仙花数

```javascript
// 寻找水仙花数
for(var i=100;i<1000;i++){
    // a,b,c 分别表示百位，十位，个位
    var i_str=i.toString();
    var a=i_str.charAt(0);
    var b=i_str.charAt(1);
    var c=i_str.charAt(2);
    // 输出a,b,c 控制台一行输出a,b,c 用空格隔开
    //console.log(a,b,c);
    if(i==(Math.pow(a,3)+Math.pow(b,3)+Math.pow(c,3))){
        console.log(i)
    }
}
```

![image-20210420152648683](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420152648683.png)

```javascript
var a=[1,2,3,4,5];
a.join("☆");//1☆2☆3☆4☆5
a.join("");//12345
a.join( );//1,2,3,4,5

var b="abcdef";
b.split(""); //["a", "b", "c", "d", "e", "f"]
b.split( ); //["abcdef"]
var c="a-b-c-d-e-f";
console.log(c.split("-")); // ["a", "b", "c", "d", "e", "f"];
```

![image-20210420154010847](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420154010847.png)

![image-20210420154039842](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420154039842.png)

![image-20210420154436903](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420154436903.png)

![image-20210420154309432](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420154309432.png)

```html
['a','b','c','d','b'].indexOf('b');//1
['a','b','c','d','b'].lastIndexOf('b'); //4
['a','b','c','d','b'].includes('b');//true
```

![image-20210420161208732](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420161208732.png)

![image-20210420161411897](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420161411897.png)

```javascript
var arr=[1,2,4,1,3,2,4,6,4];
var res=[];
for(var i=0;i<arr.length;i++){
    // 判断arr的元素是否在res数组中
    //includes判断元素是否在数组中，返回true或者false
    if(!res.includes(arr[i])){
        res.push(arr[i]);
    }
}
console.log(res);//[1, 2, 4, 3, 6]
```

![image-20210420162319471](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420162319471.png)

```html
var a=[15,43,67,25,47,63,89,34,48];
var result=[];

for(var i=0;i<3;i++){
    // 随机选择下标
    // 使用parseInt(Math.random()*(b-a+1)+a); 产生[a,b]的随机数
    var n=parseInt(Math.random()*a.length); //生成[0,length-1]的整数
    // 把这项添加到结果数组
    result.push(a[n]);
    // 同时删除原数组中的这项，防止被重复选择
    a.splice(n,1);
}
console.log(result); // [47, 25, 43]
```

![image-20210420173210504](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420173210504.png)

![image-20210420173323559](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420173323559.png)

![image-20210420173344084](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420173344084.png)

![image-20210420173403948](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420173403948.png)

```javascript
// 冒泡排序
var arr=[6,2,9,3,8,1];
// 一趟一趟的进行比较
// 用i表示需要比较的趟数，6个数字需要比较5趟
for(var i=1;i<arr.length;i++){
    // 内层循环负责两两比较
    for(var j=arr.length-1;j>=i;j--){
        // 判断项的大小
        if(arr[j]<arr[j-1]){
            // 交换变量位置
            var temp=arr[j];
            arr[j]=arr[j-1];
            arr[j-1]=temp;
        }
    }
}
console.log(arr);// [1, 2, 3, 6, 8, 9]
```

![image-20210420194840049](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420194840049.png)

```javascript
var matrix=[[11,33,55],[12,31,43,33],[12,43,53]];
console.log(matrix.length); //3
for(var i=0;i<3;i++){
    for(var j=0; j<3;j++){
        console.log(matrix[i][j]);
    }
}
```

![image-20210420195616887](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420195616887.png)

![image-20210420195519427](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420195519427.png)

![image-20210420195819441](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420195819441.png)

![image-20210420195922769](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420195922769.png)

![image-20210420200006576](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420200006576.png)

![image-20210420200117247](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420200117247.png)

![image-20210420200206636](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420200206636.png)

![image-20210420200333343](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/image-20210420200333343.png)

```js
var arr1=[1,2,3,4];
// var arr2=arr1; 不会克隆arr1的值
var result =[];
//遍历arr1的每一项，把遍历的项推入结果数组中
for(var i=0;i<arr1.length;i++){
    result.push(arr1[i]);
}
console.log(result); //[1,2,3,4]
//实现了浅克隆，值相等，而引用地址不相同
console.log(result==arr1); // false

var arr1=[1,2,3,4,[8,9,4]];
var result =[];
for(var i=0;i<arr1.length;i++){
    result.push(arr1[i]);
}
console.log(result[4]==arr1[4]); //true，只实现了浅克隆，第5个元素没有被克隆，所以返回true
```
