 !18 __uniapp生命周期__! 
onLaunch 当uniapp初始化完成时进行触发，全局只触发一次
onShow 当uniapp启动或者从后台进入前台显示
onHide 当uniapp从前台进入后台
onUniNViewMessage 对nvue页面发送数据进行监听
onError 当 uni-app 报错时触发


  __!18 页面生命周期!__ 

跟小程序的几乎无差异

 !18 __路由__! 

uni-app 有两种路由跳转方式：使用navigator组件跳转、调用API跳转。
1.打开新页面，页面重定向===》 调用 API uni.navigateTo 、使用组件 <navigator open-type="navigateTo"/>
2.页面返回 调用 API uni.navigateBack 、使用组件 <navigator open-type="navigateBack"/> 、用户按左上角返回按钮、安卓用户点击物理back按键

3.Tab 切换 调用 API uni.switchTab 、使用组件 <navigator open-type="switchTab"/> 、用户切换 Tab

4.重加载 调用 API uni.reLaunch 、使用组件 <navigator open-type="reLaunch"/>


页面样式与布局

uni-app 支持的通用 css 单位包括 px、rpx
 !18 __样式引入__! 

``` 
<style>
    @import "../../common/uni.css";

    .uni-card {
        box-shadow: none;
    }
</style>
```

为支持跨平台，框架建议使用Flex布局


 __!18 字体图标!__ 

uni-app 支持使用字体图标，使用方式与普通 web 项目相同