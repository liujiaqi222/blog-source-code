---
title: 前端工程化--webpack
date: 2021-08-04 19:38:17
tags: [webpack]
---



# 前端工程化

前端工程化指的是：在企业级的前端项目开发中，把前端开发所需的工具、技术、流程、经验等进行规范化、标准化。

最终落实到细节上，就是实现前端的“4 个现代化”：**模块化、组件化、规范化、自动化**。

目前主流的前端工程化解决方案：

- webpack（ https://www.webpackjs.com/ ）

- parcel（ https://zh.parceljs.org/ ） 

# webpack

## 1. 什么是 webpack

概念：webpack 是前端项目工程化的具体解决方案。

主要功能：它提供了友好的前端模块化开发支持，以及代码压缩混淆、处理浏览器端 JavaScript 的兼容性、性能优化等强大的功能。

好处：让程序员把工作的重心放到具体功能的实现上，提高了前端开发效率和项目的可维护性。

注意：目前企业级的前端项目开发中，绝大多数的项目都是基于 webpack 进行打包构建的。  

## 2. 创建列表隔行变色项目

① 新建项目空白目录，并运行 `npm init –y` 命令，初始化包管理配置文件 package.json

② 新建 `src` 源代码目录

③ 新建 src / index.html 首页和 src / index.js 脚本文件

④ 初始化首页基本的结构

⑤ 运行 `npm install jquery –S` 命令，安装 jQuery

⑥ 通过 ES6 模块化的方式导入 jQuery，实现列表隔行变色效果  

但是直接如果在js中通过import 导入jQuery 浏览器会报错！

`Uncaught SyntaxError: Cannot use import statement outside a module`

```js
// 使用es6模板化语法导入jquery

import $ from 'jquery'

// 隔行变色
$(function(){
    $('li:odd').css('backgroundColor','red');
    $('li:even').css('backgroundColor','cyan');
})
```

但使用webpack打包后的js，可以在浏览器中正常运行。

## 3.在项目中安装webpack

在终端运行如下的命令，安装 webpack 相关的两个包：  

```bash
npm install webpack@5.5.1 webpack-cli@4.2.0 -D
```

需要安装webpack和webpack-cli。

在安装依赖包时，有一些参数需要注意。比如使用-g参数时，表示该依赖包为全局安装

　　**参数**-S, --save表示安装包信息将加入到dependencies（生产阶段的依赖）

```bash
npm install express --save 或 npm install express -S
```

　　package.json 文件的 dependencies 字段：

```json
"dependencies": {
    "express": "^3.9.0"
}
```

　　**参数**-D, --save-dev表示安装包信息将加入到devDependencies（开发阶段的依赖），所以开发阶段一般使用它

```bash
npm install express --save-dev 或 npm install express -D
```

## 4.在项目中配置 webpack

① 在项目根目录中，创建名为 webpack.config.js 的 webpack 配置文件。

```js

webpack
├─ package-lock.json
├─ package.json
├─ src
│  ├─ index.html
│  └─ index.js
└─ webpack.config.js

```
并初始化如下的基本配置：

```js

//webpack.config.js
module.exports={
    mode:'development'
}
```

② 在 package.json 的 scripts 节点下，新增 dev 脚本如下：

```json
//package.json
{
    "scripts": {
    "dev":"webpack"
  }
}
```

'dev'名字是自定义的，但是webpack是固定的。

③ 在终端中运行 `npm run dev` 命令，启动 webpack 进行项目的打包构建  

### 4.1 mode的可选值

mode 节点的可选值有两个，分别是：
① development

- 开发环境

- 不会对打包生成的文件进行代码压缩和性能优化

- 打包速度快，适合在开发阶段使用

② production

- 生产环境

- 会对打包生成的文件进行代码压缩和性能优化

- 打包速度很慢，仅适合在项目发布阶段使用  

### 4.2  webpack.config.js 文件的作用

**webpack.config.js 是 webpack 的配置文件。**webpack 在真正开始打包构建之前，会先读取这个配置文件，从而基于给定的配置，对项目进行打包。

注意：由于 webpack 是**基于 node.js 开发出来的**打包工具，因此在它的配置文件中，支持使用 node.js 相关的语法和模块进行 webpack 的个性化配置。  

