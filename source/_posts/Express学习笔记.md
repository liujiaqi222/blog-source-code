---
title: Express学习笔记
date: 2022-01-31 17:55:37
tags: [Express, Node]
---

## 快速上手

a. 安装express，当前使用的是4.17版本。

在终端中输入以下命令：

```bash
yarn add express
```

b. 创建一个'app.js'文件，引入express，创建express实例，并监听端口3000。

```js
//app.js
const express = require('express');

const app = express();

app.listen(3000, () => {
  console.log('running at http://localhost:3000');
});
```

在终端中输入以下命令：

```bash
yarn add nodmon #用于实时更新node代码
nodemon app.js # 开始运行代码
```

c. 当客户端访问的http://localhost:3000的时候，给客户端的get请求返回内容，可以使用`res.send()`方法。

```js
const express = require('express');

const app = express();

app.get('/', (req, res) => {
  // 发送状态码
  res.send('hi express')
});


app.listen(3000, () => {
  console.log('running at http://localhost:3000');
});
```

或者使用`res.json({name:'jiaqi'})`，这里因为我安装了json插件，所以数据样式被美化了一点。

![image-20211116145100502](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161451529.png)

还可以给客户端发送返回文件，`res.download('img.png')`，因为我这里真的有一张图片，客户端访问该网址的时候，就可以下载文件。

