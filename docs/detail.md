# 细节

为了尽量模仿出和小程序相似的环境，会对一些细节表现作一些调整。

## class 前缀化

小程序中为了达到类似 web components 的效果，对于自定义组件中的 class 会进行前缀化处理，用于实现样式隔离。为了支持这个效果，在调用 [load](./api.md#loadcomponentpath-tagname--loaddefinition) 接口时传入 tagName 参数，这样在渲染自定义组件时会使用 tagName 作为 class 的前缀。默认 tagName 为 main，即前缀为 main。

假设自定义组件 comp 的模板为：

```html
<view class="abc">123</view>
```

```js
simulate.load('/comp') // 渲染出来的结果是 <comp><wx-view class="main--abc">123</wx-view></comp>

simulate.load('/comp', 'custom-comp') // 渲染出来的结果是 <comp><wx-view class="custom-comp--abc">123</wx-view></comp>
```

需要注意的是，当自定义组件里在 usingComponents 里引用了其他自定义组件的时候，是会默认声明 tagName，所以这种情况下都会进行 class 前缀化：

```json
{
    "component": true,
    "usingComponents": {
        "other-comp": "./other"
    }
}
```

这里的 other 组件在被渲染时就默认会以 other-comp 为 tagName，other 组件内的 class 就会被前缀化，加上前缀 other-comp。