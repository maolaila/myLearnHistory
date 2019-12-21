#vue使用注意事项

- 不要在选项属性或回调上使用箭头函数，比如  `created: () =&gt; console.log(this.a)`  或  `vm.$watch('a', newValue =&gt; this.myMethod())` 。因为箭头函数是和父级上下文绑定在一起的， `this`  不会是如你做预期的  `Vue`  实例，且  `this.a`  或  `this.myMethod`  也会是未定义的。
- 建议使用  `uni-app`  的  `onReady` 代替  `vue`  的  `mounted` 。
- 建议使用  `uni-app`  的  `onLoad`  代替  `vue`  的  `created` 。


#data 属性

```
//正确用法，使用函数返回对象
data() {
    return {
        title: 'Hello'
    }
}
```


用  `computed`  方法生成  `class`  或者  `style`  字符串，插入到页面中


```
&lt;template&gt;
    &lt;!-- 支持 --&gt;
    &lt;view class="container" :class="computedClassStr"&gt;&lt;/view&gt;
    &lt;view class="container" :class="{active: isActive}"&gt;&lt;/view&gt;

    &lt;!-- 不支持 --&gt;
    &lt;view class="container" :class="computedClassObject"&gt;&lt;/view&gt;
&lt;/template&gt;
&lt;script&gt;
    export default {
        data () {
            return {
                isActive: true
            }
        },
        computed: {
            computedClassStr () {
                return this.isActive ? 'active' : ''
            },
            computedClassObject () {
                return { active: this.isActive }
            }
        }
    }
&lt;/script&gt;
```