### 4.3 webpack 中的默认约定

在 webpack 中有如下的默认约定：

① 默认的打包入口文件为 src/index.js

② 默认的输出文件路径为 dist/main.js  

注意：可以在 webpack.config.js 中修改打包的默认约定。



### 4.4 自定义打包的入口与出口  

在 webpack.config.js 配置文件中，通过 `entry `节点指定打包的入口。通过 `output`节点指定打包的出口。

示例代码如下 ：

```js
const path = require('path');

module.exports = {
    mode: 'development',
    // 指定打包的入口文件
    entry: path.join(__dirname, './src/index.js'),
    output: {
        // 输出目录
        path: path.join(__dirname, './dist'),
        // 输出文件的名称
        filename: 'bundle.js'
    }
}
```

# webpack插件

## 1. webpack 插件的作用

通过安装和配置第三方的插件，可以拓展 webpack 的能力，从而让 webpack 用起来更方便。最常用的webpack 插件有如下两个：  

① webpack-dev-server

- 类似于 node.js 阶段用到的 nodemon 工具
- 每当修改了源代码，webpack 会自动进行项目的打包和构建

② html-webpack-plugin 

- webpack 中的 HTML 插件（类似于一个模板引擎插件）
- 可以通过此插件自定制 index.html 页面的内容  

## 2. webpack-dev-server

webpack-dev-server 可以让 webpack 监听项目源代码的变化，从而进行自动打包构建。  

### 2.1 安装 webpack-dev-server

运行如下的命令，即可在项目中安装此插件：参数**-D, --save-dev表示安装包信息将加入到devDependencies（开发阶段的依赖），所以开发阶段一般使用它。

```bash
npm install webpack-dev-server@3.11.0 -D
```

### 2.2 配置 webpack-dev-server

① 修改 package.json -> scripts 中的 dev 命令如下：

```json
"scripts":{
	"dev":"webpack serve" // script节点下的脚本，通过npm run 执行
}
```

② 再次运行 npm run dev 命令，重新进行项目的打包
③ 在浏览器中访问 http://localhost:8080 地址，查看自动打包效果  

![image-20210804212537956](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108042125081.png)

### 2.3 打包生成的文件哪儿去了？

① 不配置 webpack-dev-server 的情况下，webpack 打包生成的文件，会存放到实际的物理磁盘上

- 严格遵守开发者在 webpack.config.js 中指定配置

- 根据 output 节点指定路径进行存放

② 配置了 webpack-dev-server 之后，**打包生成的文件存放到了内存中**

- 不再根据 output 节点指定的路径，存放到实际的物理磁盘上

- 提高了实时打包输出的性能，因为内存比物理磁盘速度快很多  

### 2.4 生成到内存中的文件该如何访问？

webpack-dev-server 生成到内存中的文件，***默认放到了项目的根目录中***，而且是**虚拟的、不可见的。**

如我可以直接在http://localhost:8080/bundle.js访问到bundle文件。

![image-20210804213748360](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108042137421.png)

但是如果我打开http://localhost:8080/，却发现并没有bundle.js这个文件。因此bundle.js方到了根目录，但是不可见虚拟的。

![image-20210804213908399](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108042139461.png)



- 可以直接用 / 表示项目根目录，后面跟上要访问的文件名称，即可访问内存中的文件
- 例如 /bundle.js 就表示要访问 webpack-dev-server 生成到内存中的 bundle.js 文件  

<img src="https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108042141449.png" alt="image-20210804214105396" style="zoom: 50%;" />

## 3.html-webpack-plugin

`html-webpack-plugin` 是 webpack 中的 HTML 插件，可以通过此插件自定制 index.html 页面的内容。
需求：通过 html-webpack-plugin 插件，将 src 目录下的 index.html 首页，复制到项目根目录中一份！  

### 3.1 安装 html-webpack-plugin

运行如下的命令，即可在项目中安装此插件：  

```js
npm install html-webpack-plugin@4.5.0 -D
```

### 3.2 配置 html-webpack-plugin  