![动画](https://i.loli.net/2021/11/16/AhgOHMDE3xVJWjk.gif)

d. 如果想要给设置状态码，可以使用`res.sendStatus(500)`。

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161448134.png" alt="image-20211116144817087" style="zoom:50%;" />

但如果设置状态码的同时，又想要发送内容可以使用`res.status(500).send('server error')`。

<img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161449693.png" alt="image-20211116144917649" style="zoom:50%;" />

或者也可以使用 `res.status(500).json(message:'error')`，就会发送json格式的数据。

![image-20211116144644514](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161446563.png)



## 模板引擎

### 使用ejs渲染html

如果想要在客户端渲染html，我们可以使用 view engine，在这里将使用ejs这个npm包。

```bash
yarn add ejs #安装ejs
```

并在app.js设置ejs作为view engine。

```js
const express = require('express');

const app = express();

app.set('view engine', 'ejs');

app.get('/', (req, res) => {
  
});


app.listen(3000, () => {
  console.log('running at http://localhost:3000');
});
```

接着我们在根目录下创建一个view文件夹，用于存放需要被渲染的html文件夹。

![image-20211116151018624](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161510667.png)

接着将html后缀改为ejs

![image-20211116151103652](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161511685.png)

然后为了可以在ejs的文件有代码高亮和语法补全的功能，可以装一个ejs的插件。

![image-20211116151218913](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161512947.png)



接着在代码中增加一句，`app.render(view文件夹下的html名称)`，就会在客户端看到了hello了。

```js
app.get('/', (req, res) => {
  res.render('index');
});
```

![image-20211116151648927](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161516954.png)

### 给html传递信息

通过render函数的第二个参数，可以给我们的html文件传递信息

```js
app.get('/', (req, res) => {
  res.render('index',{name:'jiaqi'});
});
```

在ejs中 `<%%>`就相当于vue中的插槽`{{}}`，表示在这里使用js，`=` 表示在html中输出。

![image-20211116152211682](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161522716.png)

因此上面的截图最终在浏览器显示的是：

![image-20211116152704142](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161527164.png)

接下来，如果想要使用服务器给模板引擎传递的数据也是一样的。

![image-20211116153935849](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161539880.png)



![image-20211116153928351](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161539375.png)

但如果说我们给模板传了太多变量，到最后都不记得传没有传。比如这里我们并没有给模板引擎传name333这个变量，最后浏览器端就报错了。

![image-20211116154429299](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161544352.png)

为了这个问题，在ejs文件中我们在变量前面加一个locals。没有name333，那浏览器就不会显示，不会报错！

`![image-20211116154715656](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161547685.png)

![image-20211116154727153](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111161547173.png)

## 路由

如果说有百个路径，我们不能全部都写在app.js中，很难以维护，express当然也想到了这点。我们可以使用路由，路由就是让我们可以创建另外一个实例，它有自己的逻辑，然后最后我们可以把这些代码嵌入到我们的主代码。

> 官网解释： A `router` object is an isolated instance of middleware and routes. You can think of it as a “mini-application,” capable only of performing middleware and routing functions. Every Express application has a built-in app router.

比如在这里我写了很多与users有关的路径，我们可以将这些代码放入routes文件夹下。

```js
const express = require('express');

const app = express();

app.set('view engine', 'ejs');

app.get('/', (req, res) => {
  res.render('index',{name:'jiaqi'});
});

//users
app.get('/users', (req, res) => {
  res.send('user list');
})
app.get('/users/new', (req, res) => {
  res.send('user new form');
})

//===users


app.listen(3000, () => {
  console.log('running at http://localhost:3000');
});
```

在根目录下创建一个routes的文件夹，并创建users.js文件。

![image-20211116224541167](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111162245242.png)

接着在user.js中，引入express，并使用它的`Router()`函数，然后将`app.get()`替换成`router.get()`。

```js
//routes/users.js
const express = require('express');
const router = express.Router();


router.get('/users', (req, res) => {
  res.send('user list');
})
router.get('/users/new', (req, res) => {
  res.send('user new form');
})
```

接着，我们导出这这个router对象，并将这些路径前面的`/users`去掉（因为等下在app.js中引入这些users路由时会做处理）

```js
//routes/users.js
const express = require('express');
const router = express.Router();


router.get('/', (req, res) => {
  res.send('user list');
})
router.get('/new', (req, res) => {
  res.send('user new form');
});


module.exports = router;
```

现在在app.js中引入刚才导出的users路由。

```js
//app.js

const express = require('express');

const app = express();

app.set('view engine', 'ejs');

app.get('/', (req, res) => {
  res.render('index',{name:'jiaqi'});
});

//使用user路由
//表示以/users作为路径起点，然后再连接./routes/user.js文件中的具体的路径
app.use('/users', require('./routes/user'));


app.listen(3000, () => {
  console.log('running at http://localhost:3000');
});

```

这时我们再打开对应的网址，还是会正常显示。

![image-20211116225554512](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111162255556.png)

### 动态路由

假设我们现在有很多用户，如'/users/1','users/2','users/3'等。但我们不可能为每一个用户的请求地址都创建一个路由！



这时候，我们就可以使用动态路由，`router.get('/:id')`，通过使用`:`，告诉express匹配任意的参数后缀。

```js
//routes/users.js

const express = require('express');
const router = express.Router();


router.get('/', (req, res) => {
  res.send('user list');
})
router.get('/new', (req, res) => {
  res.send('user new form');
});

router.get('/:id', (req, res) => {
  // 这里的路径中的id是自定义的
  // 如果定义为/:userId ，想要获取对应的userId，就用req.params.userId
  res.send(`获取用户${req.params.id}的数据`);
})



module.exports = router;
```



![image-20211116230707431](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111162307466.png)





> NOTE：注意一般要将动态路由放在最底部，因为express对于请求路径是从上往下匹配的。



假设在这里，我将`/new`放在动态路由的下面

```js
const express = require('express');
const router = express.Router();


router.get('/', (req, res) => {
  res.send('user list');
})


router.get('/:id', (req, res) => {
  // 这里的路径中的id是自定义的
  // 如果定义为/:userId ，想要获取对应的userId，就用req.params.userId
  res.send(`获取用户${req.params.id}的数据`);
})

router.get('/new', (req, res) => {
  res.send('user new form');
});

module.exports = router;
```

![image-20211116231139401](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111162311436.png)



我们可以看到`/:id`也匹配了'/new'的路径，因为id不过是个名称而已，它可以匹配任何字符串，包括new这个字符串。

### app.route()

如果一个路径，既需要用到get，又需要用到put，delete，这时候我们可以用app.route()链式调用。在路由中，就是将`app.route()`换成`router.route()`

```js
const express = require('express');
const router = express.Router();


router.get('/', (req, res) => {
  res.send('user list');
})



router.route('/:id')
  .get((req, res) => {
    res.send(`get user with id ${req.params.id}`);
  })
  .put((req, res) => {
    res.send(`update user with id ${req.params.id}`);
  })
  .delete((req, res) => {
    res.send(`delete user with id ${req.params.id}`);
  })


module.exports = router;
```



这样我们不就少写了几次路径！

### app.param(name,callback)

和上面一样在路由中写作`router.param()`。它的意思是如果各个路由的路径如果匹配到了app.param的第一个参数（也就是name），则执行后面的回调函数（callback）。

回调函数中的参数按顺序分别是请求对象，响应对象，next中间件，参数的值以及参数的名字，也就是`app.param(name,callback(req,res,next,id,name))`

```js
const express = require('express');
const router = express.Router();


router.get('/', (req, res) => {
  res.send('user list');
})



router.route('/:id')
  .get((req, res) => {
    res.send(`get user with id ${req.params.id}`);
  })
  .put((req, res) => {
    res.send(`update user with id ${req.params.id}`);
  })
  .delete((req, res) => {
    res.send(`delete user with id ${req.params.id}`);
  })

// 如果匹配到了路径中有id的就执行这个函数
// 而上面的那些get，put，delete都有id这个参数
router.param('id', (req, res, next, id, name) => {
  console.log(id, name); //控制台打印了2，id
})


module.exports = router;
```

但是浏览器一直在转圈，表示在加载中，且页面也没有被刷新！

![image-20211117000459777](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202111170004832.png)

这是因为我们没有调用next函数，执行下一步。因为`router.param()`本质上就是一种中间件，中间件就是**请求发送到服务器后，但服务器还没有将响应返回客户端。**

经历了这个中间件，然后才会调用实际对应的路由（如果在这个中间件中调用了next()）。

**如果此时在该中间件的req或者res上挂载属性，之后的路由可以访问到**

```js
router.get('/:id', (req, res) => {
   //此处可以获取到刚才在req上添加的user属性.
  console.log(req.user)
  res.send(`user's id is ${req.params.id}`);
})

const users = [{ name: 'jiaqi' }, { name: 'sally' }];

router.param('id', (req, res, next, id) => {
  //接下来其他路由可以访问到此时添加的user属性
  req.user = users[id]
  next();
})
```

## 中间件

如果有一个函数，每个路径都需要使用，可以在最顶部用`app.use()`定义一个中间件。中间件是从上到下运行的。

永远不要忘记了中间件函数需要调用`next()`。

```js
const express = require('express');

const app = express();

app.set('view engine', 'ejs');
app.use(logger); //中间件


app.get('/', (req, res) => {
  res.render('index',{text:'world'});
})

const userRouter = require('./routes/users')
app.use('/users', userRouter);

function logger(req, res, next) {
  console.log(req.originalUrl);
  next()
}


app.listen(3000, () => {
  console.log('running at http://localhost:3000');
});
```

除了用`app.use()`使用中间件，还可以直接在某个路由前使用中间件。

```js
app.get('/',logger, (req, res) => {
  res.render('index',{text:'world'});
})
```

如果愿意的话，也可以在同一个路径下使用多个中间件函数。

```js
app.get('/',logger, logger, (req, res) => {
  res.render('index',{text:'world'});
})
```



### 渲染静态文件

express 自带一些实用的中间件，比如渲染静态文件。

比如在根目录下的`public`文件夹下，我们放了一些静态的HTML文件，HTML的内容我们不会去进行修改。

![image-20220131132902752](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/image-20220131132902752.png)

```js
app.use(express.static('public'));
```



此时，即使我们什么路由也不写，它就可以直接渲染 public 文件夹下的 `index.html`。

```js
const express = require('express');

const app = express();

app.set('view engine', 'ejs');

app.use(express.static('public'));

app.listen(3000, () => {
  console.log('running at http://localhost:3000');
});
```

![image-20220131133027618](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/image-20220131133027618.png)

如果在 public 文件夹下创建一个新的文件夹 test，然后在 test 文件夹下创建一个 index.html，接着如果我们访问：`http://localhost:3000/test/`也可以打开该html文件。

总之，url会与文件实际存放路径一一对应。



### 解析表单

如果要获取到别人提交的表单信息，我们可以使用这个中间件：

```js
app.use(express.urlencoded({extended:true}));
```

之后，我们就可以通过 `req.body`获取到用户提交的信息。



假设表单为：

```js
<form action="/users" method="post">
    <input type="text" name="userName" id="name" value="<%=locals.name %>">
  <button type="submit">提交</button>
</form>
```

获取到表单信息：

```js
router.get('/new', (req, res) => {
  res.render('users/new',{name:'jiaqi'}) //第二个参数给模板引擎传递参数
})

router.post('/', (req, res) => {
  console.log(req.body); //获取到用户的提交的表单信息
  res.send('create user');
})
```

### 解析JSON

这个和上面的解析表单作用一样，区别在于上面是解析表单，而这个是解析用户的JSON格式的请求。

```js
app.use(express.json())
```







