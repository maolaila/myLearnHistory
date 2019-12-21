 `uni-app`  App端内置 [weex]渲染引擎，提供了原生渲染能力。
 !18 项目渲染模式! 


```
// manifest.json    
{    
    // ...    
     /* App平台特有配置 */    
    "app-plus": {    
        "renderer": "native", //App端纯原生渲染模式
    }    
}
```

```
// manifest.json    
{    
    // ...    
     /* App平台特有配置 */    
    "app-plus": {    
        "nvueCompiler":"uni-app" //是否启用 uni-app 模式  
    }    
}
```


###[1. 新建 nvue 页面](https://uniapp.dcloud.io/use-weex?id=_1-%e6%96%b0%e5%bb%ba-nvue-%e9%a1%b5%e9%9d%a2)

不管是vue页面还是nvue页面，都需要在pages.json中注册。如果在HBuilderX中新建页面是会自动注册的，如果使用其他编辑器，则需要自行在pages.json里注册。


###[2. 开发 nvue 页面](https://uniapp.dcloud.io/use-weex?id=_2-%e5%bc%80%e5%8f%91-nvue-%e9%a1%b5%e9%9d%a2)


```
&lt;template&gt;
    &lt;div class="example"&gt;
        &lt;div class="item" @click="changeNum"&gt;&lt;text class="text"&gt;点击文字，改变数字大小: {{num}}&lt;/text&gt;&lt;/div&gt;
        &lt;div class="item" @click="showModal('native')"&gt;&lt;text class="text"&gt;使用 weex 的 API 弹出模态框&lt;/text&gt;&lt;/div&gt;
        &lt;div class="item" @click="showModal('uni')"&gt;&lt;text class="text"&gt;使用 uni-app 的 API 弹出模态框&lt;/text&gt;&lt;/div&gt;
        &lt;div class="item" @click="showModal('plus')"&gt;&lt;text class="text"&gt;使用 plus 的 API 弹出模态框&lt;/text&gt;&lt;/div&gt;
    &lt;/div&gt;
&lt;/template&gt;
&lt;script&gt;
    export default {
        data() {return {num: 1}},
        created() {console.log('页面created')},
        methods: {
            changeNum() { this.num += 1; },
            showModal(type) {
                if (type === 'uni') {
                    uni.showModal({content: '使用 uni-app 的 API 弹出模态框'});
                }
                else if (type === 'plus') {
                    plus.nativeUI.alert('使用 uni-app 的 API 弹出模态框');
                } else {
                    var modal = weex.requireModule('modal')
                    modal.alert({message: '使用 weex 的 API 弹出模态框'});
                }
            }
        }
    }
&lt;/script&gt;
&lt;style&gt;
    .example {flex-direction: column;}
    .text {line-height: 80px;font-size: 32px;color: #333;}
    .item {height: 80px;width: 690px;margin: 30px;background-color: #f8f8f8;border-width: 1px;border-color: #eee;border-radius: 10px;align-items: center;}
&lt;/style&gt;
```
##[页面跳转](https://uniapp.dcloud.io/use-weex?id=%e9%a1%b5%e9%9d%a2%e8%b7%b3%e8%bd%ac)

