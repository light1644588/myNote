#### vuex

##### `state` 状态

##### `getters` 提取器

​	`vuex`中由state派生出来的属性，类似于单组件中的`computed`，

​	辅助函数 `mapGetters`

```JavaScript
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
   state: {
       todos: [
           { id: 1, text: '...', done: true },
           { id: 2, text: '...', done: false },
       ]
   },
   getters: {
       doneTodos: state => {
           return state.todos.filter(todo => todo.done);
       },
       doneTodosCount: (state, getters) => {
           return getters.doneTodos.length
       },
       getTodoById: (state) => (id) => {
           return state.todos.find(todo => todo.id === id)
       }
   }
});

import { mapGetters } from 'vuex';

new Vue({ 
    el: '#app',
    store,
    data: {
    },
    computed: mapGetters([
        'doneTodos', 'doneTodosCount', 'getTodoById'
    ])
});

console.log(store.getters.getTodoById(48))
```

****

##### `mutation`变化

​	唯一可以更改`vuex`中`state`属性的位置，

​	辅助函数 `mapMutations`

```JavaScript
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
    state: {
        count: 0
    },
    mutations: {
        increment (state) {
            state.count++
        },
        incrementBy (state, payload) { // 提交有载荷
            state.count += payload.amount
        }
    }
});

import { mapState, mapMutations } from 'vuex';

new Vue({ 
    el: '#app',
    store,
    data: {
    },
    computed: mapState([
        'count'
    ]),
    methods: mapMutations([
        'increment',
        'incrementBy'
    ])
});
```

****

##### `actions`动作

​	唯一提交突变的位置

​	辅助函数`mapActions`

```javascript
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
    state: {
        count: 0
    },
    mutations: {
        increment (state) {
            state.count++
        },
        decrement (state) {
            state.count--
        }
    },
    actions: {
        //incrementAsync (context) {
        //    context.commit('increment')
        //}, =>
        incrementAsync ({ commit }) {
            setTimeout(() => {
                commit('increment')
            }, 1000)
        },
        actionA ({ commit }) {
            return new Promise((resolve, reject) => {
                setTimeout(() => {
                    commit('someMutation')
                    resolve()
                }, 1000)
            })
        },
        actionB ({ dispatch, commit }) {
            return dispatch('actionA').then(() => {
                commit('someOtherMutation')
            })
        },
        async actionC ({ commit }) {
            commit('gotData', await getData())
        },
        async actionD ({ dispatch, commit} ) {
            await dispatch('actionC')
            commit('gotOtherData', await getOtherData())
        }
    }
})

import { mapState, mapMutations } from 'vuex';

new Vue({ 
    el: '#app',
    store,
    data: {
    },
    computed: mapState([ 'count' ]),
    methods: {
        increment () {
            this.$store.dispatch('incrementAsync');
        },
        decrement () {
            this.$store.commit('decrement');
        },
        testAction () {
            this.$store.dispatch('actionA').then(() => {
                
            })
        }
    }
});

```

****

##### `modules ` 模块

​	将`store`划分为`module`，每个模块可以包含其自己的`state`，`mutation`，`action`，`getter`，甚至是嵌套`module`, 整个过程都是分形。

