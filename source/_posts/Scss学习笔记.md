---
title: Scss学习笔记
date: 2021-09-24 22:12:10
tags:
---

在 vscode 中，首先需要安装 `live sass compiler`，将 sass 实时编译为 css 。

![image-20210922223456637](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109222235743.png)

点击底部的 watch sass，即可将 sass 文件实时编译为同名的 css 文件。

![image-20210922223702754](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109222237909.png)



## 嵌套

我觉得 sass 中最重要的功能就是可以像 html 中进行层层嵌套，而不用像css中选择某一个元素，需要写多个父级标签。

```html
<header>
    <h1>hello my dear</h1>
    <button>Hello</button>
</header>
<div class="contact">
    <button>submit</button>
    <div class="info">Our contact info</div>
    <p>This is our info</p>
</div>
```

```scss
// scss
header{
    button{
        color:blue;
    }
}
```

```css
/*css*/

header blue{
    color:blue;
}
```

当有多层级的html时，scss 的嵌套功能就比css好用太多了。当**然，在scss中，是完全支持普通css的写法！**

另外，如果想要写鼠标悬停按钮的效果(`hover`)，用 scss 可以这样实现。

```scss
header{
    button{
        color:blue;
        &:hover{
            color:pink;
        }
    }
}
```

是不是又可以少写好多标签？

同理，如果想给 button 添加伪元素 也可以直接在 button 的样式内用 `&` 来连接 before 或者 after。

```js
header {
    button {
        color: blue;
        &:hover {
            color: blue;
        }
        &::after{ // 也可以只写一个: 如 &:after
            content:'hi';
            color:red;
        }
    }
}
```

## 定义变量

当有某个值需要多次使用时，可以用 `$` 符号修饰它，并为它指定值。

```scss
$primaryBtn: red;

// 接着就可以调用这个变量

header{
    button{
        color: $primaryBtn; 
    }
}
```

当然css也可以实现这个功能：

```css
*{
    --primaryBtn:red;
}

header button{
    color:var(--primaryBtn);
}
```



## 拆分 scss 文件

比如我现在的scss文件如下：

```scss
$primaryBtn:red;
$text:rgb(31, 31, 190);


header {
    color: $text;
    display: flex;
    justify-content: center;

    button {
        color: $primaryBtn;
        &:hover {
            color: blue;
        }
        &::after{
            content:'hi';
            color:red;
        }
    }
}

.contact button{
    background-color: $primaryBtn;
    color:$text;
}
```

此时，我想将 header 的样式分出去单独写。但在 css 中是没有像 JavaScript中 ES6 新增的 模块化语法。不过，sass呢却支持这种模块化语法。



首先，给分出去的header创建一个新的文件，名字为 `_header.scss` ，用下划线来表示它是模块（可以不加）。

接下来需要在核心的sass中导入这个模块。

```scss
$primaryBtn:red;
$text:rgb(31, 31, 190);

@import './header'; //后缀不用写，下划线也不用写

.contact button{
    background-color: $primaryBtn;
    color:$text;
}
```



## 自定义代码块

比如如果我想要header中的内容居中，常见的写法就是用flex来实现。

```scss
header{
    display: flex;
    justify-content: center;
    align-items: center
}
```

与此同时，我还想要别的地方内容也居中，于是我就会不断重复上面三行css代码。

因此，如果css中能像js一样自定义函数，不就可以复用代码了。

很遗憾，css目前并不支持这样做，但是scss却可以实现这一功能。

```scss
// 定义代码块
@mixin flexCenter {
    display: flex;
    justify-content: center;
    align-items: center;
} 

// 调用代码块
header {
    height: 100vh;
    @include flexCenter(); 
}
```

另外，还可以给定义的代码块进行传参，如我想要改变 justify-content 的值为 flex-start：

```scss
@mixin flexCenter($justify) { // 注意定义变量以 $ 开头
    display: flex;
    justify-content: $justify;
    align-items: center;
}


header {
    height: 100vh;
    @include flexCenter(flex-start); // 调用代码块时传参
}
```

当然，也可以定义多个参数：

```scss
@mixin flexCenter($justify,$direction) {
    display: flex;
    justify-content: $justify;
    align-items: center;
    flex-direction: $direction;
}


header {
    color: $text;
    height: 100vh;
    @include flexCenter(flex-start,column);
}
```

### 继承

现在，我已经写好了header的样式，接下来要写footer的样式，footer和header的大体样式相同。因此，我可以复制header的样式并粘贴到footer。

但scss给我们提供了更简单的方法，那就是使用关键词 extend。如果footer中有些样式不同，我们可以直接覆盖原来的样式。

```scss
header{
    color: pink;
    background-color: black;
    font-size: 14px;
}

footer{
    @extend header;
    background-color: red; // 覆盖header的背景颜色
}
```

此时 `live sass compiler` 会将scss编译成这样：

```css
header, footer {
  color: pink;
  background-color: black;
  font-size: 14px;
}

footer {
  background-color: red;
}
```