nvue 的页面跳转，与 weex 不同，仍然遵循 uni-app 的路由模型。vue 页面和 nvue 页面之间不管怎么跳转，都遵循这个模型。包括 nvue 页面跳向 nvue 页面。
每个页面都需要在 pages.json 中注册，调用   `uni-app`  的 [路由 API](https://uniapp.dcloud.io/api/router) 进行跳转。

###[nvue 向 vue 通讯](https://uniapp.dcloud.io/use-weex?id=nvue-%e5%90%91-vue-%e9%80%9a%e8%ae%af)

1. 在  `nvue`  使用  `uni.postMessage(data)`  发送数据通讯， `data`  为  `JSON`  格式（键值对的值仅支持String）。
2. 在  `App.vue`  里使用  `onUniNViewMessage`  进行监听。


```
//test.nvue
&lt;template&gt;
    &lt;div @click="test"&gt;
        &lt;text&gt;点击页面发送数据&lt;/text&gt;
    &lt;/div&gt;
&lt;/template&gt;
&lt;script&gt;
    export default {
        methods: {
            test(e) {
                uni.postMessage({test: "数据",value:"数据"});
            }
        }
    }
&lt;/script&gt;
```

```
//App.vue
&lt;script&gt;
    export default {
        onUniNViewMessage:function(e){
          console.log("App.vue收到数据")
          console.log(JSON.stringify(e.data))  
        },
        onLaunch: function() {
            console.log('App Launch');
        }
    }
&lt;/script&gt;
```

###[vue 向 nvue 通讯](https://uniapp.dcloud.io/use-weex?id=vue-%e5%90%91-nvue-%e9%80%9a%e8%ae%af)



```
const globalEvent = weex.requireModule('globalEvent');
globalEvent.addEventListener("plusMessage", e =&gt; {
  console.log(e.data);//得到数据
});
```

```
//index.nvue
&lt;template&gt;
    &lt;div @click="test"&gt;
        &lt;text&gt;点击页面发送数据{{num}}&lt;/text&gt;
    &lt;/div&gt;
&lt;/template&gt;
&lt;script&gt;
    const globalEvent = weex.requireModule('globalEvent');
    export default {
        data() {
            return {
                num: "0"
            }
        },
        created() {
            globalEvent.addEventListener("plusMessage", e =&gt; {
                console.log(e.data);
                if (e.data.num) { //存在num时才赋值，在nvue里调用uni的API也会触发plusMessage事件，所以需要判断需要的数据是否存在
                    this.num = e.data.num
                }
            });
        },
        methods: {
            test(e) {
                uni.navigateTo({
                    url: '../test/test'
                })
            }
        }
    }
```

```
//test.vue
&lt;template&gt;
    &lt;view&gt;
        &lt;button type="primary" @click="test"&gt;点击改变nvue的数据&lt;/button&gt;
    &lt;/view&gt;
&lt;/template&gt;
&lt;script&gt;
    export default {
        methods: {
            test() {
                var pages = getCurrentPages();
                var page = pages[pages.length - 2];
                var currentWebview = page.$getAppWebview();
                plus.webview.postMessageToUniNView({
                    num: "123"
                }, currentWebview.id);
                uni.navigateBack()
            }
        }
    }
&lt;/script&gt;
```

##[vue 和 nvue 共享的变量和数据](https://uniapp.dcloud.io/use-weex?id=vue-%e5%92%8c-nvue-%e5%85%b1%e4%ba%ab%e7%9a%84%e5%8f%98%e9%87%8f%e5%92%8c%e6%95%b0%e6%8d%ae)

 __1. vuex:__ 

 __2. uni.storage:__ 

 __3. globalData:__ 


```
&lt;script&gt;  
    export default {  
        globalData: {  
            text: 'text'  
        },  
        onLaunch: function() {  
            console.log('App Launch')  
        },  
        onShow: function() {  
            console.log('App Show')  
        },  
        onHide: function() {  
            console.log('App Hide')  
        }  
    }  
&lt;/script&gt;
```


##[weex 编译模式中使用 weex 第三方库](https://uniapp.dcloud.io/use-weex?id=weex-%e7%bc%96%e8%af%91%e6%a8%a1%e5%bc%8f%e4%b8%ad%e4%bd%bf%e7%94%a8-weex-%e7%ac%ac%e4%b8%89%e6%96%b9%e5%ba%93)

 `npm init -y` 

 `npm i weex-ui -S` 


```
&lt;template&gt;
  &lt;div&gt;
      &lt;wxc-cell label="标题" title="Weex Ui" :has-arrow="true" @wxcCellClicked="wxcCellClicked" :has-margin="true"&gt;&lt;/wxc-cell&gt;
  &lt;/div&gt;
&lt;/template&gt;
&lt;script&gt;
  import { WxcCell } from 'weex-ui';
  export default {
    components: { WxcCell },
    methods: {
      wxcCellClicked (e) {
        console.log(e)
      }
    }
  };
&lt;/script&gt;
```




- nvue 页面只能使用 flex 布局，不支持其他布局方式。
- weex 
	下，页面内容高过屏幕高度并不会自动滚动，它没有页面滚动的概念，只有部分组件可滚动（list、waterfall、scroll-view/scroller），要滚得内容需要套在可滚动组件下。这不符合前端开发的习惯，所以在
	 nvue 编译为 uni-app模式时，给页面外层自动套了一个 
	scroller，页面内容过高会自动滚动。（组件不会套，页面有recycle-list是也不会套）。后续会提供配置，可以设置不自动套。
- weex 下，px是与屏幕宽度相关的动态单位，750px代表成屏幕宽度100%，它的静态单位是wx。在 nvue 编译为 uni-app模式时，纠正了这个问题，rpx是与屏幕宽度相关的动态单位，px是静态单位。
- 页面开发前，首先想清楚这个页面的纵向内容有什么，哪些是要滚动的，然后每个纵向内容的横轴排布有什么，按 flex 布局设计好界面。
- 文字内容，必须、只能在组件下。不能在、的text区域里直接写文字。否则即使渲染了，也无法绑定js里的变量。
- 支持的css有限，不过并不影响布局出你需要的界面，flex还是非常强大的。[详见](https://weex.apache.org/zh/docs/styles/common-styles.html#%E7%9B%92%E6%A8%A1%E5%9E%8B)
- 不支持背景图。但可以使用image组件和层级来实现类似web中的背景效果。因为原生开发本身也没有web这种背景图概念
- css选择器支持的比较少，没有web丰富。详见weex的样式文档
- class 进行绑定时只支持数组语法。
- nvue页面没有bounce回弹效果，只有几个列表组件有bounce效果，包括 list、recycle-list、waterfall。
- Android端在一个页面内使用大量圆角边框会造成性能问题，尤其是多个角的样式还不一样。应避免这类使用。