```js
const path = require('path');

// 1.导入HTML插件，得到一个构造函数
const HtmlPlugin=require('html-webpack-plugin');

// 2.创建插件的实例对象
const htmlPlugin=new HtmlPlugin({
    template:'./src/index.html', //指定要复制的文件路径（原文件的存放路径）,
    filename:'./index.html'  //生成的文件存放路径

})

module.exports = {
    mode: 'development',
    // 3.通过plugins节点，使htmlPlugin插件生效
    plugins:[htmlPlugin]
}
```

### 3.3 html-webpack-plugin的注意点

HTML 插件在生成的 index.html **页面的底部**，**自动注入**了打包的 bundle.js 文件  

![image-20210804215836002](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108042158066.png)

## 4.devServer 节点

在 webpack.config.js 配置文件中，可以通过 devServer 节点对 webpack-dev-server 插件进行更多的配置，示例代码如下：  

```js
module.exports = {
    mode: 'development',

    devServer:{
        open:true, //初次打包完成后，自动打开浏览器
        host:'127.0.0.1', // 实时打包前使用的主机地址
        port:80 // 实时打包所用的端口号
    }
}
```

# webpack中的loader

## 1.loader概述

在实际开发过程中，webpack 默认只能打包处理以 .js 后缀名结尾的模块。其他**非 .js 后缀名结尾的模块**，webpack 默认处理不了，**需要调用 loader 加载器才可以正常打包**，否则会报错！

loader 加载器的作用：**协助 webpack 打包处理特定的文件模块**。

比如：
-  css-loader 可以打包处理 .css 相关的文件
-  less-loader 可以打包处理 .less 相关的文件
-  babel-loader 可以打包处理 webpack 无法处理的高级 JS 语法  

## 2.loader的调用过程

![image-20210804221504744](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108042215867.png)

## 3.打包css文件

① 运行 `npm install style-loader css-loader -D` 命令，安装处理 css 文件的 loader

② 在 `webpack.config.js` 的 module / rules 数组中，添加 loader 规则如下  

```js
 const path = require('path');

 module.exports = {
   entry: './src/index.js',
   output: {
     filename: 'bundle.js',
     path: path.resolve(__dirname, 'dist'),
   },
  module: { //所有第三方模块的匹配规则
    rules: [ //文件后缀名的匹配规则
      {
        test: /\.css$/i,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
 };
```

模块 loader 可以链式调用。链中的每个 loader 都将对资源进行转换。**链会逆序执行。** （即多个loader从后往前调用）第一个 loader 将其结果（被转换后的资源）传递给下一个 loader，依此类推。最后，webpack 期望链中的最后的 loader 返回 JavaScript。

