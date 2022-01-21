---
title: 了解虚拟DOM
date: 2021-07-31 19:15:43
tags: DOM,JS
---


虚拟 DOM 简单说就是 **用JS对象来模拟 DOM 结构**


## why

因为操作DOM是非常慢的，每个 DOM 元素都继承了非常多的属性。

比如创建一个 div元素，它自己尽管是没有任何属性，但是它的原型链超级超级长！

HTMLDivElement -> HTMLElement -> Element -> Node -> EventTarget->{一个对象}

![](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20211217150719.png)

如果把它继承的属性打印出来，会发现可以绕地球2圈。

![](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20211217151132.png)



不过，这都不是关键，最重要的是直接操作dom，浏览器的重绘和重排会很消耗性能。

## 虚拟DOM原理

因此，相对于 DOM 对象，原生的 JavaScript 对象处理起来更快。DOM 树上的结构、属性信息我们都可以很容易地用 JavaScript 对象表示出来：


```js
const element = {
  tagName: 'ul', // 节点标签名
  props: { // DOM的属性，用一个对象存储键值对
    id: 'list'
  },
  children: [ // 该节点的子节点
    {tagName: 'li', props: {class: 'item'}, children: ["Item 1"]},
    {tagName: 'li', props: {class: 'item'}, children: ["Item 2"]},
    {tagName: 'li', props: {class: 'item'}, children: ["Item 3"]},
  ]
}
```


这个对象可以有三个属性：`tag`、`props`、`children`:

-   tag：必选。就是标签。也可以是组件，或者函数
-   props：非必选。就是这个标签上的属性和方法
-   children：非必选。就是这个标签的内容或者子节点。

上面对应的HTML写法是：

```html
<ul id='list'>
  <li class='item'>Item 1</li>
  <li class='item'>Item 2</li>
  <li class='item'>Item 3</li>
</ul>
```


用 JavaScript 对象表示 DOM 信息和结构，当状态变更的时候，重新渲染这个 JavaScript 的对象结构。用新渲染的对象树去和旧的树进行对比，记录这两棵树差异。记录下来的不同就是我们需要对页面真正的 DOM 操作，然后把它们应用在真正的 DOM 树上，页面就变更了。

这样就可以做到：视图的结构确实是整个全新渲染了，但是最后操作DOM的时候确实只变更有不同的地方。

这就是所谓的 Virtual DOM 算法。包括几个步骤：

1.  用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中
2.  当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异
3.  把2所记录的差异应用到步骤1所构建的真正的DOM树上，视图就更新了


## 实现

### 步骤一：用JS对象模拟DOM树

首先写一个构造函数，然后创建实现上面的 dom对象。

```js
function Element(tagName,props,children){
 this.tagName = tagName;
 this.props = props;
 this.children = children;
}

```

接着，用对象来表示：

```js
const ul = new Element('ul',{id:'list'},[
 new Element('li',{class:'item'},['Item 1']),
 new Element('li',{class:'item'},['Item 2']),
 new Element('li',{class:'item'},['Item 2']),
]);
```

现在`ul`只是一个 JavaScript 对象表示的 DOM 结构，页面上并没有这个结构。我们可以根据这个`ul`构建真正的`<ul>` 。

```js
Element.prototype.render = function(){
 // 根据tag创建元素
 const el = document.createElement(this.tagName);

 // 设置DOM属性
 for(let propName in this.props){
 el.setAttribute(propName,this.props[propName]);
 }
 // 递归el的children属性
 const children = this.children || [];
 children.forEach(child => {
 const childEl = (child instanceof Element)?
 child.render() //如果子节点也是虚拟DOM，递归构建DOM节点
 : document.createTextNode(child) // 如果字符串，只构建文本节点
 el.appendChild(childEl);
 })
 return el;
}
```

`render`方法会根据`tagName`构建一个真正的DOM节点，然后设置这个节点的属性，最后递归地把自己的子节点也构建起来。所以只需要：

```js
  
const ulDom = ul.render();
document.body.appendChild(ulDom);

```

### 步骤二：比较两棵虚拟DOM树的差异

正如你所预料的，比较两棵DOM树的差异是 Virtual DOM 算法最核心的部分，这也是所谓的 Virtual DOM 的 diff 算法。两个树的完全的 diff 算法是一个时间复杂度为 O(n^3) 的问题。但是在前端当中，你很少会跨越层级地移动DOM元素。

1.  只比较同一层级，不跨级比较
![](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20211217160056.png)
上面的`div`只会和同一层级的`div`对比，第二层级的只会跟第二层级对比。这样算法复杂度就可以达到 O(n)。

#### 1.深度优先遍历，记录差异

在实际的代码中，会对新旧两棵树进行一个深度优先的遍历，这样每个节点都会有一个唯一的标记：
![](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/20211217160224.png)

在深度优先遍历的时候，每遍历到一个节点就把该节点和新的的树进行对比。如果有差异的话就记录到一个对象里面。


#### 2.比较差异类型

上面说的节点的差异指的是什么呢？对 DOM 操作可能会：

1.  替换掉原来的节点，例如把上面的`div`换成了`section`
2.  移动、删除、新增子节点，例如上面`div`的子节点，把`p`和`ul`顺序互换
3.  修改了节点的属性
4.  对于文本节点，文本内容可能会改变。例如修改上面的文本节点2内容为`Virtual DOM 2`。

### 步骤三：把差异应用到真正的DOM树上

因为步骤一所构建的 JavaScript 对象树和`render`出来真正的DOM树的信息、结构是一样的。所以我们可以对那棵DOM树也进行深度优先的遍历，遍历的时候从步骤二生成的`patches`对象中找出当前遍历的节点差异，然后进行 DOM 操作。

所以，最重要的就是 比较差异类型，也就是[[DIFF算法]]。

参考：
1. https://github.com/livoras/blog/issues/13
2. https://juejin.cn/post/7010594233253888013