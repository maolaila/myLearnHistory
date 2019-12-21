vuex 和 vuex 本地持久化
一、新建store

``` 
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex);

const config = {
    // 定义状态
    state: {
        isLogin: false
    },

    // getters
    getters: {
        // isLogin:(state) =>{
        //     return state.isLogin;
        // },
        // 等同上面的写法
        isLogin: state => state.isLogin
    },

    // 修改state里面的变量
    mutations: {
        // state指向上面的state,payload是调用muation时传过来的参数
        updateLogin(state, payload) {
            state.isLogin = payload;
        }
    },

    // action行为
    actions: {

    },

    // module
    modules: {

    }
}
export default new Vuex.Store(config);
```

二。在main.js导入并挂载到vue的实例上

``` 
import Vue from 'vue'
import App from './App.vue'
import store from './store/index'

Vue.config.productionTip = false

new Vue({
  store,
  render: h => h(App),
}).$mount('#app')

```
三、获取在vuex定义的状态
通过this.$store.state.xxx 来取,具体使用


``` 
created() {
    console.log(this.$store.state.isLogin);
    console.log(this.$store.state.firstName);
}

// 通常我们会定义计算属性来取值,比如
computed: {
    // 自定义计算属性
    isLogin() {
      // 获取vuex的isLogin的值
      return this.$store.state.isLogin
    }
}
```
四、修改state中的状态
1.定义state和mutation


``` 
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)

export default new Vuex.Store({
    state: {
        name: "没名字",
        count: 1
    },
    getters: {

    },
    // 修改state里的值必须通过mutation来修改
    mutations: {
        /**
         * 定义一个修改name的mutation
         * state是上面的定义的state
         * payload是新的数据
         */
        updateName(state, payload) {
            state.name = payload;
        }
    }
})

```

2.在需要的时候调用mutation进行修改state里的name状态
this.$store.commit('updateName','老胡');

五、通过vuex实现加减
1.在vuex里配置state和mutation

``` 
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)

export default new Vuex.Store({
    state: {
        count: 0
    },

    mutations: {
        addOne(state, payload) {
            state.count = state.count + 1;
        },
        minusOne(state, payload) {
            if (state.count > 0) {
                state.count = state.count - 1;
            }
        }
    }
})

```
2.index.vue的配置

``` 
<template>
  <div>
    <button @click="minus">-</button>
    <span>{{count}}</span>
    <button @click="add">+</button>
  </div>
</template>

<script>
import { mapState } from "vuex";
export default {
  computed: mapState({
    count: "count"
  }),

  methods: {
    add() {
      this.$store.commit("addOne");
    },
    minus() {
      this.$store.commit("minusOne");
    }
  }
};
</script>
```

六、vuex 本地持久化

1.安装 vuex-persistedstate

``` 
npm i vuex-persistedstate
```
2.在vuex中,添加plugins

``` 
 import createPersistedState from 'vuex-persistedstate'
 
 plugins: [createPersistedState()],
```

3.store修改

``` 
import Vue from 'vue';
import Vuex from 'vuex';
import createPersistedState from 'vuex-persistedstate'
// 导入模块
import login from './module/login'
import my from './module/my'
Vue.use(Vuex);
export default new Vuex({
    plugins: [createPersistedState()],
    // 模块
    modules: {
      login,
      my
    },
    state: {
        isLogin: false,
        username: '',
        token: ''
    },
    getters: {
        isLogin: state => state.isLogin,
        token: state => state.token,
        username: state => state.username
    },
    mutations: {
        updateLogin(state, payload) {
            state.isLogin = payload;
        },
        updateToken(state, payload) {
            state.token = payload;
        },
        updateUsername(state, payload) {
            state.username = payload;
        }
    },
    actions: {
      LoginAction({commit}, payload) {
        commit('updateLogin',payload)
      },
      TokenAction({commit}, payload) {
        commit('updateToken',payload)
      },
      UsernameAction({commit}, payload) {
        commit('updateUsername',payload)
      },
    }
})

```