应保证 loader 的先后顺序：[`'style-loader'`](https://webpack.docschina.org/loaders/style-loader) 在前，而 [`'css-loader'`](https://webpack.docschina.org/loaders/css-loader) 在后。如果不遵守此约定，webpack 可能会抛出错误。

③在项目中添加一个新的 `style.css` 文件，并将其 import 到我们的 `index.js` 中。现在，在此模块执行过程中，含有 CSS 字符串的 `<style>` 标签，将被插入到 html 文件的 `<head>` 中。

```js
import './css/index.css'

// 使用es6模板化语法导入jquery

import $ from 'jquery'

// 隔行变色
$(function(){
    $('li:odd').css('backgroundColor','black');
    $('li:even').css('backgroundColor','pink');
})
```

要查看 webpack 做了什么，请打开控制台检查页面（不要查看页面源代码，它不会显示结果，因为 `<style>` 标签是由 JavaScript 动态创建的），并查看页面的 head 标签。它应该包含 style 块元素，也就是我们在 `index.js` 中 import 的 css 文件中的样式。

## 4.打包处理less文件

① 运行 `npm i less-loader less -D` 命令
② 在 `webpack.config.js` 的 module/rules 数组中，添加 loader 规则如下：  

```js
module.exports = {
    module: {
        rules: [
            {
                test:/\.less$/,
                use:['style-loader','css-loader','less-loader']
            }
        ]
    }
}
```



less是less-loader的内置依懒项，全部代码：

```js
const path = require('path');

// 1.导入HTML插件，得到一个构造函数
const HtmlPlugin = require('html-webpack-plugin');

// 2.创建插件的实例对象
const htmlPlugin = new HtmlPlugin({
    template: './src/index.html', //指定要复制的文件路径（原文件的存放路径）,
    filename: './index.html'  //生成的文件存放路径

})

module.exports = {
    mode: 'development',
    // 指定打包的入口文件
    entry: path.join(__dirname, './src/index.js'),
    output: {
        // 输出目录
        path: path.join(__dirname, './dist'),
        // 输出文件的名称
        filename: 'bundle.js'
    },
    // 3.通过plugins节点，使htmlPlugin插件生效
    plugins: [htmlPlugin],
    devServer: {
        open: true,
        host: '127.0.0.1',
        port: 80
    },
    // 所有第三方模块的匹配规则
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ['style-loader','css-loader']
            },
            {
                test:/\.less$/,
                use:['style-loader','css-loader','less-loader']
            }
        ]
    }
}
```

## 5. 打包图片

v5已经弃用了`file-loader`和`url-loader`，因此需要使用webpack自带的`asset modules`。

```js
//webpack.config.js

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
      {
        test: /\.(png|svg|jpg|jpeg|gif)$/i,
        type: 'asset/resource',
      },
     ],
   },
 };
```

如何实现的：

js：在 `import MyImage from './my-image.png'` 时，此图像将被处理并添加到 `output` 目录，*并且* `MyImage` 变量将包含该图像在处理后的最终 url。

css：在使用 [css-loader](https://webpack.docschina.org/loaders/css-loader) 时，如前所示，会使用类似过程处理你的 CSS 中的 `url('./my-image.png')`。loader 会识别这是一个本地文件，并将 `'./my-image.png'` 路径，替换为 `output` 目录中图像的最终路径。

html：而 [html-loader](https://webpack.docschina.org/loaders/html-loader) 以相同的方式处理 `<img src="./my-image.png" />`。

## 6. loader的另外一种配置方式

带参数项的 loader 还可以通过对象的方式进行配置：  

```js
{
    test:/\.jpg|png|gif$/,
    use:{
        loader:'url-loader',
        options:{
            limit:222222
        }
    }
}
```

## 7.使用babel处理 js 文件中的高级语法

webpack 只能打包处理一部分高级的 JavaScript 语法。对于那些 webpack 无法处理的高级 js 语法，需要借助于 babel-loader 进行打包处理。

### 7.1 安装 babel-loader 相关的包

运行如下的命令安装对应的依赖包：

```bash
npm install babel-loader@8.2.1 @babel/core@7.12.3 @babel/plugin-proposal-class-properties@7.12.1 -D
```

### 7.2 配置 babel-loader

在 webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：  

```js
module.exports = {
    mode: 'development',
    // 所有第三方模块的匹配规则
    module: {
            {
                test:/\.js$/,
                // exclude 为排除项
                // 表示babel-loade无需处理node_modules下的js文件
                exclude:/node_modules/,
                use:{
                    loader:'babel-loader',
                    options:{
                        // 声明一个babel插件，用于转换class中的高级语法
                        plugins:['@babel/plugin-proposal-class-properties']
                    }
                }
            }
        ]
    }
}
```

# 管理输出





























# 打包发布

## 1.为什么要打包发布

项目开发完成之后，使用 webpack 对项目进行打包发布的主要原因有以下两点：

① 开发环境下，打包生成的文件存放于内存中，无法获取到最终打包生成的文件

② 开发环境下，打包生成的文件不会进行代码压缩和性能优化为了让项目能够在生产环境中高性能的运行，因此需要对项目进行打包发布。  

## 2. 配置 webpack 的打包发布

 在 package.json 文件的 scripts 节点下，新增 build 命令如下：

```json
{
    "scripts":{
        "dev":"webpack serve", // 开发环境中，运行dev命令
        "build":"webpack --mode production" // 项目发布时运行build命令
    }
}
```

`--model` 是一个参数项，用来指定 webpack 的运行模式。production 代表生产环境，会对打包生成的文件进行代码压缩和性能优化。
注意：通过 --model 指定的参数项，会**覆盖** webpack.config.js 中的 model 选项。 

### 3. 把 JavaScript 文件统一生成到 js 目录中

在 webpack.config.js 配置文件的 output 节点中，进行如下的配置：  

```js
const path = require('path');

// 1.导入HTML插件，得到一个构造函数
const HtmlPlugin = require('html-webpack-plugin');

// 2.创建插件的实例对象
const htmlPlugin = new HtmlPlugin({
    template: './src/index.html', //指定要复制的文件路径（原文件的存放路径）,
    filename: './index.html'  //生成的文件存放路径

})

module.exports = {
    mode: 'development',
    // 指定打包的入口文件
    entry: path.join(__dirname, './src/index.js'),
    output: {
        // 输出目录
        path: path.join(__dirname, './dist'),
        // 明确告诉webpack把生成的bundle.js文件存放到dist目录下的js文件夹下
        filename: 'js/bundle.js'
    },
}
```

## 4.把图片文件统一生成到 image 目录中

修改 webpack.config.js 中的 url-loader 配置项，新增 `outputPath` 选项即可指定图片文件的输出路径：  

```js
module.exports = {
    mode: 'development',
    // 所有第三方模块的匹配规则
    module: {
        rules: [
            {
                test:/\.jpg|png|gif$/,
                use:{
                    loader:'url-loader',
                    options:{
                        limit:2222,
                        //明确指出生成的文件存储到dist目录下的image文件夹中
                        outputPath:'image'
                    }
				}
            }
        ]
    }
}
```

# Source Map

## 1.生产环境中遇到的问题

前端项目在投入生产环境之前，都需要对 JavaScript 源代码进行压缩混淆，从而减小文件的体积，提高文件的加载效率。此时就不可避免的产生了另一个问题：对压缩混淆之后的代码除错（debug）是一件极其困难的事情  

![image-20210805125849106](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108051258183.png)

- 变量被替换成没有任何语义的名称
-  空行和注释被剔除  

## 2. 什么是 Source Map  

**Source Map 就是一个信息文件，里面储存着位置信息。也就是说，Source Map 文件中存储着代码压缩混淆前后的对应关系。**

有了它，出错的时候，除错工具将直接显示原始代码，而不是转换后的代码，能够极大的方便后期的调试  

## 3. webpack 开发环境下的 Source Map

在开发环境下，webpack 默认启用了 Source Map 功能。当程序运行出错时，可以直接在控制台提示错误行的位置，并定位到具体的源代码：  

### 3.1 默认 Source Map 的问题  

开发环境下**默认生成的 Source Map记录的是生成后的代码**的位置。会导致运行时报错的行数与源代码的行数不一致的问题。示意图如下：  

### 3.2 解决默认 Source Map 的问题

开发环境下，推荐在 webpack.config.js 中添加如下的配置，即可保证运行时报错的行数与源代码的行数保持一致：  

```js
module.exports={
    mode:'development',
    //eval-source-map 仅限在开发模式下使用，不建议在生产模式下使用
    //使用此配置生成的source map即可保证运行时报错的行数与源代码的行数保持一致：  
    devtool: 'eval-source-map'
}
```

## 4. webpack 生产环境下的 Source Map

在生产环境下，如果省略了 devtool 选项，则最终生成的文件中不包含 Source Map。这能够防止原始代码通过 Source Map 的形式暴露给别有所图之人。  

### 4.1只定位行数不暴露源码  

在生产环境下，如果只想定位报错的具体行数，且不想暴露源码。此时可以将 devtool 的值设置为`nosources-source-map`。实际效果如图所示：  

![image-20210805131426332](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108051314367.png)

### 4.2 定位行数且暴露源码

在生产环境下，如果想在定位报错行数的同时，展示具体报错的源码。此时可以将 devtool 的值设置为source-map。

![image-20210805131530831](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108051315878.png)

## 5.Source Map 的最佳实践  

① 开发环境下：

- 建议把 devtool 的值设置为 eval-source-map

- 好处：可以精准定位到具体的错误行

② 生产环境下：

- 建议关闭 Source Map 或将 devtool 的值设置为 nosources-source-map

- 好处：防止源码泄露，提高网站的安全性  
