# 扩展 Vue

## Mixins

Mixin (混入) 是一种可以在多个 Vue 组件之间灵活复用特性的机制。你可以像写一个普通 Vue 组件的选项对象一样编写一个 mixin：

```// mixin.js
module.exports = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}```

```
// test.js
var myMixin = require('./mixin')
var Component = Vue.extend({
  mixins: [myMixin]
})
var component = new Component() // -> "hello from mixin!"```

更多细节请参见 **API**。

## 使用插件进行扩展

通常插件会为 Vue 添加一个全局的功能。

### 撰写插件

你可以撰写以下几种典型类型的插件：

1. 添加一个或几个全局方法。比如 `vue-element`
2. 添加一个或几个全局资源：指令、过滤器、动画效果等。比如 `vue-touch`
3. 通过绑定到 `Vue.prototype` 的方式添加一些 Vue 实例方法。这里有个约定，就是 Vue 的实例方法应该带有 `$` 前缀，这样就不会和用户的数据和方法产生冲突了。

```
exports.install = function (Vue, options) {
  Vue.myGlobalMethod = ...          // 1
  Vue.directive('my-directive', {}) // 2
  Vue.prototype.$myMethod = ...     // 3
}
```

### 使用插件

假设我们使用的构建系统是 `CommonJS`，则需要作如下调用：

```
var vueTouch = require('vue-touch')
// use the plugin globally
Vue.use(vueTouch)
```

你也可以向插件里传递额外的选项：

```Vue.use(require('my-plugin'), {
  /* pass in additional options */
})```

## 现有的插件 & 工具

- `vue-resource`： 一个插件，为用 `XMLHttpRequest` 或 `JSONP` 生成网络请求、响应提供服务。
- `vue-validator`： 一个表单验证的插件。
- `vue-devtools`：一个用来调试 Vue.js 应用程序的 Chrome 浏览器开发者工具扩展。
- `vue-touch`：：添加基于 Hammer.js 的触摸手势的指令。
- `vue-element`： 用 Vue.js 注册 `Custom Elements`。
- 用户贡献的工具列表

下一节: [最佳实践与技巧](http://cn.vuejs.org/guide/best-practices.html)。