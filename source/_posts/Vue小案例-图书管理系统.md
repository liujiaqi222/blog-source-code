---
title: Vue小案例--图书管理系统
date: 2021-07-30 17:55:39
tags: [VUE,案例]
---

# 图书管理

## 1. 图书列表

⚫ 实现静态列表效果
⚫ 基于数据实现模板效果
⚫ 处里每行的操作按钮  

![image-20210730181114909](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210730181123.png)

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style type="text/css">
    .grid {
      margin: auto;
      width: 500px;
      text-align: center;
    }

    .grid table {
      width: 100%;
      border-collapse: collapse;
    }

    .grid th,
    td {
      padding: 10;
      border: 1px dashed orange;
      height: 35px;
      line-height: 35px;
    }

    .grid th {
      background-color: orange;
    }
  </style>
</head>

<body>
  <div id="app">
    <div class="grid">
      <table>
        <thead>
          <tr>
            <th>编号</th>
            <th>名称</th>
            <th>时间</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td></td>
            <td></td>
            <td></td>
            <td>
              <a href="">修改</a>
              <span>|</span>
              <a href="">删除</a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
  <script type="text/javascript" src="0./vue.js"></script>
  <script type="text/javascript">

    var vm = new Vue({
      el: '#app',
      data: {
        books: [{
          id: 1,
          name: '三国演义',
          date: ''
        }, {
          id: 2,
          name: '水浒传',
          date: ''
        }, {
          id: 3,
          name: '红楼梦',
          date: ''
        }, {
          id: 4,
          name: '西游记',
          date: ''
        }]
      }
    });
  </script>
</body>

</html>
```

## 2. 添加图书

⚫ 实现表单的静态效果
⚫ 添加图书表单域数据绑定
⚫ 添加按钮事件绑定
⚫ 实现添加业务逻辑  

![image-20210730182422489](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210730182423.png)

```html
<div id="app">
  <div class="grid">
    <div>
      <h1>图书管理</h1>
      <div class="book">
        <div>
          <label for="id">
            编号：
          </label>
          <input type="text" id="id" v-model='id'>
          <label for="name">
            名称：
          </label>
          <input type="text" id="name" v-model='name'>
          <button @click='handle'>提交</button>
        </div>
      </div>
    </div>
    <table>
      <thead>
        <tr>
          <th>编号</th>
          <th>名称</th>
          <th>时间</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for='book in books' :key='book.id'>
          <td>{{book.id}}</td>
          <td>{{book.name}}</td>
          <td>{{book.date}}</td>
          <td>
            <!-- 禁用页面刷新 -->
            <a href="" @click.prevent>修改</a>
            <span>|</span>
            <a href="" @click.prevent>删除</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
<script type="text/javascript" src="/0.vue.js"></script>
<script type="text/javascript">

  var vm = new Vue({
    el: '#app',
    data: {
      id:'',
      name:'',
      books: [{
        id: 1,
        name: '三国演义',
        date: ''
      }, {
        id: 2,
        name: '水浒传',
        date: ''
      }, {
        id: 3,
        name: '红楼梦',
        date: ''
      }, {
        id: 4,
        name: '西游记',
        date: ''
      }]
    },
    methods: {
      handle(){
        // 添加图书
        let book={};
        book.id=this.id;
        book.name=this.name;
        this.books.push(book);
        // 清空input输入域
        this.id='';
        this.name='';
      }
    },
  });
</script>
```

## 3. 修改图书

⚫ 修改信息填充到表单
⚫ 修改后重新提交表单
⚫ 重用添加和修改的方法  

```html
<div id="app">
  <div class="grid">
    <div>
      <h1>图书管理</h1>
      <div class="book">
        <div>
          <label for="id">
            编号：
          </label>
          <input type="text" id="id" v-model='id' :disabled='flag'>
          <label for="name">
            名称：
          </label>
          <input type="text" id="name" v-model='name'>
          <button @click='handle'>提交</button>
        </div>
      </div>
    </div>
    <table>
      <thead>
        <tr>
          <th>编号</th>
          <th>名称</th>
          <th>时间</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for='book in books' :key='book.id'>
          <td>{{book.id}}</td>
          <td>{{book.name}}</td>
          <td>{{book.date}}</td>
          <td>
            <!-- 禁用页面刷新 -->
            <a href="" @click.prevent='edit(book.id)'>修改</a>
            <span>|</span>
            <a href="" @click.prevent>删除</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
