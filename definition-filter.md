# 自定义过滤器

## 基础

和自定义指令类似，你可以用全局方法 `Vue.filter()`，传递一个**过滤器 ID** 和一个**过滤器函数**来注册一个自定义过滤器。过滤器函数会接受一个参数值并返回将其转换后的值：

```
Vue.filter('reverse', function (value) {
  return value.split('').reverse().join('')
})
```


```
<!-- 'abc' => 'cba' -->
<span v-text="message | reverse"></span>
```

过滤器函数也可以接受内联参数：

```
Vue.filter('wrap', function (value, begin, end) {
  return begin + value + end
})
```

```
<!-- 'hello' => 'before hello after' -->
<span v-text="message | wrap 'before' 'after'"></span>
```

## 双向过滤器

到目前为止，我们使用过滤器都是把来自模型的值在显示到视图之前进行转换。其实我们也可以定义一个过滤器，在把来自视图的值（input 元素）在写回模型之前进行转换：

```
Vue.filter('check-email', {
  // 这里 read 可选，只是为了演示
  read: function (val) {
    return val
  },
  // write 函数会在数据写入到模型之前被调用
  write: function (val, oldVal) {
    return isEmail(val) ? val : oldVal
  }
})
```

## 动态参数

如果一个过滤器参数没有被引号包裹，它会在当前 `vm` 的数据作用域里当做表达式进行动态求值。此外，过滤器函数的 `this` 上下文永远是调用它的当前 `vm`。

```
<input v-model="userInput">
<span>{{msg | concat userInput}}</span>```

```Vue.filter('concat', function (value, input) {
  // 这里 `input` === `this.userInput`
  return value + input
})
```

在上面这个例子中，显然用内联表达式也可以达成相同的效果。但是面对更复杂的需求时，常常需要不止一个语句，这种情况下你就得把逻辑放到一个计算属性中或是一个自定义过滤器中。

内建的 `filterBy` 和 `orderBy` 过滤器都是根据当前 Vue 实例的状态对传入的数组进行处理。

很好！现在，是时候了解 Vue 的核心概念：[组件系统](system.md)了。

