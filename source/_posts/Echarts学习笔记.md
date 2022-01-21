---
title: Echarts学习笔记
date: 2021-10-09 23:39:36
tags: [echarts]
---

# Echarts 快速上手

1. 引入 echarts.js 文件
2. 准备一个呈现图表的盒子
3. 初始化echarts实例对象
4. 准备配置项
5. 将配置项设置给echarts实例对象

```html
<div style='width: 600px; height: 400px;'></div>
<script src="./lib/echarts.js"></script>
<script>
  // 基于准备好的dom，初始化echarts实例
  let myChart = echarts.init(document.querySelector('div'));
  // 指定图表的配置项和数据
  let option = {
    title:{
      text:'ECharts 入门实例',
      link:'https://jiaqicoder.com',
      textStyle:{
        color:'pink',
        fontStyle:'normal'
      }
    },
    xAxis:{
      type:'category', // 类目轴
      data:['衬衫','羊毛衫','雪纺衫','裤子'] 
    },
    yAxis:{
      type:'value', // 数值轴
    },
    series:[{
      name:'销量',
      type:'bar', //line pie
      data:[5,10,15,20]
    }]
  }
    // 使用刚指定的配置项和数据显示图表。
    myChart.setOption(option);
</script>
```

相关配置：

- xAxis：直接坐标系中的x轴
- yAxis：直角坐标系中的y轴
- series：系列列表。每个系列通过type决定自己的图表类型
- [title]([Documentation - Apache ECharts](https://echarts.apache.org/zh/option.html#title))：标题组件，包含主标题和副标题。

![image-20210924105806030](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241614890.png)

# 通用配置

通用配置指的就是任何图表都能使用的配置。

## 标题 title

- 文字样式

  ```js
  title:{
    text:'成绩展示',
    textStyle:{
      color:'red',
      fontSize:'28px',
      fontWeight:800,
    },
  },
  ```

- 标题边框

  ```js
  borderWidth:5,
  borderColor:'blue',
  borderRadius:5
  ```

- 标题位置

  ```js
  title:{
    text:'成绩展示',
    textStyle:{
      color:'red',
      fontSize:'28px'
    },
  borderWidth:5,
  borderColor:'blue',
  borderRadius:5,
  left:100,
  top:20,
  },
  ```

  `top` 的值可以是像 `20` 这样的具体像素值，可以是像 `'20%'` 这样相对于容器高宽的百分比，也可以是 `'top'`, `'middle'`, `'bottom'`。

  如果 `top` 的值为`'top'`, `'middle'`, `'bottom'`，组件会根据相应的位置自动对齐。

## 提示 tooltip

tooltip：提示框组件，用于配置鼠标滑过或者点击图表时的显示框。

- 触发类型：trigger （可选）

  - `'item'`

    数据项图形触发，主要在散点图，饼图等无类目轴的图表中使用。

  - `'axis'`

    坐标轴触发，主要在柱状图，折线图等会使用类目轴的图表中使用。

  - `'none'`

    什么都不触发。

- 触发时机：triggerOn （可选）

  - `'mousemove'`

    鼠标移动时触发。

  - `'click'`

    鼠标点击时触发。

  - `'mousemove|click'`

    同时鼠标移动和点击时触发。

- 格式化：formatter

  提示框浮层内容格式器，支持字符串模板和回调函数两种形式。

  字符串模板：

  模板变量有 `{a}`, `{b}`，`{c}`，`{d}`，`{e}`，分别表示系列名，数据名，数据值等。 在 [trigger](https://echarts.apache.org/zh/option.html#tooltip.trigger) 为 `'axis'` 的时候，会有多个系列的数据，此时可以通过 `{a0}`, `{a1}`, `{a2}` 这种后面加索引的方式表示系列的索引。 不同图表类型下的 `{a}`，`{b}`，`{c}`，`{d}` 含义不一样。 其中变量`{a}`, `{b}`, `{c}`, `{d}`在不同图表类型下代表数据含义为：

  - 折线（区域）图、柱状（条形）图、K线图 : `{a}`（系列名称），`{b}`（类目值），`{c}`（数值）, `{d}`（无）
  - 散点图（气泡）图 : `{a}`（系列名称），`{b}`（数据名称），`{c}`（数值数组）, `{d}`（无）
  - 地图 : `{a}`（系列名称），`{b}`（区域名称），`{c}`（合并数值）, `{d}`（无）
  - 饼图、仪表盘、漏斗图: `{a}`（系列名称），`{b}`（数据项名称），`{c}`（数值）, `{d}`（百分比）

  ```js
  tooltip:{
    trigger:'axis',// 'item'
    triggerOn:'click',
    formatter:'{b} 的成绩是 {c}', //{b}（类目值），{c}（数值）
  }
  ```

  -----

  回调函数格式：

  支持返回 HTML 字符串或者创建的 DOM 实例。

  第一个参数 `arg` 是 formatter 需要的数据集

  ```js
  tooltip:{
    trigger:'axis',// 'item'
    triggerOn:'click',
    formatter:function(arg){
      console.log(arg); // 可以打印看看 有什么值
      return `${arg[0].name}的分数是 ${arg[0].data}`; // 返回值将会呈现在页面中
    }
  },
  ```

## 工具按钮 toolbox

内置有导出图片，数据视图，动态类型切换,数据区域缩放，重置五个工具。

```js
toolbox:{
  feature:{
    saveAsImage:{}, // 保存为图片
    dataView:{}, //数据视图
    restore:{}, // 重置功能
    dataZoom:{}, // 区域缩放
    magicType:{
      type:['line','bar']    // 切换图表类型
    }
  }
},
```

![image-20210924143614282](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241615720.png)



## 图例 legend

legend：图例，用于筛选系列，需要和series配合使用。

- legend中的 data是一个数组
- legend中的data的值需要和series数组中某组数据的name值一致

```js
series: [
  {
    name: '语文成绩',
    type: 'bar',
    data: [98, 88, 78, 80]
  },{
    name:'数学成绩',
    data:[42,55,99,88],
    type:'bar'
  }
],
legend:{
  data:['语文成绩','数学成绩'], // 如果series中的name有值时，legend中的data可以不写
}
```

![image-20210924151308211](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241615448.png)

![image-20210924151320748](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241615889.png)



# ECharts的常用图表

- 柱状图 bar
- 折线图
- 散点图
- 饼图
- 地图
- 雷达图
- 仪表盘图

## 柱状图

### 实现步骤

- ECharts 最基本的代码结构：

  引入js文件，设置DOM容器，初始化对象，设置options

- x轴数据：

  数据1：['张三','李四','王五','赵六']

- y轴数据：

  数据2：[88,92,79,79]

- 图表类型：

  在series下设置 type:bar

```html
<body>
  <div style="width: 600px;height: 400px;"></div>
  <script src="./lib/echarts.js"></script>
  <script>

    let myCharts = echarts.init(document.querySelector('div'));
    myCharts.setOption({
      title: {
        text:'成绩'
      },
      xAxis: {
        type: 'category',
        data: ['张三','李四','王五','赵六']
      },
      yAxis: {
        type: 'value'
      },
      series: [{
        name: '成绩',
        data: [5, 10, 15, 20],
        type: 'bar'
      }],
    });
  </script>
</body>
```

### 常见效果

-  标记：

  - 最大值 最小值（用markPoint来标记点）

    ```js
    series: [{
      name: '成绩',
      data: [5, 10, 15, 20],
      type: 'bar',
      markPoint:{
        data:[{
          type:'max',
          name:'最大值'
        },{
          type:'min',
          name:'最小值'
        }]
      }
    }]
    ```

    ![image-20210924114001186](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241615217.png)

  

  -  平均值(markLine)

    ```js
    series: [{
      name: '成绩',
      data: [5, 10, 15, 20],
      type: 'bar',
      markPoint:{
        data:[{
          type:'max',
          name:'最大值'
        },{
          type:'min',
          name:'最小值'
        }]
      },
      markLine:{
        data:[
          {
            type:'average',name:'平均值'
          }
        ]
      }
    }],
    ```

    ![image-20210924114211478](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241615232.png)

- 显示：

  - 数值显示(label)

    ```js
    series: [{
      name: '成绩',
      data: [5, 10, 15, 20],
      type: 'bar',
      markPoint:{
        data:[{
          type:'max',
          name:'最大值'
        },{
          type:'min',
          name:'最小值'
        }]
      },
      markLine:{
        data:[
          {
            type:'average',name:'平均值'
          }
        ]
      },
      label:{
        show:true,
      }
    }],
    ```

    

    ![image-20210924114641529](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241615544.png)

  - 柱宽度(barWidth)

    ```js
    series: [{
      name: '成绩',
      data: [5, 10, 15, 20],
      type: 'bar',
      markPoint:{
        data:[{
          type:'max',
          name:'最大值'
        },{
          type:'min',
          name:'最小值'
        }]
      },
      markLine:{
        data:[
          {
            type:'average',name:'平均值'
          }
        ]
      },
      label:{
        show:true,
        position:'inside'
      },
      barWidth:'50%' // 调整为默认宽度的50%
    }],
    ```

    

  - 横向柱状图

    将柱状图变为横向的：

    ```js
    xAxis: {
      type: 'value'
    },
    yAxis: {
      data: ['张三', '李四', '王五', '赵六'],
      type: 'category',
    }
    ```

    ![image-20210924115428763](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241615320.png)

## 折线图

### 实现步骤

- ECharts 最基本的代码结构：

  引入js文件，设置DOM容器，初始化对象，设置options

- x轴数据：

  数据1：['1月','2月','3月','4月','5月','6月']

- y轴数据：

  数据2：[88,92,79,79,39,99]

- 图表类型：

  在series下设置 type: line

```js
let myChart = echarts.init(document.querySelector('div'));
myChart.setOption({
  title:{
    text:'康师傅方便面销量',
    textStyle:{
      color:'blue'
    }
  },
  xAxis:{
    data:['1月','2月','3月','4月','5月','6月'],
    type:'category'
  },
  yAxis:{
    type:'value',
  },
  series:{
    name:'康师傅的销量',
    type:'line',
    data:[35,29,38,88,69,89],
  },
  tooltip:{
    trigger:'item',
    formatter:'{b}的销量为{c}'
  }
})
```

### 常见的效果

- 标记

  - 最大值 最小值 markPoint

    ```js
    series:{
      name:'康师傅的销量',
      type:'line',
      data:[35,29,38,88,69,89],
      markPoint:{
        data:[
          {type:'max'},
          {type:'min'}
        ]
      },
    },
    ```

  - 平均值 markLine

    ```js
    series:{
      name:'康师傅的销量',
      type:'line',
      data:[35,29,38,88,69,89],
      markPoint:{
        data:[
          {type:'max'},
          {type:'min'}
        ]
      },
      markLine:{
        data:[
          {type:'average'}
        ]
      }
    }
    ```

    ![image-20210924153332983](C:\Users\singhand\AppData\Roaming\Typora\typora-user-images\image-20210924153332983.png)

  - 标注区间 markArea

    ```js
    series:{
      name:'康师傅的销量',
      type:'line',
      data:[35,29,38,88,69,89],
      markPoint:{
        data:[
          {type:'max'},
          {type:'min'}
        ]
      },
      markLine:{
        data:[
          {type:'average'}
        ]
      },
      markArea:{
        data:[
          [
            {xAxis:'1月'}, // 开始月份
            {xAxis:'2月'}, // 结束月份
          ],
          [
            {xAxis:'3月'}, // 开始月份
            {xAxis:'5月'}, // 结束月份
          ],
        ]
      }
    },
    ```
  
    ![image-20210924160114696](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241615129.png)
  
  

- 线条控制：

  - 平滑 smooth

    ```js
    series:{
      name:'康师傅的销量',
      type:'line',
      data:[35,29,38,88,69,89],
      smooth:true
    },
    ```

    

    ![image-20210924161756384](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241617419.png)

  - 线条样式 lineStyle

    ```js
    series:{
      name:'康师傅的销量',
      type:'line',
      data:[35,29,38,88,69,89],
      smooth:true,
      lineStyle:{
        color:'green',
        type:'dotted', //solid dashed
      }
    },
    ```

    

  - 填充风格 areaStyle

    注意和markArea的区别。

    ```js
    series:{
      name:'康师傅的销量',
      type:'line',
      data:[35,29,38,88,69,89],
      smooth:true,
      lineStyle:{
        color:'green',
        type:'dotted', //solid dashed
      },
      areaStyle:{
        color:'pink'
      }
    },
    ```

    ![image-20210924162815962](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241628999.png)

    全部代码：[A Pen by liujiaqi222 (codepen.io)](https://codepen.io/liujiaqi222/pen/gORdQeo)
  
  - 紧挨边缘 boundaryGap
  
    默认和边缘是存在距离的，注意此时bundaryGap的是放在 xAxis中的。
  
    ![image-20210924164058998](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241640031.png)
  
    ```js
    xAxis: {
      data: ['1月', '2月', '3月', '4月', '5月', '6月'],
      type: 'category',
      boundaryGap:false, // 设置为无间隙
    },
    ```
  
    ![image-20210924164443113](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241644145.png)
  
    
  
  - 脱离0值比例缩放 scale
  
    如果数值相差无几，且数值较大，此时发现y轴的坐标依旧从0开始。
  
    ![image-20210924164917454](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241649496.png)
  
    此时，就可以将scale属性的值设置为true，允许坐标轴缩放。
  
    ```js
    yAxis: {
      type: 'value',
      scale:true
    },
    ```
  
    ![image-20210924165202333](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241652365.png)
  
  - 堆叠图 （stack的属性值要设置为相同的）
  
    如果两条不同的线，他们的数值波动较大就会出现如下的重叠情况，此时容易让人眼花缭乱。
  
    ![image-20210924170852203](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241708239.png)
  
    因此可以将两条线的stack属性值设置为同一个值（任意设置），此时第一条仍然为第一条线，第二条线则为原来两条线的总和（叠加）。
  
    ```js
    series: [{
      name: '康师傅的销量',
      type: 'line',
      data: [3091, 900, 300, 2006, 1510, 1813],
      stack: 'all',
      // stack属性值相同的serie，后面的serie的值将会叠加在前一个serie的值
      // 第二条线
    }, {
    
      name: '',
      type: 'line',
      data: [2000, 2328, 3282, 230, 1889, 1123],
      stack: 'all',
    }],
    ```
  
    ![image-20210924171151354](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109241711386.png)
  
    
  

## 散点图

散点图可以帮助我们推断变量间的相关性。

### 实现步骤

- 引入js文件，创建DOM容器，初始化对象，设置option

- x轴数据和y轴的数据：二维数组

  数组1：[[身高1,体重1],[身高2,体重2],[身高3,体重3]]

- 图表类型：

  在series下设置type:scatter

  **xAxis和yAxis的type都要设置为value**
  
  ```js
  const myChart = echarts.init(document.querySelector('div'));
  myChart.setOption({
    xAxis: {
      type: 'value',
      scale:true
    },
    yAxis: {
      type: 'value',
      scale:true
    },
    series: [{
      data: axisdata,
      type: 'scatter'
    }]
  })
  ```

###  常见的效果

- 气泡图效果不同
  - 散点的大小不同
  
  - 散点的颜色不同
  
    ```js
    series: [{
      data: axisdata,
      type: 'scatter',
      // symbolSize:10 ,// 改变散点的大小
      // 不同的元素，散点的大小不同
      symbolSize: function (arg) {
        const height = arg[0];
        const weight = arg[1];
        if ((weight / Math.pow(height / 100, 2) > 25)) {
          return 15;
        } 
          return 5;
        
      },
      // 控制样式
      itemStyle: {
        // color:'green'
        color: function (arg) {
          const height = arg.data[0];
          const weight = arg.data[1];
          if ((weight / Math.pow(height / 100, 2) > 25)) {
          return 'pink';
        } 
          return 'blue';
        
        }
      }
    }]
    ```
  
    
  
    ![image-20210928121227440](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109281213005.png)
  
- 涟漪动画效果

  type: effectScatter

  ```js
  series: [{
    data: axisdata,
    type: 'effectScatter',
    showEffectOn:'emphasis', // render（页面加载完成后显示） emphasis(鼠标滑过的时候显示)
    rippleEffect:{
      scale:5
    },
  }]
  ```

  

## 直角坐标系的常用配置

直角坐标系图表：柱状图 折线图 散点图

### 配置1：网格grid

grid 是用来控制直接坐标系的布局和大小。

```js
grid:{
  show:true, // 显示grid
  borderWidth:10, // 边框
  borderColor:'pink', //边框颜色
  // grid的位置和大小
  top:120,  
  left:120,
  width:600,
  height:140,
},
```

![image-20210928143034621](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109281430660.png)

### 配置2：坐标轴axis

坐标轴分为x轴和y轴，一个grid中最多有两种位置的x轴和y轴。

- 坐标轴类型type

  value：数值轴，自动从目标数据中读取数据

  category：类目轴，该类型需要通过data设置类目数据
  
- 显示位置 position

  xAxis：可取值为top或者bottom

  yAxis：可以取值left或者right

### 配置3：区域缩放 dataZoom

dataZoom 用于数据缩放，对数据范围过滤，x轴和y轴都可以拥有。

dataZoom 是一个数组，意味着可以配置多个区域缩放器。

- type

  slider：滑块

  ![image-20210928145615405](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109281456457.png)

  inside：内置，依靠鼠标滚轮或者双指缩放

  ![image-20210928145636483](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109281456524.png)

  ```js
  dataZoom:[{
    type: 'inside' // slider 滑块 inside：内置，依靠鼠标滚轮或者双指缩放
  }],
  ```

  

- 指明 dataZoom 作用的坐标轴

  xAxisIndex:设置缩放组件控制的是哪个x轴，一般写0即可

  yAxisIndex:设置缩放组件控制的是哪个y轴，一般写0即可

  ```js
  dataZoom: [
    {
      // slider 滑块 inside：内置，依靠鼠标滚轮或者双指缩放
      type: 'slider', 
      xAxisIndex:0
    },
    {
      type: 'slider', 
      yAxisIndex:0
    }
  ],
  ```

- 指明初始状态的缩放情况

  start：数据窗口范围的起始百分比

  end：数据窗口范围结束的百分比

  ```js
  dataZoom: [
    {
      // slider 滑块 inside：内置，依靠鼠标滚轮或者双指缩放
      type: 'slider', 
      xAxisIndex:0
    },
    {
      type: 'slider', 
      yAxisIndex:0,
      start:0,
      end:50 //指的是最高点的50%
    }
  ],
  ```



## 饼图

饼图可以很好地帮助用户快速了解不同分类的数据占比情况。

### 实现步骤

- ECharts 最基本的代码结构：

  引入js文件，设置DOM容器，初始化对象，设置options

- 数据准备：

  `[{name:'淘宝',value:11232},{name:'闲鱼',value:5632},{name:'京东',value:7885},{name:'拼多多',value:4632}]`

- 图表类型：

  在series下设置type:pie

  ```js
  const myChart = echarts.init(document.querySelector('div'));
  myChart.setOption({
    series:[{
      type:'pie',
      data:[{name:'淘宝',value:11232},{name:'闲鱼',value:5632},{name:'京东',value:7885},{name:'拼多多',value:4632}]
    }]
  });
  ```

### 常见效果

- 显示数值

  label.formatter

  ```js
  series:[{
    type:'pie',
    data:[{name:'淘宝',value:11232},{name:'闲鱼',value:5632},{name:'京东',value:7885},{name:'拼多多',value:4632}],
    label:{
      // 是否显示标签 
      show:true, //默认为true
      //决定文字显示的内容
      // formatter:'{b}-{c}-{d}%', // 淘宝-11232-28.23%
      formatter:function(arg){
        return `${arg.name}-${arg.value}`;
      }
    }
  }]
  ```

  ![image-20210928153224578](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109281532648.png)

- 圆环 

  设置两个半径 `radius:['40%','80%']`

  ```js
  series: [{
    type: 'pie',
    data: [{ name: '淘宝', value: 11232 }, { name: '闲鱼', value: 5632 }, { name: '京东', value: 7885 }, { name: '拼多多', value: 4632 }],
    // 20代表20px；20%代表相对于其容器的高度或者宽度较小的值的一半
    radius: ['40%', '80%'],   // 较小的圆半径为40%，较大的为80%
  
  }]
  ```

  

  <img src="https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109281537737.png" alt="image-20210928153703675"  />

- 南丁格尔图：

  `roseType:'radius'`

  ```js
  myChart.setOption({
    series: [{
      type: 'pie',
      data: [{ name: '淘宝', value: 11232 }, { name: '闲鱼', value: 5632 }, { name: '京东', value: 7885 }, { name: '拼多多', value: 4632 }],
      roseType: 'radius' //南丁格尔图 饼图的每块半径都不一样
    }]
  });
  ```

  ![image-20210928154303804](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109281543847.png)

- 选中效果

  ```js
  // 设置为single 选中的一部分会偏离圆点一小段 选中其他的之前的选中块又会回来
  // selectedMode:'single' 
  selectedMode:'multiple', // 选中其他块，偏离不会回来
  // 设置偏离的距离
  selectedOffset:30
  ```

  ![image-20210928154757511](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109281547552.png)

## 地图

地图可以帮助我们从宏观的角度快速地分析出不同地理位置上的数据差异。[官网文档](https://echarts.apache.org/zh/option.html#geo)

地图图表的使用方式：

- 百度地图Api

  需要申请百度地图的 access key

- 矢量地图

  需要准备矢量地图的数据

### 实现步骤

- ECharts 最基本的代码结构：

  引入js文件，设置DOM容器，初始化对象，设置options

- 准备中国的矢量地图json文件，放到json/map/的目录下

- 使用ajax获取china.json

- 在回调函数中向echarts全局对象注册地图json数据

  `echarts.registerMap('chinaMap',chinaJson)`

- 在geo节点下设置 `type:'map' map:'chinaMap'`

  ```js
  const myChart = echarts.init(document.querySelector('div'));
  fetch('./JSON/map/china.json').then(res=>res.json()).then((data)=>{
    echarts.registerMap('chinaMap',data); //echarts全局对象注册地图json数据
    myChart.setOption({
      geo:{
        type:'map', 
        map:'chinaMap'
      }
    })
  })
  ```
  
  ![image-20210928162654623](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109281626661.png)

### 常用配置

- 缩放拖动

  roam

- 名称显示

  label

- 初始缩放比例

  zoom

- 地图中心点

  center

```js
const myChart = echarts.init(document.querySelector('div'));
fetch('./JSON/map/china.json').then(res=>res.json()).then((data)=>{
  echarts.registerMap('chinaMap',data);
  myChart.setOption({
    geo:{
      type:'map',
      map:'chinaMap',
      roam:true, // 设置允许缩放以及允许拖动地图
      // 只缩放 scale 只拖动 move
      label:{
        show:true, //展示所有标签
      },
      zoom:1,//设置初始化缩放比例
      center:[99,40] //根据经度纬度设置 某个地区为中心
    }
  })
})
```

### 常见效果

#### 不同的城市显示不同的颜色

1. 显示基本的中国地图

2. series中设置城市的空气质量

3. 将series下的数据和geo关联在一起

   `geoIndex:0` ,`type:'map'`

4. 结合visualMap配合使用

   min max inRange

   ```js
       const myChart = echarts.init(document.querySelector('div'));
       const airData = [
           { name: '北京', value: 39.92 },
           { name: '天津', value: 39.13 },
           { name: '上海', value: 31.22 },
           { name: '重庆', value: 66 },
           { name: '河北', value: 147 },
           { name: '河南', value: 113 },
           { name: '云南', value: 25.04 },
           { name: '辽宁', value: 50 },
           { name: '黑龙江', value: 114 },
           { name: '湖南', value: 175 },
           { name: '安徽', value: 117 },
           { name: '山东', value: 92 },
           { name: '新疆', value: 84 },
           { name: '江苏', value: 67 },
           { name: '浙江', value: 84 },
           { name: '江西', value: 96 },
           { name: '湖北', value: 273 },
           { name: '广西', value: 59 },
           { name: '甘肃', value: 99 },
           { name: '山西', value: 39 },
           { name: '内蒙古', value: 58 },
           { name: '陕西', value: 61 },
           { name: '吉林', value: 51 },
           { name: '福建', value: 29 },
           { name: '贵州', value: 71 },
           { name: '广东', value: 38 },
           { name: '青海', value: 57 },
           { name: '西藏', value: 24 },
           { name: '四川', value: 58 },
           { name: '宁夏', value: 52 },
           { name: '海南', value: 54 },
           { name: '台湾', value: 88 },
           { name: '香港', value: 66 },
           { name: '澳门', value: 77 },
           { name: '南海诸岛', value: 55 }
       ]
       fetch('./JSON/map/china.json').then(res => res.json()).then(data => {
         // console.log(data);
         echarts.registerMap('chinaMap',data);
         myChart.setOption({
           geo:{
             map:'chinaMap',
             type:'map'
           },
           series:[{
             data:airData,
             // 将series下的数据和geo关联起来
             geoIndex:0, //将空气质量的数据和第0个geo配置关联在一起
             type:'map',
           }],
           visualMap:{
             min:0,
             max:300,
             inRange:{
               color:['white','red'] //修改颜色
             }
           }
         })
       })
   ```

   ![image-20210928175640244](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109281756290.png)

   另外，还有一个属性为 `calcuable`, 将其设置为 true，通过拖动手柄可以对其筛选。下图筛选的是0~118的数据

   ![image-20210928175816418](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109281758460.png)

#### 地图和散点图结合使用

1. 给series下增加新的对象

2. 准备好散点数据，设置给新对象的data

3. 配置新对象的type

   type:effectScatter

4. 让散点图使用地图坐标系统

   coordinateSystem:'geo'

5. 让涟漪效果更加明显

   rippleEffect:{scale:10}

   ```js
   series: [
     {
       type: 'effectScatter',
       data: scatterData,
       coordinateSystem: 'geo', // 指明散点使用的坐标系，geo的坐标系
       rippleEffect: {
         scale: 10, //设置涟漪动画的效果的大小
       }
     }
   ],
   ```

   ![image-20210929170828567](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109291708625.png)

## 雷达图

雷达图可以用来分析多个维度的数据与标准数据的对比情况。

### 实现步骤

- ECharts 最基本的代码结构：

   引入js文件，设置DOM容器，初始化对象，设置options
   
- 在radar节点下定义各个维度的**最大值**

   indicator:[{name:'易用性',max:100},{name:'功能',max:100},{name:'拍照',max:100},{name:'跑分',max:100},{name:'续航',max:100}]

- 准备具体产品的数据

   data: [{ name: '华为手机1', value: [48, 88, 99, 66, 77] }, { name: '小米手机', value:[88, 90, 89, 95, 87] }]

- 图表类型

   在series下设置type:radar

   ```js
   const myChart = echarts.init(document.querySelector('div'));
   myChart.setOption({
     radar: {
       indicator: [{ name: '易用性', max: 100 }, { name: '功能', max: 100 }, { name: '拍照', max: 100 }, { name: '跑分', max: 100 }, { name: '续航', max: 100 }],
   
     },
     series: [{
       type: 'radar',
       data: [{ name: '华为手机1', value: [48, 88, 99, 66, 77] }, { name: '小米手机', value:[88, 90, 89, 95, 87] }]
     }]
   })
   ```

   

   ![image-20210929172444874](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109291724913.png)

### 常用配置

- 显示数值

  label:true

- 区域面积（配置在series节点下）

  areaStyle:{}

- 配置雷达图最外层的展示类型（配置在radar节点下）

  shape:'circle' // 默认为polygen

  ```js
  myChart.setOption({
    radar: {
      indicator: [{ name: '易用性', max: 100 }, { name: '功能', max: 100 }, { name: '拍照', max: 100 }, { name: '跑分', max: 100 }, { name: '续航', max: 100 }],
      shape:'circle' // 默认值为polygon
    },
    series: [{
      type: 'radar',
      data: [{ name: '华为手机1', value: [48, 88, 99, 66, 77] }, { name: '小米手机', value:[88, 90, 89, 95, 87] }],
      areaStyle:{}
    }],
    label:{
      show:true
    },
     legend:{}
  })
  ```

  ![image-20210929173611589](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109291736631.png)

## 仪表盘

仪表盘主要用在进度把控以及数据范围的监控。

### 实现步骤

- ECharts 最基本的代码结构：

  引入js文件，设置DOM容器，初始化对象，设置options

- 准备数据，设置给series下的data

  data：[{value:97}]

- 图表类型：

  在series下设置type:gauge

  ```js
  const myChart = echarts.init(document.querySelector('div'));
  myChart.setOption({
    series: [
      {
        data: [
          { value: 97 },// 每一个对象就代表一个指针
          // 每一个对象就代表一个指针
        ],
        type: 'gauge'
      }
    ]
  })
  ```

  ![image-20210929175100354](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109291751403.png)

### 常见效果

- 数值范围：

  min、max来控制

  ```js
  myChart.setOption({
    series: [
      {
        data: [
          { value: 97 },// 每一个对象就代表一个指针
          // 每一个对象就代表一个指针
        ],
        type: 'gauge',
        min:50 //控制仪表盘的范围
      }
    ],
  })
  ```

  ![image-20210929175328268](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109291753316.png)


## 总结

### 特点总结

- 柱状图：柱状图描述的是分类数据，呈现的是每一个分类中有多少？,图表所表达出来的含义在于不同类别 数据的排名对比情况
- 折线图：折线图更多的使用来呈现数据随时间的『变化趋势』
- 散点图：散点图可以帮助我们推断出不同维度数据之间的相关性。比如在之前的例子中,看得出身高和体重是正相关, 身高越高, 体重越重；散点图也经常用在地图的标注上。
- 饼图：饼图可以很好地帮助用户快速了解不同分类的数据的占比情况
- 地图：地图主要可以帮助我们从宏观的角度快速看出不同地理位置上数据的差异
- 雷达图：雷达图可以用来分析多个维度的数据与标准数据的对比情况
- 仪表盘：仪表盘可以更直观的表现出某个指标的进度或实际情况

### 配置项总结

![image-20210930093001344](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109300930460.png)

# Echarts 进阶

## 显示相关

### 主题

#### 内置主题

Echarts 内置了两套主题：light dark。

- 在初始化对象方法init中可以声明

  `const chart = echarts.init(dom,'light');`

  ![dark主题](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109301022634.png)

#### 自定义主题

1. 在[主题编辑器](https://echarts.apache.org/zh/theme-builder.html)中编辑主题

   ![image-20210930102607631](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109301026746.png)

2. 下载主题，是一个js文件

3. 引入主题js文件

4. 在init方法中使用主题

### 调色板

#### 调色盘

它是一组颜色，图形和系列会自动从中选取颜色。

- 主题调色盘

  操作上面定义主题一样

- 全局调色盘

  如果配置了，将会覆盖主题的调色盘：`myCharts.setOption({color:['red','green','blue']})`

- 局部调色盘

  `series:[{type:'bar',color:['red','green','blue']}]`

#### 颜色渐变

- 线性渐变

  ```js
  series: {
    data: [83, 98, 90, 89, 92],
    type: 'bar',
    markPoint: {
      data: [
        {
          type: 'max',
          name: '最大值'
        },
        {
          type:'min',
          name:'最小值'
        }
      ]
    },
    markLine:{
      data:[{
        type:'average',
        name:'平均值'
      }]
    },
    label:{
      show:true,
      position:'inside'
    },
    barWidth:'40%',
    itemStyle:{
      color:{
        type:'linear', // 线性渐变
        x:0, // 定义颜色渐变的方向
        y:0,
        x2:0,
        y2:1,
        colorStops:[
          {offset:0,color:'red'}, // 百分之0处的颜色为红色
          {offset:1,color:'pink'} // 百分之100处的颜色为蓝色 
        ]
      }
    }
  }
  ```

  ![image-20210930105849775](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109301058837.png)

- 径向渐变

  ```js
  itemStyle:{
    // color:{
    //   type:'linear', // 线性渐变
    //   x:0, // 定义颜色渐变的方向
    //   y:0,
    //   x2:0,
    //   y2:1,
    //   colorStops:[
    //     {offset:0,color:'red'}, // 百分之0处的颜色为红色
    //     {offset:1,color:'pink'} // 百分之100处的颜色为蓝色 
    //   ]
    // }
    color:{
      type:'radial',
      x:0.5, // 圆心的x坐标
      y:0.5, // 圆心的y坐标
      r:1, // 半径
      colorStops:[
        {offset:0,color:'red'},
        {offset:1,color:'blue'}
      ]
  
    }
  }
  ```

  ![image-20210930110554025](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109301105076.png)

### 样式

直接样式和高亮样式，优先级较高，会覆盖主题和调色盘的效果

#### 直接样式

itemStyle, textStyle, lineStyle, areaStyle, label

```js
myChart.setOption({
  title:{
    text:'饼图的测试',
    // 2.textStyle
    textStyle:{
      color:'red'
    }
  },
  series: {
    color: ['blue', 'red', 'purple', 'pink', 'green'],
    type: 'pie',
    data: [{
      name: '张三', value: 33,
      // 1.itemStyle控制张三这一区域的样式
      itemStyle: {
        color:'yellow'
      },
      // 3.label
      label:{
        color:'green'
      }
    },
    { 'name': '王五', value: 44 },
    { 'name': '赵六', value: 54 },
    { 'name': '李四', value: 39 },
    { 'name': '老张', value: 63 }
    ],
    label: {
      show: true,
      formatter: '{b}-{c}',
    }
  }
})
```

![image-20210930112829865](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109301128919.png)

#### 高亮样式

在 emphasis 中包裹 itemStyle, textStyle, lineStyle, areaStyle, label

```js
series: {
  color: ['blue', 'red', 'purple', 'pink', 'green'],
  type: 'pie',
  data: [{
    name: '张三', value: 33,
    // 1.itemStyle控制张三这一区域的样式
    itemStyle: {
      color:'yellow'
    },
    // 3.label
    label:{
      color:'green'
    },
    // 高亮样式
    emphasis:{
      itemStyle:{color:'pink'},
      label:{color:'black'}
    }
  },
  { 'name': '王五', value: 44 },
  { 'name': '赵六', value: 54 },
  { 'name': '李四', value: 39 },
  { 'name': '老张', value: 63 }
  ],
  label: {
    show: true,
    formatter: '{b}-{c}',
  }
}
})
```



### 自适应  

当浏览器的大小发生变化的时候，图表也能随之适配变化。

1. 监听窗口大小变化的事件（resize）

2. 在事件处理函数中调用Echarts实例对象中的resize即可。

   ```js
   window.addEventListener('resize',()=>{
       myChart.resize();
   })
   ```

   

## 动画的使用

### 加载动画

ECharts已经内置好了加载数据的动画，我们只需要在合适的时机显示或者隐藏即可

- 显示加载动画

  `myChart.showLoading()`

- 隐藏加载动画

  `myChart.hideLoading()`

  ```js
  const myChart = echarts.init(document.querySelector('div'));
  myChart.showLoading();
  fetch('./Json/data.json').then(res => res.json()).then(data =>{
    myChart.hideLoading();
    const dataArr = data.map(item =>[item.height,item.weight]);
    myChart.setOption({
      xAxis:{
        type:'value',
        scale:true
      },
      yAxis:{
        type:'value',
        scale:true,
      },
      series:{
        type:'scatter',
        data:dataArr
      }
    })
  })
  ```

  ![image-20210930120726408](https://gitee.com/zyxbj/image-warehouse/raw/master/pics/202109301207486.png)

### 增量动画

所有数据的更新都是通过setOption来实现，不需要考虑数据到底发生了那些变化。

新旧option并不是相互覆盖的关系，是相互整合的关系。设置新的option时，只需要考虑变化的部分就可以了。

```js
const btnmodify = document.querySelector('#modify');
const btnadd = document.querySelector('#add');
btnmodify.addEventListener('click', () => {
  const newData = [25, 33, 55, 66];
  // 新旧option并不是相互覆盖的关系，是相互整合的关系
  // 设置新的option时，只需要考虑变化的部分就可以了。
  myCharts.setOption({
    series: [{
      data: newData,

    }],
  });
})
btnadd.addEventListener('click', () => {
  const newData = [66, 72, 65, 88, 43];
  const newName = ['张三', '李四', '王五', '赵六', '小吴'];
  myCharts.setOption({
    xAxis: {
      data: newName
    },
    series: [{
      data: newData,

    }],
  })
})
```

全部代码：https://codepen.io/liujiaqi222/pen/ZEyPGNp

### 动画配置

- 开启动画

  animation:true

- 动画时长

  animationDuration:1000 (以毫秒为单位)

- 缓动动画

  animationEasing:'bounceOut'

  ```js
  myCharts.setOption({
    animation: true,
    animationDuration: function (arg) {
      console.log(arg); // 会打印动画元素的索引值
      return arg * 2000;
    },
    animationEasing: 'bounceOut', // linear 等等
  });
  ```

  

- 动画阈值

  animationThreshold:10 // 同一类的动画，最多只展示10个

## 交互API

### 全局的echarts对象

全局[echarts对象](https://echarts.apache.org/zh/api.html#echarts)是引入echarts.js文件之后就可以直接使用的对象

- init 初始化echarts实例对象，使用主题

- registerTheme 注册主题，只有注册过的主题，才能在init方法中使用该主题

- registerMap 注册地图数据

  ```js
  fetch('./JSON/map/china.json').then(res=>res.json()).then((data)=>{
    // 注册地图
    echarts.registerMap('chinaMap',data);
    myChart.setOption({
      geo:{
        type:'map',
        map:'chinaMap',
        roam:true, // 设置允许缩放以及允许拖动地图
        // 只缩放 scale 只拖动 move
        label:{
          show:true, //展示所有标签
        },
        zoom:1,//设置初始化缩放比例
        center:[99,40] //根据经度纬度设置 某个地区为中心
      }
    })
  })
  ```

  

- connect

  一个页面中可以有多个独立的图表，每一个图表都对应着一个ECharts实例对象。

  connect可以实现多图关联，传入联动目标为ECharts实例对象，支持数组。保存图片的时候会自动拼接，刷新按钮，重置按钮，提示框联动，图例选择，数据范围修改等。

  ```js
  echarts.connect([mychart1,mychart2])
  ```

### echartsInstance对象

[eChartsInstance对象](https://echarts.apache.org/zh/api.html#echartsInstance)是通过echarts.init方法调用后的返回值

- setOption

  设置或修改图表实例的配置项以及数据，可多次调用setOption方法（会合并新的配置和旧的配置）。

- resize

  重新计算和绘制图表

  ```js
  const myChart = echarts.init(dom);
  window.addEventListener('resize',()=>{
      myChart.resize();
  })
  ```

- [on\off](https://echarts.apache.org/zh/api.html#events)

  绑定或者解绑事件处理函数

  - 鼠标事件

    ```js
    // 对事件监听
    myChart.on('click',function(arg){
      console.log(arg);
      console.log('click...');
    });
    ```

  - ECharts事件

    常见事件：legendSelectChanged, 'dataZoom','pieSelectChanged','mapSelectChanged'等

    ```js
    myChart.on('legendSelectChanged',function(arg){
      console.log(arg); //打印选中状态
    })
    ```

- [dispatchAction](https://echarts.apache.org/zh/api.html#action)

  触发某些行为（使用代码模拟用户的行为）

  ```js
  mCharts.dispatchAction({
      type:'hightlight', //事件类型
      seriesIndex:0, //图表索引
      dataIndex:1 //图表中哪一项高亮
  })
  ```

  

- clear

  清空当前实例，会移除实例中所有的组件和图表；

  清空之后，可以再次setOption

  ```js
  myChart.clear(); // 整个图表将不会显示
  myChart.setOption(option); // 图表将会重新展示
  ```

- dispose

  销毁实例，实例销毁后无法再被使用

  ```js
  myChart.clear(); 
  myChart.setOption(option);   // 会报错
  ```
