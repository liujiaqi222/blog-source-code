---
title: Vuex学习笔记
date: 2021-08-22 17:55:37
tags: [Vue, Vuex]
---

笔记参考文档：https://next.vuex.vuejs.org/zh/guide/

# Vuex

## 1.什么是vuex

概念：专门在 Vue 中实现集中式状态（数据）管理的一个 Vue 插件，对 vue 应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。  

## 2.什么时候使用 Vuex  

1. 多个组件依赖于同一状态

2. 来自不同组件的行为需要变更同一状态  

## 3.Vuex工作原理

![vuex](https://next.vuex.vuejs.org/vuex.png)

## 4.vuex安装与导入

```bash
yarn add vuex@next 
```

```js
import { createApp } from 'vue'
import { createStore } from 'vuex'
import App from './App.vue'

// 创建一个新的 store 实例
const store = createStore({
  state () {
    return {
      count: 0
    }
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})

const app = createApp(App)

// 将 store 实例作为插件安装
app.use(store).mount('#app');
```

单文件来写：

```js
// main.js
import { createApp } from 'vue'
import App from './App.vue'
import store from './store/index.js'

createApp(App).use(store).mount('#app');

```

```js
//store/index.js
import { createStore } from 'vuex'

// 准备actions 用于响应组件中的动作
const actions = {};
// 准备mutations 用于操作数据(state)
const mutations = {};
// 准备state 用于存储数据
const state = {};

// 创建store
const store = createStore({
    actions,
    mutations,
    state(){
        return state;
    }
});

export default store;
```

## 5. vuex小案例

1. 安装，导入vuex，创建store实例。

   ```js
   //store/index.js
   import { createStore } from 'vuex'
   
   // 创建store
   const store = createStore({
       actions: {},
       mutations: {},
       state: {}
   });
   
   export default store;
   ```

   然后在 `main.js` 中引入导出的 `store`。

   ```js
   import { createApp } from 'vue'
   import App from './App.vue'
   import store from './store/index.js'
   
   createApp(App).use(store).mount('#app');
   ```

   

2. 在组件中使用store，首先得从 `vuex` 中导入 `useStore`。

   ```js
   import { useStore } from "vuex";
   export default {
     setup() {
       const store = useStore();
   };
   ```

   这里准备了一个`count.vue` 组件，目的是实现自定义加减。

   ```vue
   <template>
     <h1>当前求和为 </h1>
     <select name="" id="" v-model.number="n">
       <option value="1">1</option>
       <option value="2">2</option>
       <option value="3">3</option>
     </select>
     <button @click="add">+</button>
     <button @click="subtract">-</button>
     <button @click="addOdd">当前求和为奇数才加</button>
     <button @click="addWait">等等再加</button>
   </template>
   ```

   

3. 开始实现功能

   ```vue
   //count.vue
   <script>
   import { ref,computed } from "vue";
   import { useStore } from "vuex";
   
   export default {
     setup() {
       const store = useStore();
   
       let n = ref(1);
   
       function add() {
         // dispatch会触发action函数 第二个参数n.value可以传值
         store.dispatch('add',n.value); // ① dispatch
       }
   
       // 注意这里sum是通过computed
       return { n, add, sum:computed(()=>{
           return store.state.sum;
       })};
     },
   };
   ```

   ```js
   //store/index.js
   import { createStore } from 'vuex'
   
   // 创建store
   const store = createStore({
       actions: {
           add(context,value){ // ②被触发的action
               context.commit('ADD',value) //③ commit
           }
       },
       mutations: {
   
           ADD(state,value){ // ④被触发的mutation
               state.sum+=value;
           }
       },
       state: {sum:0}
   });
   
   export default store;
   ```

   

4. 最终加上一个减法的功能，另外“等等再加”和“奇数再加”只需要触发即可，不需要再次额外定义函数了。

   ```vue
   //count.vue
   <template>
     <h1>当前求和为{{ sum }}</h1>
     <select name="" id="" v-model.number="n">
       <option value="1">1</option>
       <option value="2">2</option>
       <option value="3">3</option>
     </select>
     <button @click="add">+</button>
     <button @click="subtract">-</button>
     <button @click="addOdd">当前求和为奇数才加</button>
     <button @click="addWait">等等再加</button>
   </template>
   
   <script>
   import { ref, computed } from "vue";
   import { useStore } from "vuex";
   
   export default {
     setup() {
       const store = useStore();
   
       let n = ref(1);
   
       function add() {
         // dispatch会触发action函数 第二个参数n.value可以传值
         store.dispatch("add", n.value); // ① dispatch
       }
   
       function subtract() {
         store.dispatch("subtract", n.value);
       }
   
       function addOdd() {
         if (store.state.sum % 2) {
           store.dispatch("add", n.value);
         }
       }
   
       function addWait() {
         setTimeout(() => {
           store.dispatch("add", n.value);
         }, 500);
       }
       // 注意这里sum是通过computed
       return {
         n,
         add,
         subtract,
         addOdd,
         addWait,
         sum: computed(() => {
           return store.state.sum;
         }),
       };
     },
   };
   </script>
   ```

   ```js
   //store/index.js
   import { createStore } from 'vuex'
   
   // 创建store
   const store = createStore({
       actions: {
           add(context,value){ // ②被触发的action
               context.commit('ADD',value) //③ commit
           },
           subtract(context,value){
               context.commit('SUBTRACT',value);
           }
       },
       mutations: {
   
           ADD(state,value){ // ④被触发的mutation
               state.sum+=value;
           },
           SUBTRACT(state,value){
               state.sum-= value;
           }
       },
       state: {sum:0}
   });
   
   export default store;
   ```

   > 最终效果：

![最终效果](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108291505497.gif)

5. 一点优化：

   add和subtract的action没有任何逻辑，因此可以直接commit触发mutation。

   addOdd和addWait的业务逻辑写在actions中比较合适。

   ```js
   function add() {
     // dispatch会触发action函数 第二个参数n.value可以传值
     store.commit("ADD", n.value); // ① dispatch
   }
   
   function subtract() {
     store.commit("SUBTRACT", n.value);
   }
   
   function addOdd() {
     if (store.state.sum % 2) {
       store.dispatch("addOdd", n.value);
     }
   }
   
   function addWait() {
     store.dispatch("addWait", n.value);
   }
   ```

   最后add、addOdd、addWait都是调用`ADD`的mutation。

   ```js
   //store/index.js
   ctions: {
       // add(context, value) { // ②被触发的action
       //     context.commit('ADD', value) //③ commit
       // },
       // subtract(context, value) {
       //     context.commit('SUBTRACT', value);
       // },
       addOdd(context, value) {
           // 注意这里是 context.state
           if (context.state.sum % 2) {
               context.commit('ADD', value) 
           }
       },
       addWait(context,value){
           setTimeout(() => {
               context.commit('ADD', value) 
           }, 500);
       }
   },
   ```

   > NOTE: 如果setup中不提供sum这个state，页面中也可以直接获取到它。但是通过computed属性来返回，就不用写`$store.state` 这个前缀了。

   ```vue
   <template>
      <h1>当前求和为{{ $store.state.sum }}</h1>
   </template>
   ```

   

## 6.多组件vuex小案例

```js
//store/index.js
import { createStore } from 'vuex'

// 创建store
const store = createStore({
    actions: {
        addOdd(context, value) {
            // 注意这里是 context.state
            if (context.state.sum % 2) {
                context.commit('ADD', value) //③ commit
            }
        },
        addWait(context, value) {
            setTimeout(() => {
                context.commit('ADD', value)
            }, 500);
        }
    },
    mutations: {

        ADD(state, value) { // ④被触发的mutation
            state.sum += value;
        },
        SUBTRACT(state, value) {
            state.sum -= value;
        },
        ADD_PERSON(state, value) {
            state.peopleList.push(value);
        }
    },
    state: {
        sum: 0,
        peopleList: [
            { id: '001', name: 'alex' }
        ]
    },
    // getters用于加工state中的数据
    getters: {
        sumTimesTen(state) {
            return state.sum * 10;
        }
    }
});

export default store;
```

```vue
//people.vue
<template>
  <h1>人员列表</h1>
  <h3>Count组件的总和 {{sum}}</h3>
  <input type="text" placeholder="请输入名字" v-model="name"/>
  <button @click='addPerson'>添加</button>
  <ul>
    <li v-for='p in peopleList' :key='p.id'>{{p.name}}</li>
  </ul>
</template>

<script>
import {nanoid} from 'nanoid';
export default {
    data(){
        return {name:''}
    },
    computed:{
        peopleList(){
            return this.$store.state.peopleList;
        },
        sum(){
            return this.$store.state.sum;
        }
    },
    methods:{
       addPerson(){
            const person = {id:nanoid(),name:this.name};
            this.$store.commit('ADD_PERSON',person);
       }
    }
};
</script>

<style>
</style>
```

```vue
//count.vue
<template>
  <h1>当前求和为{{ sum }}</h1>
  <h3>当前求和放大10倍{{ sumTimesTen }}</h3>
  <h3>Person组件的总人数是{{ peopleList.length }}</h3>
  <select name="" id="" v-model.number="n">
    <option value="1">1</option>
    <option value="2">2</option>
    <option value="3">3</option>
  </select>
  <button @click="add(n)">+</button>
  <button @click="subtract(n)">-</button>
  <button @click="addOdd(n)">当前求和为奇数才加</button>
  <button @click="addWait(n)">等等再加</button>
</template>

<script>
import { mapState, mapGetters, mapMutations, mapActions } from "vuex";

export default {
  data() {
    return { n: 1 };
  },
  computed: {
    ...mapState(["sum", "peopleList"]),
    ...mapGetters(["sumTimesTen"]),
  },
  methods: {
    // 由于需要传参，此时的参数通过模板来传递
    ...mapMutations({
      add: "ADD",
      subtract: "SUBTRACT",
    }),
    ...mapActions(["addOdd", "addWait"]),
  },
};
</script>

<style>
button {
  margin: 5px;
}
</style>
```

![image-20210829215744695](https://raw.githubusercontent.com/liujiaqi222/images/master/pics/202108292157773.png)







# 核心概念

## mapXXX

### mapState

ps：实在无法用组合式api写下去了这个了。

当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性。

1. 对象写法

```js
// 在单独构建的版本中辅助函数为 Vuex.mapState
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // 写法一：箭头函数可使代码更简练
    count: state => state.count,

    // 写法二：传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

  })
}
```

2. 数组写法

当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 `mapState` 传一个字符串数组。

```js
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])
```

**`mapState` 函数返回的是一个对象。**我们如何将它与局部计算属性混合使用呢？使用从有了[对象展开运算符](https://github.com/tc39/proposal-object-rest-spread)，我们可以极大地简化写法：

```js
computed: {
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}
```

### mapGetter

`mapGetters` 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性：

```
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```

如果你想将一个 getter 属性另取一个名字，使用对象形式：

```
...mapGetters({
  // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
  doneCount: 'doneTodosCount'
})
```



### mapMutations

你可以在组件中使用 `this.$store.commit('xxx')` 提交 mutation，或者使用 `mapMutations` 辅助函数将组件中的 methods 映射为 `store.commit` 调用（需要在根节点注入 `store`）。

```
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }
}
```

前面的count小案例，经过改造如下：注意此时的传参是通过模板来传递的。

```vue
<template>
  <div>
    <h1>当前求和为{{ sum }}</h1>
    <h3>当前求和放大10倍{{ sumTimesTen }}</h3>
    <h3>姓名：{{ name }} 性别：{{ gender }}</h3>
    <select name="" id="" v-model.number="n">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
    <button @click="add(n)">+</button>
    <button @click="subtract(n)">-</button>
    <button @click="addOdd">当前求和为奇数才加</button>
    <button @click="addWait">等等再加</button>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { useStore, mapState,mapGetters ,mapMutations} from "vuex";

export default {
  setup() {
    const store = useStore();

    let n = ref(1);

    function addOdd() {
      if (store.state.sum % 2) {
        store.dispatch("addOdd", n.value);
      }
    }

    function addWait() {
      store.dispatch("addWait", n.value);
    }

    // 注意这里sum是通过computed
    return {
      n,
      addOdd,
      addWait,
    };
  },
  // 用组合式api实在写不下去了
  computed: {
    
    ...mapState(["sum", "name", "gender"]),
    ...mapGetters(['sumTimesTen'])
  },
  methods:{
    /* add(){
      this.$store.commit("ADD", n.value);
    },
    subtract(){
      this.$store.commit('SUBTRACT')
    }, */
    ...mapMutations({
      // 由于需要传参，此时的参数通过模板来传递
      add:'ADD',subtract:'SUBTRACT',
    })
  }
};
</script>
```



### mapActions

你在组件中使用 `this.$store.dispatch('xxx')` 分发 action，或者使用 `mapActions` 辅助函数将组件的 methods 映射为 `store.dispatch` 调用（需要先在根节点注入 `store`）：

```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
}
```

前面的小案例最终改造结果：

```vue
<template>
  <div>
    <h1>当前求和为{{ sum }}</h1>
    <h3>当前求和放大10倍{{ sumTimesTen }}</h3>
    <h3>姓名：{{ name }} 性别：{{ gender }}</h3>
    <select name="" id="" v-model.number="n">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
    <button @click="add(n)">+</button>
    <button @click="subtract(n)">-</button>
    <button @click="addOdd(n)">当前求和为奇数才加</button>
    <button @click="addWait(n)">等等再加</button>
  </div>
</template>

<script>
import {mapState, mapGetters, mapMutations, mapActions } from "vuex";

export default {
  data() {
    return { n: 1 };
  },
  computed: {
    ...mapState(["sum", "name", "gender"]),
    ...mapGetters(["sumTimesTen"]),
  },
  methods: {
    // 由于需要传参，此时的参数通过模板来传递
    ...mapMutations({
      add: "ADD",
      subtract: "SUBTRACT",
    }),
    ...mapActions(["addOdd", "addWait"]),
  },
};
</script>

```



## Getter

有时候我们需要从 store 中的 state 中派生出一些状态。

```vue
//count.vue
<template>
  <div>
    <h1>当前求和为{{ sum }}</h1>
    <h3>当前求和放大10倍{{ sumTimesTen }}</h3>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { useStore } from "vuex";

export default {
  setup() {
    return {
      sum: computed(() => {
        return store.state.sum;
      }),
      sumTimesTen: computed(() => {
        return store.getters.sumTimesTen;
      }),
    };
  },
};
</script>

```

```js
//store/index.js
import { createStore } from 'vuex'

// 创建store
const store = createStore({
    state: { sum: 0 },
    // getters用于加工state中的数据
    getters:{
        sumTimesTen(state){
            return state.sum*10;
        }
    }
});

export default store;
```



## module

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = createStore({
  modules: {
    a: moduleA,
    b: moduleB
  }
})
```

### 模块的局部状态

对于模块内部的 mutation 和 getter，接收的第一个参数是**模块的局部状态对象**。(state仅仅只指自己的state)

```js
const moduleA = {
  state: () => ({
    count: 0
  }),
  mutations: {
    increment (state) {
      // 这里的 `state` 对象是模块的局部状态
      state.count++
    }
  },

  getters: {
    doubleCount (state) {
      return state.count * 2
    }
  }
}
```



### 命名空间

默认情况下，模块内部的 action 和 mutation 仍然是注册在**全局命名空间**的——这样使得多个模块能够对同一个 action 或 mutation 作出响应。

如果希望你的模块具有更高的封装度和复用性，你可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名。

```js
const store = createStore({
  modules: {
    account: {
      namespaced: true,

      // 模块内容（module assets）
      state: () => ({ ... }), // 模块内的状态已经是嵌套的了，使用 `namespaced` 属性不会对其产生影响
      getters: {
        isAdmin () { ... } // -> getters['account/isAdmin']
      },
      actions: {
        login () { ... } // -> dispatch('account/login')
      },
      mutations: {
        login () { ... } // -> commit('account/login')
      },
    }
  }
})
```

启用了命名空间的 getter 和 action 会收到局部化的 `getter`，`dispatch` 和 `commit`。换言之，你在使用模块内容（module assets）时不需要在同一模块内额外添加空间名前缀。

#### 在带命名空间的模块内访问全局内容

如果你希望使用全局 state 和 getter，`rootState` 和 `rootGetters` 会作为第三和第四参数传入 getter，也会通过 `context` 对象的属性传入 action。

若需要在全局命名空间内分发 action 或提交 mutation，将 `{ root: true }` 作为第三参数传给 `dispatch` 或 `commit` 即可。

```js
modules: {
  foo: {
    namespaced: true,

    getters: {
      // 在这个模块的 getter 中，`getters` 被局部化了
      // 你可以使用 getter 的第四个参数来调用 `rootGetters`
      someGetter (state, getters, rootState, rootGetters) {
        getters.someOtherGetter // -> 'foo/someOtherGetter'
        rootGetters.someOtherGetter // -> 'someOtherGetter'
      },
      someOtherGetter: state => { ... }
    },

    actions: {
      // 在这个模块中， dispatch 和 commit 也被局部化了
      // 他们可以接受 `root` 属性以访问根 dispatch 或 commit
      someAction ({ dispatch, commit, getters, rootGetters }) {
        getters.someGetter // -> 'foo/someGetter'
        rootGetters.someGetter // -> 'someGetter'

        dispatch('someOtherAction') // -> 'foo/someOtherAction'
        dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'

        commit('someMutation') // -> 'foo/someMutation'
        commit('someMutation', null, { root: true }) // -> 'someMutation'
      },
      someOtherAction (ctx, payload) { ... }
    }
  }
}
```

### 带命名空间的绑定函数

当使用 `mapState`、`mapGetters`、`mapActions` 和 `mapMutations` 这些函数来绑定带命名空间的模块时，写起来可能比较繁琐：

```js
computed: {
  ...mapState({
    a: state => state.some.nested.module.a,
    b: state => state.some.nested.module.b
  })
},
methods: {
  ...mapActions([
    'some/nested/module/foo', // -> this['some/nested/module/foo']()
    'some/nested/module/bar' // -> this['some/nested/module/bar']()
  ])
}
```

对于这种情况，你可以将模块的空间名称字符串作为第一个参数传递给上述函数，这样所有绑定都会自动将该模块作为上下文。于是上面的例子可以简化为：

```js
computed: {
  ...mapState('some/nested/module', {
    a: state => state.a,
    b: state => state.b
  })
},
methods: {
  ...mapActions('some/nested/module', [
    'foo', // -> this.foo()
    'bar' // -> this.bar()
  ])
}
```

而且，你可以通过使用 `createNamespacedHelpers` 创建基于某个命名空间辅助函数。它返回一个对象，对象里有新的绑定在给定命名空间值上的组件绑定辅助函数：

```js
import { createNamespacedHelpers } from 'vuex'

const { mapState, mapActions } = createNamespacedHelpers('some/nested/module')

export default {
  computed: {
    // 在 `some/nested/module` 中查找
    ...mapState({
      a: state => state.a,
      b: state => state.b
    })
  },
  methods: {
    // 在 `some/nested/module` 中查找
    ...mapActions([
      'foo',
      'bar'
    ])
  }
}
```

### module小案例

```js
//store/index.js
import { createStore } from 'vuex';
import countOptions from './count';
import peopleOptions from './people';
// 创建store
const store = createStore({
    modules:{
        countOptions,
        peopleOptions
    }
});

export default store;
```

```js
// store/people.js
// 人员管理相关的配置
export default {
    namespaced: true,
    actions: {
        addWang(context, value) {
            // 只能添加王姓名
            if (value.name.indexOf('王') === 0) {
                context.commit('ADD_PERSON', value);
            } else {
                alert('添加的人必须姓王')
            }
        },

    },
    mutations: {
        ADD_PERSON(state, value) {
            state.peopleList.push(value);
        }
    },
    state: {
        peopleList: [
            { id: '001', name: 'alex' }
        ]
    },
    getters: {
        firstPersonName(state) {
            // 这里的state是 peopleOptions的state
            return state.peopleList[0].name;
        }
    }
}

```

```js
// store/count.js
// 求和相关的配置
export default {
    namespaced: true,
    actions: {
        addOdd(context, value) {
            // 注意这里是 context.state
            if (context.state.sum % 2) {
                context.commit('ADD', value)
            }
        },
        addWait(context, value) {
            setTimeout(() => {
                context.commit('ADD', value)
            }, 500);
        }
    },
    mutations: {
        ADD(state, value) {
            state.sum += value;
        },
        SUBTRACT(state, value) {
            state.sum -= value;
        },
    },
    state: { sum: 0 },
    getters: {
        sumTimesTen(state) {
            return state.sum * 10;
        }
    }
}
```

----

```vue
//people.vue
<template>
  <h1>人员列表</h1>
  <h3>Count组件的总和 {{ sum }}</h3>
  <h3>列表中第一个人的名字：{{ firstPersonName }}</h3>
  <input type="text" placeholder="请输入名字" v-model="name" />
  <button @click="addPerson">添加</button>
  <button @click="addWang">添加一个姓王的人</button>
  <ul>
    <li v-for="p in peopleList" :key="p.id">{{ p.name}}</li>
  </ul>
</template>

<script>
import { nanoid } from "nanoid";
export default {
  data() {
    return { name: "" };
  },
  computed: {
    peopleList() {
      // 注意写法
      return this.$store.state.peopleOptions.peopleList;
    },
    sum() {
      return this.$store.state.countOptions.sum;
    },
    firstPersonName() {
      return this.$store.getters["peopleOptions/firstPersonName"];
    },
  },
  methods: {
    addPerson() {
      const person = { id: nanoid(), name: this.name };
      // 注意写法
      this.$store.commit("peopleOptions/ADD_PERSON", person);
      this.name = "";
    },
    addWang() {
      const person = { id: nanoid(), name: this.name };
      console.log(person);
      this.$store.dispatch("peopleOptions/addWang", person);
      this.name = "";
    }
  }

};
</script>

<style>
</style>
```

```vue
//count.vue
<template>
  <h1>当前求和为{{ sum }}</h1>
  <h3>当前求和放大10倍{{ sumTimesTen }}</h3>
  <h3>Person组件的总人数是{{ peopleList.length }}</h3>
  <select name="" id="" v-model.number="n">
    <option value="1">1</option>
    <option value="2">2</option>
    <option value="3">3</option>
  </select>
  <button @click="add(n)">+</button>
  <button @click="subtract(n)">-</button>
  <button @click="addOdd(n)">当前求和为奇数才加</button>
  <button @click="addWait(n)">等等再加</button>
</template>

<script>
import { mapState, mapGetters, mapMutations, mapActions } from "vuex";

export default {
  data() {
    return { n: 1 };
  },
  computed: {
    ...mapState("countOptions", ["sum"]),
    ...mapState({peopleList:state => state.peopleOptions.peopleList}),
    ...mapGetters("countOptions", ["sumTimesTen"]),
  },
  methods: {
    // 由于需要传参，此时的参数通过模板来传递
    ...mapMutations("countOptions", {
      add: "ADD",
      subtract: "SUBTRACT",
    }),
    ...mapActions("countOptions", ["addOdd", "addWait"]),
  },
  
};
</script>
```

