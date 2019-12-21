#首屏加速——CDN

####先了解一下，什么是CDN？（了解即可）

     __全称__ :Content Delivery Network或Content Ddistribute Network，即内容分发网络，
      __目的：__ 解决因分布、带宽、服务器性能带来的访问延迟问题，适用于站点加速、点播、直播等场景。使用户可就近取得所需内容，解决 Internet网络拥挤的状况，提高用户访问网站的响应速度和成功率。
####在项目中CDN的原理（也是自己了解）~

        作为一个网站应用，加载速度是非常重要的。加载速度，一个是程序的合理安排，如以组件按需加载，一个是js、css等资源的异步加载。
        在Vue项目中，引入到工程中的所有js、css文件，编译时都会被打包进vendor.js，浏览器在加载该文件之后才能开始显示首屏。若是引入的库众多，那么vendor.js文件体积将会相当的大，影响首开的体验。
        解决方法是，将引用的外部js、css文件剥离开来，不编译到vendor.js中，而是用资源的形式引用，这样浏览器可以使用多个线程异步将vendor.js、外部的js等加载下来，达到加速首开的目的。
        外部的库文件，可以使用CDN资源，或者别的服务器资源等。
#常用的使用方式（****）

     __一、资源引入__ 
          __在index.html中，添加CDN资源，例如：__ 

>&lt;script src="https://cdn.bootcss.com/vue/2.5.2/vue.min.js"&gt;&lt;/script&gt;
>&lt;script src="https://cdn.bootcss.com/vue-router/3.0.1/vue-router.min.js"&gt;&lt;/script&gt;
>&lt;script src="https://cdn.bootcss.com/vuex/3.0.1/vuex.min.js"&gt;&lt;/script&gt;

     __二、添加配置__ 
         __在bulid/webpack.base.conf.js文件中，增加externals，将引用的外部模块导入，如下：__ 

>module.exports = {
>   entry: {
>            app: './src/main.js'
>   },
>   externals:{
>            'vue': 'Vue',
>            'vue-router': 'VueRouter',
>            'vuex':'Vuex'
>       }

         __注意一点：__ 
        格式为 'aaa' : 'bbb', 其中，aaa表示要引入的资源的名字，bbb表示该模块提供给外部引用的名字，由对应的库自定。例如，vue为Vue，vue-router为VueRouter.
     __三、去掉原有的引用__ 
         __去掉import，如：__ 
        // import Vue from 'vue'
        // import Router from 'vue-router'
        去掉Vue.use(XXX)，如：
        // Vue.use(Router)
#首屏加速——路由懒加载

    路由懒加载，例：


![](C:\Users\dell\Desktop\学习文档\vue首屏加速-路由懒加载.png)




    组件懒加载，在vue实例中组件引入的时候添加。例：


![](C:\Users\dell\Desktop\学习文档\vue首屏加速-组件懒加载.png)



    全局懒加载，在项目中的mian.js中添加。例：


![](C:\Users\dell\Desktop\学习文档\vue首屏加速-全局懒加载.png)




#首屏加速——图片懒加载

     __VUE图片懒加载-vue lazyload插件的简单使用__ 
####一. vue lazyload插件：


>插件地址：https://github.com/hilongjw/vue-lazyload

####二.使用实例

    1.安装

>npm install vue-lazyload --save-dev

    2.main.js 引入插件

>import VueLazyLoad from 'vue-lazyload'
>  Vue.use(VueLazyLoad,{
>      error:require('./statics/site/imgs/erro.jpg'),
>      loading:require('./statics/site/imgs/load.gif')
>  })
>// 这里还需要加上一个 require

    3.Vue文件中将需要懒加载的图片绑定 v-bind:src 修改为 v-lazy

>&lt;img class="item-pic" v-lazy="newItem.picUrl"/&gt;

####三.功能扩展

         __图片懒加载的简单效果已经实现了，然后就可以按这开发文档的api进行扩展了：__ 

![](C:\Users\dell\Desktop\学习文档\vue首屏加速-功能扩展api文档.png)

