# progress-catalog

这个插件根据选定的目录内容中的 `h1, h2, h3, h4, h5, h6` 标签来自动生成目录插入到选定的目录容器中，并且提供一个漂亮的样式效果

- 监听内容区滚动
- 点击跳转功能

兼容性：IE10+ (由于使用了 `node.classList`)

欢迎提issue，提pr~

## Preview
炫酷模式：

![preview](https://github.com/SHERlocked93/progress-catalog/blob/master/assets/progress-catalog.gif)

普通模式：

![preview](https://github.com/SHERlocked93/progress-catalog/blob/master/assets/simple.gif)

可以通过 [线上DEMO](http://sherlocked93.club/vue-style-codebase/) 来预览一下炫酷模式的效果

## 2. 实现思路

滚动的监听通过 [getBoundingClientRect](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect) 获取元素大小以及相对视口的位置，判断我们的监听对象 `h1~h6` 标签是否在视口中，如果在则添加 `linkActiveClass`  类。

点击目录项的定位跳转在hash模式的单页面应用中会与传统的锚点定位冲突，会导航到错误的路由路径，这里采用把要跳转的id放到 [dataset](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset) 中，跳转的时候取出来使用 [scrollIntoView](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/scrollIntoView) 来进行平滑滚动到目标位置。

左边的边栏线则是使用 [svg](http://www.ruanyifeng.com/blog/2018/08/svg.html) 的path来画出来的，根据层级相应做一些调整，辅以css的 `transition` 的效果就可以呈现出不错的移动效果。

## Build Setup

``` bash
# npm安装包依赖
npm install

# 使用babel编译
npm run build
```


## Api
如果要使用默认的样式，请手动引入

```js
import 'progress-catalog/src/progress-catalog.css'
```

使用方法：
```js
// 引入
import Catalog from 'ProgressCatalog'

// 使用 
new Catalog({
	contentEl: 'loading-animation',
	catalogEl: `catalog-content-wrapper`,
	selector: ['h2', 'h3']
})
```

构造函数还有一些其他参数：

#### contentEl [String]
需要检索生成目录的内容区的id选择器，不需要加#

#### catalogEl [String]
将生成的目录append进的目录容器的id选择器，不需要加#

#### scrollWrapper [可选, String]
监听scroll事件的内容区容器的id选择器，不需要加#，如果不填则默认是 `contentEl` 的父元素

#### linkClass [可选, String]
所有目录项都有的类，默认值：`cl-link`

#### selector [可选, Array]
选择目录的标题标签，默认值：`['h1', 'h2', 'h3', 'h4', 'h5', 'h6']`

如果只希望生成目标内容区的 h2, h3 标签的目录，那么可以设置 `selector: ['h2', 'h3']`

#### activeHook [可选, Function]
当激活新的目录项标签的时候的回调函数

#### topMargin [可选, Number]
第一个目录标签在被认为可见之前需要向下移动的距离，默认值：`0`

#### bottomMargin [可选, Number]
同上，向下移动的距离，默认值：`0`

#### cool [可选, Boolean]
炫酷模式开关，默认值：`true`
