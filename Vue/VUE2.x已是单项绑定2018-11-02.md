普通使用v-model
```
<input v-model="searchText">
```
等价于：
```
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```
当用在组件上时，v-model 则会这样：
```
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
```
为了让它正常工作，这个组件内的 `<input>` 必须：

将其 value 特性绑定到一个名叫 value 的 prop 上
在其 input 事件被触发时，将新的值通过自定义的 input 事件抛出
写成代码之后是这样的：
```
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
```
现在 v-model 就应该可以在这个组件上完美地工作起来了：
```
<custom-input v-model="searchText"></custom-input>
```