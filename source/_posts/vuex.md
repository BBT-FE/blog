---
title: vuex
date: 
tag:
 - 出云 
photos:
 - http://img.mp.itc.cn/upload/20161008/e8fadf087de643e8862d2836d0a65bba_th.png
---

vue - vuex 学习
<!--more-->

#### 1、vuex
  vuex 就是使用一个 store 对象来包含所有的应用层级状态，也就是数据的来源
  例如： 
  ```js
    const moduleA = {
      state: { ... },
      mutations: { ... },
      actions: { ... },
      getters: { ... }
    }

    const moduleA = {
      state: { ... },
      mutations: { ... },
      actions: { ... },
      getters: { ... }
    }

    const store = new Vuex.Store({
      modules: {
        a: moduleA,
        B: moduleB
      }
    })

    store.state.a => moduleA的状态
    store.state.b => moduleB的状态
  ```

#### 2、store 的四个属性：state， getters, mutations, actions 


  state: state 上存放的，说的简单一些就是变量，也就是所谓的状态。没有使用 state 的时候，我们都是直接在 data 中进行初始化的，但是有了 state 之后，我们就把 data 上的数据转移到 state 上去了。当一个组件需要获取多个状态时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 mapState 辅助函数帮助我们生成计算属性，
  ```js
    import { mapState } from 'vuwx'

    export default {
      
      computed: mapState({

        count: state => state.count,

        countAlias: 'count',

        countPlusLocalState (state) {
          return state.count + this.localCount
        }
      })
    }
    // 其实就是把 state 上保存的变量转移到计算属性上。当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 mapState 传一个字符串数组。

    computed: mapState([
      // 映射 this.count 为 store.state.count
      'count'
    ])

    //为了更好地理解这个函数的作用，我们可以看看它的源代码。

    export function mapState(state) {
      const res = {}
      normalizeMap(state).forEach(({ key, val }) => {
        res[key] = function mappedState () {
          return typeof val === 'function' ? val.call(this, this.$route.state, this.$route.getters) : this.$store.state[val]
        }
      })
      return res
    }

    function normalizeMap (map) {
      return Array.isArray(map) ? map.map(key => ({ key, val: key })) : Object.keys(map).map(key => ({ key, val: map[key] }))
    }

    mapstate 即可以接受对象，也可以接受数组。最终返回的是一个对象。并且 res[key] 的值都是来于 store,这也是映射
  ```

  getters: getters上简单来说就是存放一些公共函数供组件调用。getters 会暴露为 store.getters 对象，也就是说可以通过 this.$store.getters.valueName对派生出来的状态进行访问来进行相应的调用。mapGetters 辅助函数仅仅是将 store 中的 getters 映射到局部计算属性，其实也就是从 getters 中获取对应的属性，跟解构类似
  ```js
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

  // 如果你想将一个 getter 属性另取一个名字，使用对象形式：
  mapGetters({
    // 映射 `this.doneCount` 为 `store.getters.doneTodosCount`
    doneCount: 'doneTodosCount'
  })
  ```

  mutations: Mutations的中文意思是“变化”，利用它可以更改状态。事实上，更改 Vuex 的 store 中的状态的唯一方法就是提交 （commit）mutation。不过，mutation触发状态改变的方式有一点特别，所谓commit一个mutation，实际是像触发一个事件一样，传入一个mutation的类型以及携带一些数据（称作payload，载荷）。
  `注意： 一条重要的原则就是要记住 mutation 必须是同步函数。`
  ```js
    即：

    mutations: {   //放置mutations方法
      increment(state, payload) {
        //在这里改变state中的数据
        state.count = payload.number;
      }
    },

    // commit一个mutation在代码层面怎么表示呢？
    this.$store.commit('increment', {
      amount: 10
    })

或者： 

    const store = new Vuex.Store({
      state: {
        count: 1
      },
      mutations: {
        increment (state) {
          // 变更状态
          state.count++
        }
      }
    })

    store.commit('increment')

  // 当 mutation 事件类型比较多的时候，我们可以使用常量替代 mutation 事件类型。同时把这些常量放在单独的文件中可以让我们的代码合作者对整个 app 包含的 mutation 一目了然：
  // mutation-types.js
  export const SOME_MUTATION = 'SOME_MUTATION'

  // store.js
  import Vuex from 'vuex'
  import { SOME_MUTATION } from './mutation-types.js'

  const store = new Vuex.State({
    state: { ... },
    mutations: {
       // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
      
      [SOME_MUTATION] (state) {

      }
    }
  })


  // 除了这种使用 this.$store.commit('xxx') 提交 mutation的方式之外，还有一种方式，即使用 mapMutations 辅助函数将组件中的 methods 映射为 this.$store.commit。例如：

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

  actions: 前面说了，mutation 像事件注册，需要相应的触发条件。而 Action 就那个管理触发条件的。
  Action 类似于 mutation，不同在于：Action 提交的是 mutation，而不是直接变更状态。Action 可以包含任意异步操作。 

  ```js
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }

  // Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 context.commit 提交一个 mutation，或者通过 context.state 和 context.getters 来获取 state 和 getters。
  // mutation处理函数中所做的事情是改变state，而action处理函数中所做的事情则是commit mutation。
  // 实践中，我们会经常会用到 ES2015 的 参数解构 来简化代码（特别是我们需要调用 commit 很多次的时候）：
  actions: {
    increment ({ commit }) {
      commit('increment')
    }
  }

  // 还记得我们前面说过 mutation 像事件类型吗？因此需要我们给定某个动作来进行触发。而这就是分发 action。Action 通过 store.dispatch 方法触发：

  store.dispatch('increment')