<script type="text/javascript" src="/0.vue.js"></script>
<script type="text/javascript">

  var vm = new Vue({
    el: '#app',
    data: {
      id: '',
      name: '',
      books: [{
        id: 1,
        name: '三国演义',
        date: ''
      }, {
        id: 2,
        name: '水浒传',
        date: ''
      }, {
        id: 3,
        name: '红楼梦',
        date: ''
      }, {
        id: 4,
        name: '西游记',
        date: ''
      }],
      flag: false
    },
    methods: {
      handle() {
        if (this.flag) {
          this.books.forEach(item => {
            // 如果表单中的id与数据中的id相同
            if (item.id == this.id) {
              item.name = this.name;
              // 允许输入
              this.flag = false;
              return;
            }
          })
        } else {
          // 添加图书
          let book = {};
          book.id = this.id;
          book.name = this.name;
          this.books.push(book);
        }
        // 清空input输入域
        this.id = '';
        this.name = '';
      },
      edit(id) {
        this.books.forEach(item => {
          if (item.id == id) {
            this.id = item.id;
            this.name = item.name;
            this.flag = true;
            return;
          }
        });
      }
    },
  });
</script>
```

## 4.删除图书

⚫ 删除按钮绑定事件处理方法
⚫ 实现删除业务逻辑  

```html
<div id="app">
  <div class="grid">
    <div>
      <h1>图书管理</h1>
      <div class="book">
        <div>
          <label for="id">
            编号：
          </label>
          <input type="text" id="id" v-model='id' :disabled='flag'>
          <label for="name">
            名称：
          </label>
          <input type="text" id="name" v-model='name'>
          <button @click='handle'>提交</button>
        </div>
      </div>
    </div>
    <table>
      <thead>
        <tr>
          <th>编号</th>
          <th>名称</th>
          <th>时间</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for='book in books' :key='book.id'>
          <td>{{book.id}}</td>
          <td>{{book.name}}</td>
          <td>{{book.date}}</td>
          <td>
            <!-- 禁用页面刷新 -->
            <a href="" @click.prevent='edit(book.id)'>修改</a>
            <span>|</span>
            <a href="" @click.prevent='del(book.id)'>删除</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
<script type="text/javascript" src="/0.vue.js"></script>
<script type="text/javascript">

  var vm = new Vue({
    el: '#app',
    data: {
      id: '',
      name: '',
      books: [{
        id: 1,
        name: '三国演义',
        date: ''
      }, {
        id: 2,
        name: '水浒传',
        date: ''
      }, {
        id: 3,
        name: '红楼梦',
        date: ''
      }, {
        id: 4,
        name: '西游记',
        date: ''
      }],
      flag: false
    },
    methods: {
      handle() {
        if (this.flag) {
          this.books.some(item => {
            // 如果表单中的id与数据中的id相同
            if (item.id == this.id) {
              item.name = this.name;
              // 允许输入
              this.flag = false;
              console.log(item.id);
              return true;
            }
          })
        } else {
          // 添加图书
          let book = {};
          book.id = this.id;
          book.name = this.name;
          this.books.push(book);
        }
        // 清空input输入域
        this.id = '';
        this.name = '';
      },
      edit(id) {
        this.books.some(item => {
          if (item.id == id) {
            this.id = item.id;
            this.name = item.name;
            this.flag = true;
            return true;
          }
        });
      },
      del(id){
        this.books.some((item,index) =>{
          if(item.id==id){
            this.books.splice(index,1);
            return true;
          }
        })
      }
    },
  });