- 模块本地状态

  在模块本地变量和获取器内部，接收到第一个参数是模块本地状态，

  ```js
  const moduleA = {
    state: () => ({
      count: 0
    }),
    mutations: {
      increment (state) {
        // `state` is the local module state
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

  模块内部，`context.state`将公开本地状态，而根状态将公开为`context.rootState`:

  ```js
  const moduleA = {
    // ...
    actions: {
      incrementIfOddOnRootSum ({ state, commit, rootState }) {
        if ((state.count + rootState.count) % 2 === 1) {
          commit('increment')
        }
      }
    }
  }
  ```

  同样，在模块获取器内部，根状态将作为第三个参数公开：

  ```js
  const moduleA = {
    // ...
    getters: {
      sumWithRootCount (state, getters, rootState) {
        return state.count + rootState.count
      }
    }
  }
  ```

  

- 命名空间 `namespaced`

  默认情况下，动作和突变仍会在**全局名称空间**下注册，即多个模块对相同的动作/突变类型做出响应。默认情况下，`getter`也会在全局名称空间中注册，为了避免破坏更改，不要在不同的，没有命名空间的模块中定义两个具有相同名称的getter，会出错。

  模块重用，可以使用将其标记为命名空间`namespaced: true`，注册模块后，将根据模块在其上注册的路径自动为其所有的获取，操作和突变命名空间。

  ```js
  const store = createStore({
    modules: {
      account: {
        namespaced: true,
  
        // module assets
        state: () => ({ ... }), // module state is already nested and not affected by namespace option
        getters: {
          isAdmin () { ... } // -> getters['account/isAdmin']
        },
        actions: {
          login () { ... } // -> dispatch('account/login')
        },
        mutations: {
          login () { ... } // -> commit('account/login')
        },
  
        // nested modules
        modules: {
          // inherits the namespace from parent module
          myPage: {
            state: () => ({ ... }),
            getters: {
              profile () { ... } // -> getters['account/profile']
            }
          },
  
          // further nest the namespace
          posts: {
            namespaced: true,
  
            state: () => ({ ... }),
            getters: {
              popular () { ... } // -> getters['account/posts/popular']
            }
          }
        }
      }
    }
  })
  ```

  命名空间的`getter`和`action`将接受本地化`getters`，`dispatch`和`commit`。换句话说，您可以使用模块资产而无需在同一模块中写入前缀。是否在命名空间之间切换不会影响模块内部的代码。(?????)

  ****

- 命名空间模块访问全局资产

  如果要使用全局状态和getter，`rootState`和`rootGetters`作为第3和第4参数传递给getter函数，并且还作为`context`传递给动作函数的对象的属性公开。

  要在全局名称空间中分派操作或提交`{ root: true }`更改`dispatch`，请作为第三个参数传递给和`commit`。

  ```js
  modules: {
    foo: {
      namespaced: true,
  
      getters: {
        // `getters` is localized to this module's getters
        // you can use rootGetters via 4th argument of getters
        someGetter (state, getters, rootState, rootGetters) {
          getters.someOtherGetter // -> 'foo/someOtherGetter'
          rootGetters.someOtherGetter // -> 'someOtherGetter'
          rootGetters['bar/someOtherGetter'] // -> 'bar/someOtherGetter'
        },
        someOtherGetter: state => { ... }
      },
  
      actions: {
        // dispatch and commit are also localized for this module
        // they will accept `root` option for the root dispatch/commit
        someAction ({ dispatch, commit, getters, rootGetters }) {
          getters.someGetter // -> 'foo/someGetter'
          rootGetters.someGetter // -> 'someGetter'
          rootGetters['bar/someGetter'] // -> 'bar/someGetter'
  
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

  ****

- 在命名空间模块中注册全局操作

  如果要在命名空间模块中注册全局操作，则可以用标记它，`root: true`并将操作定义放置为`function handler`。例如：

  ```js
  {
    actions: {
      someOtherAction ({dispatch}) {
        dispatch('someAction')
      }
    },
    modules: {
      foo: {
        namespaced: true,
  
        actions: {
          someAction: {
            root: true,
            handler (namespacedContext, payload) { ... } // -> 'someAction'
          }
        }
      }
    }
  }
  ```

  ****

- 命名空间绑定辅助函数

  当绑定一个命名空间的模块将组件与`mapState`，`mapGetters`，`mapActions`和`mapMutations`辅助函数，它可以得到一个有点冗长：

  ```js
  computed: {
    ...mapState({
      a: state => state.some.nested.module.a,
      b: state => state.some.nested.module.b
    }),
    ...mapGetters([
      'some/nested/module/someGetter', // -> this['some/nested/module/someGetter']
      'some/nested/module/someOtherGetter', // -> this['some/nested/module/someOtherGetter']
    ])
  },
  methods: {
    ...mapActions([
      'some/nested/module/foo', // -> this['some/nested/module/foo']()
      'some/nested/module/bar' // -> this['some/nested/module/bar']()
    ])
  }
  ```

  在这种情况下，您可以将模块名称空间字符串作为第一个参数传递给辅助函数，以便所有绑定都使用该模块作为上下文来完成。上面可以简化为：

  ```js
  computed: {
    ...mapState('some/nested/module', {
      a: state => state.a,
      b: state => state.b
    }),
    ...mapGetters('some/nested/module', [
      'someGetter', // -> this.someGetter
      'someOtherGetter', // -> this.someOtherGetter
    ])
  },
  methods: {
    ...mapActions('some/nested/module', [
      'foo', // -> this.foo()
      'bar' // -> this.bar()
    ])
  }
  ```

  此外，您可以使用来创建命名空间的`Helpers` 辅助器,`createNamespacedHelpers`。它返回一个对象，该对象具有与给定名称空间值绑定的新的组件绑定辅助器：

  ```js
  import { createNamespacedHelpers } from 'vuex'
  
  const { mapState, mapActions } = createNamespacedHelpers('some/nested/module')
  
  export default {
    computed: {
      // look up in `some/nested/module`
      ...mapState({
        a: state => state.a,
        b: state => state.b
      })
    },
    methods: {
      // look up in `some/nested/module`
      ...mapActions([
        'foo',
        'bar'
      ])
    }
  ```

  

- 表格处理：

  例子：在严格模式下使用Vuex时，在`v-model`属于Vuex的状态下使用可能会有些棘手

  ```html
  <input v-model="obj.message">
  ```

  假设`obj`是一个计算属性，该属性从商店返回一个`Object`，则当用户键入输入内容时，`v-model`此处将尝试直接进行更改`obj.message`。在严格模式下，这将导致错误，因为未在显式的`Vuex`变异处理程序内执行变异。

  方案：处理它的“ Vuex方法”是绑定`<input>`的值并在`input`或`change`事件上调用方法：

  ```html
  <input :value="message" @input="updateMessage">
  ```

  ```js
  computed: {
    ...mapState({
      message: state => state.obj.message
    })
  },
  methods: {
    updateMessage (e) {
      this.$store.commit('updateMessage', e.target.value)
    }
  }
  
  
  
  mutations: {
    updateMessage (state, message) {
      state.obj.message = message
    }
  }
  ```

- 双向计算属性

  诚然，上面的内容比`v-model`+ +局部状态要冗长得多，因此我们也失去了一些有用的功能`v-model`。一种替代方法是使用带有setter的双向计算属性：

  ```html
  <input v-model="message">
  ```

  ```javascript
  computed: {
    message: {
      get () {
        return this.$store.state.obj.message
      },
      set (value) {
        this.$store.commit('updateMessage', value)
      }
    }
  }
  ```

  

