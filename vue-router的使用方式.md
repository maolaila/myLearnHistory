1.使用方式
``` 
import Vue from 'vue';
import VueRouter from 'vue-router';
//主体
import App from './components/app.vue';
import Home from './components/home.vue'
//安装插件
Vue.use(VueRouter); //挂载属性
//创建路由对象并配置路由规则
let router = new VueRouter({
    routes: [
        //一个个对象
        { path: '/home', component: Home }
    ]
});
//new Vue 启动
new Vue({
    el: '#app',
    //让vue知道我们的路由规则
    router: router, //可以简写router
    render: c => c(App),
})
```


``` 
//app.vue中
<template>
    <div>
        <!-- 留坑，非常重要 -->
        <router-view></router-view>
    </div>
</template>
<script>
    export default {
        data(){
            return {}
        }
    }
</script>

```

2.vue-router参数传递

  1.用name传递参数
    在路由文件src/router/index.js里配置name属性
	
``` 
routes: [
    {
      path: '/',
      name: 'Hello',
      component: Hello
    }
]
```
2.通过<router-link> 标签中的to传参

``` 
  <router-link :to="{name:'hi1',params:{username:'jspang',id:'555'}}">Hi页面1</router-link>
```
然后把src/router/index.js文件里给hi1配置的路由起个name,就叫hi1.


``` 
{path:'/hi1',name:'hi1',component:Hi1}
```
3.利用url传递参数----在配置文件里以冒号的形式设置参数。

``` 
{
    path:'/params/:newsId/:newsTitle',
    component:Params
}
```
4.使用path来匹配路由，然后通过query来传递参数

``` 
<router-link :to="{ name:'Query',query: { queryId:  status }}" >
     router-link跳转Query
</router-link>
```
我们可以获取参数：
``` 
this.$route.query.queryId
```


二、vue-router配置子路由(二级路由)

修改router/index.js代码，子路由的写法是在原有的路由配置下加入children字段


``` 
 routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld,
      children: [{path: '/h1', name: 'H1', component: H1},//子路由的<router-view>必须在HelloWorld.vue中出现
        {path: '/h2', name: 'H2', component: H2}
      ]
    }
  ]

```
三、单页面多路由区域操作
1.App.vue文件，在<router-view>下面新写了两行<router-view>标签,并加入了些CSS样式


``` 
<template>
  <div id="app">
    <img src="./assets/logo.png">
       <router-link :to="{name:'HelloWorld'}"><h1>H1</h1></router-link>
       <router-link :to="{name:'H1'}"><h1>H2</h1></router-link>
    <router-view></router-view>
    <router-view name="left" style="float:left;width:50%;background-color:#ccc;height:300px;"/>
    <router-view name="right" style="float:right;width:50%;background-color:yellowgreen;height:300px;"/>
  </div>
</template>
```

2.需要在路由里配置这三个区域，配置主要是在components字段里进行

``` 
export default new Router({
    routes: [
      {
        path: '/',
        name: 'HelloWorld',
        components: {default: HelloWorld,
          left:H1,//显示H1组件内容'I am H1 page,Welcome to H1'
          right:H2//显示H2组件内容'I am H2 page,Welcome to H2'
        }
      },
      {
        path: '/h1',
        name: 'H1',
        components: {default: HelloWorld,
          left:H2,//显示H2组件内容
          right:H1//显示H1组件内容
        }
      }
    ]
  })

```



 