</script>
```

经过测试，使用forEach方法，如果已经匹配到了想要的数组元素，用return 是无法退出遍历的。因此，使用some方法更为恰当，当匹配到所需元素时，return true即可退出遍历。

## 5.常用特性应用场景

⚫ 过滤器（格式化日期）
⚫ 自定义指令（获取表单焦点）
⚫ 计算属性（统计图书数量）
⚫ 侦听器（验证图书存在性）
⚫ 生命周期（图书数据处理）  

[使用正则格式化时间](https://jiaqicoder.com/2021/07/31/%E4%BD%BF%E7%94%A8%E6%AD%A3%E5%88%99%E6%A0%BC%E5%BC%8F%E5%8C%96%E6%97%B6%E9%97%B4/)

最终代码：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style type="text/css">
    .grid {
      margin: auto;
      width: 530px;
      text-align: center;
    }

    .grid table {
      border-top: 1px solid #C2D89A;
      width: 100%;
      border-collapse: collapse;
    }

    .grid th,
    td {
      padding: 10;
      border: 1px dashed #F3DCAB;
      height: 35px;
      line-height: 35px;
    }

    .grid th {
      background-color: #F3DCAB;
    }

    .grid .book {
      padding-bottom: 10px;
      padding-top: 5px;
      background-color: #F3DCAB;
    }

    .grid .total {
      box-sizing: border-box;
      padding-left: 80px;
      text-align: left;
      height: 30px;
      line-height: 30px;
      background-color: #F3DCAB;
      border-top: 1px solid #C2D89A;
    }

    .grid .total span:nth-child(3) {
      margin-left: 30px;
      color: rgba(202, 14, 14, 0.644);
    }
  </style>
</head>

<body>
  <div id="app">
    <div class="grid">
      <div>
        <h1>图书管理</h1>
        <div class="book">
          <div>
            <label for="id">
              编号：
            </label>
            <input type="text" id="id" v-model='id' :disabled='flag' v-focus>
            <label for="name">
              名称：
            </label>
            <input type="text" id="name" v-model='name'>
            <button @click='handle' :disabled='submitFlag'>提交</button>
          </div>
        </div>
      </div>
      <div class='total'>
        <span>图书总数: </span>
        <span>{{totalofbook}}</span>
        <span v-show='err'>{{errMessage}}</span>
      </div>
      <table>
        <thead>
          <tr>
            <th>编号</th>
            <th>名称</th>
            <th>时间</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for='book in books' :key='book.id'>
            <td>{{book.id}}</td>
            <td>{{book.name}}</td>
            <td>{{book.date | format('yyyy-MM-dd')}}</td>
            <td>
              <!-- 禁用页面刷新 -->
              <a href="" @click.prevent='edit(book.id)'>修改</a>
              <span>|</span>
              <a href="" @click.prevent='del(book.id)'>删除</a>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
  <script type="text/javascript" src="/0.vue.js"></script>
  <script type="text/javascript">
    Vue.filter('format', function (date, format) {
      if (typeof date === "string") {
        var mts = date.match(/(\/Date\((\d+)\)\/)/);
        if (mts && mts.length >= 3) {
          date = parseInt(mts[2]);
        }
      }
      date = new Date(date);
      if (!date || date.toUTCString() == "Invalid Date") {
        return "";
      }
      var map = {
        "M": date.getMonth() + 1, //月份 
        "d": date.getDate(), //日 
        "h": date.getHours(), //小时 
        "m": date.getMinutes(), //分 
        "s": date.getSeconds(), //秒 
        "q": Math.floor((date.getMonth() + 3) / 3), //季度 
        "S": date.getMilliseconds() //毫秒 
      };
      format = format.replace(/([yMdhmsqS])+/g, function (all, t) {
        var v = map[t];
        if (v !== undefined) {
          if (all.length > 1) {
            v = '0' + v;
            v = v.substr(v.length - 2);
          }
          return v;
        } else if (t === 'y') {
          return (date.getFullYear() + '').substr(4 - all.length);
        }
        return all;
      });
      return format;
    })

    Vue.directive('focus', {
      inserted: function (el) {
        el.focus();
      }
    })
    var vm = new Vue({
      el: '#app',
      data: {
        id: '',
        name: '',
        books: [],
        flag: false,
        submitFlag: false,
        err: false,
        errMessage: ''
      },
      methods: {
        handle() {
          if (this.flag) {
            this.books.some(item => {
              // 如果表单中的id与数据中的id相同
              if (item.id == this.id) {
                item.name = this.name;
                // 允许输入
                this.flag = false;
                console.log(item.id);
                return true;
              }
            })
          } else {
            // 添加图书
            let book = {};
            book.id = this.id;
            book.name = this.name;
            this.books.push(book);
          }
          // 清空input输入域
          this.id = '';
          this.name = '';
        },
        edit(id) {
          this.books.some(item => {
            if (item.id == id) {
              this.id = item.id;
              this.name = item.name;
              this.flag = true;
              return true;
            }
          });
        },
        del(id) {
          this.books.some((item, index) => {
            if (item.id == id) {
              this.books.splice(index, 1);
              return true;
            }
          })
        }
      },
      computed: {
        totalofbook: function () {
          return this.books.length;
        }
      },
      watch: {
        name: function (value) {
          // 验证图书名称是否存在
          this.books.some(item => {
            if (value == item.name) {
              this.submitFlag = true;
              this.err = true;
              this.errMessage = '图书名称已存在'
              return true;
            }
          });
        },
      },
      // 该生命周期钩子函数被触发的时候
      // 一般此时用于获取后台数据，然后把数据填充在模板
      mounted() {
        let data=[{
          id: 1,
          name: '三国演义',
          date: Date.now()
        }, {
          id: 2,
          name: '水浒传',
          date: 962770087686
        }, {
          id: 3,
          name: '红楼梦',
          date: 999778657686
        }, {
          id: 4,
          name: '西游记',
          date: 162770298686
        }];
        this.books=data;
      },
    });
  </script>
</body>

</html>
```

![image-20210731144257953](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/20210731144306.png)