或者：
  // 所传的两个参数一个是要触发的action的类型，一个是所携带的数据（payload），类似于上文所讲的commit mutation时所传的那两个参数。
  this.$store.dispatch(actionType, payload)。
  
  // 以载荷形式分发
  this.$store.dispatch('incrementAsync', {
    amount: 10
  })

  或

  // 以对象形式分发
  this.$store.dispatch({
    type: 'incrementAsync',
    amount: 10
  })

mapActions: 

  // 你在组件中使用 this.$store.dispatch('xxx') 分发 action，或者使用 mapActions 辅助函数将组件的 methods 映射为 store.dispatch 调用（需要先在根节点注入 store）：

  import { mapActions } from 'vuex'

  export default {
    // ...
    methods: {
      ...mapActions([
        'increment' // 映射 this.increment() 为 this.$store.dispatch('increment')
      ]),
      ...mapActions({
        add: 'increment' // 映射 this.add() 为 this.$store.dispatch('increment')
      })
    }
  }

  // 这句话意思其实是，当你使用了 mapActions， 你就不需要再次使用 this.$store.dispatch('xxx')，当你没使用的话，你可以需要手动去分法。比如下面的代码：

  methods: {
    saave() {
      date: this.date,
      totalTime: this.totalTime,
      comment: this.comment
    };
    this.$store.dispatch('savePlan', Plan)
    this.$store.dispatch('addTotalTime', totalTime)

  }

action 内部执行异步操作：

  actions: {
    incrementAsync ({ commit }) {
      setTimeout(() => {
        commit('increment')
      }, 1000)
    }
  }

  // this.$store.dispatch 可以处理被触发的 action 的处理函数返回的 Promise，并且 this.$store.dispatch 仍旧返回 Promise。
  actions: {
    actionA ({ commit }) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          commit('someMutation')
          resolve()
        }, 1000)
      })
    }
  }

  $this.store.dispatch('actionA').then(() => {
    // ...
  })


  actions: {
  // ...
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }

最后，如果我们利用 async / await 这个 JavaScript 即将到来的新特性，我们可以像这样组合 action：

  // 假设 getData() 和 getOtherData() 返回的是 Promise

  actions: {
    async actionA ({ commit }) {
      commit('gotData', await getData())
    },
    async actionB ({ dispatch, commit }) {
      await dispatch('actionA') // 等待 actionA 完成
      commit('gotOtherData', await getOtherData())
    }
  }
  这实际上就实现了action嵌套另一个action的情况。

  讲了这么多action，你可能会纳闷一个问题：action这鬼东西不是和mutation看起来好多地方都很像吗，直接触发mutation去改变state岂不更好？为什么还要先触发action，再由action去触发mutation才达到改变state的目的？

  这是个好问题。还记得上面我们提到过mutation只能是同步的操作而action可以是包含异步操作吗？那么，若想进行异步操作，通过mutation显然是无法完成的，所以就有了action。我们来看一个更加实际的购物车示例，涉及到调用异步 API 和分发多重 mutation：

  actions: {
    checkout ({ commit, state }, products) {
      // 把当前购物车的物品备份起来
      const savedCartItems = [...state.cart.added]
      // 发出结账请求，然后乐观地清空购物车
      commit(types.CHECKOUT_REQUEST)
      // 购物 API 接受一个成功回调和一个失败回调
      shop.buyProducts(
        products,
        // 成功操作
        () => commit(types.CHECKOUT_SUCCESS),
        // 失败操作
        () => commit(types.CHECKOUT_FAILURE, savedCartItems)
      )
    }
  }
  ```



#### 3、vue store存储commit和dispatch
  this.$store.commit('toShowLoginDialog', true);
  this.$store.dispatch('toShowLoginDialog',false)
  主要区别是：
  dispatch：含有异步操作，例如向后台提交数据，写法： this.$store.dispatch('mutations方法名',值)
  commit：同步操作，写法：this.$store.commit('mutations方法名',值)