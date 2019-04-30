首先，不管你采用哪种方式，最终实现的调用方式都是
```
vm.api()
```
也就是说，两种方法，实现的原理都是在 `Vue.prototype` 上添加了一个方法。所以结论是**“没有区别”**。

再来说说 `Vue.use()` 到底干了什么。

我们知道，`Vue.use()` 可以让我们安装一个自定义的Vue插件。为此，我们需要声明一个` install` 函数
```
// 创建一个简单的插件 say.js
var install = function(Vue) {
  if (install.installed) return // 如果已经注册过了，就跳过
  install.installed = true

  Object.defineProperties(Vue.prototype, {
    $say: {
      value: function() {console.log('I am a plugin')}
    }
  })
}
module.exports = install
```
然后我们要注册这个插件
```
import say from './say.js'
import Vue from 'vue'

Vue.use(say)
```

这样，在每个 `Vue` 的实例里我们都能调用 `say` 方法了。

我们来看 `Vue.use` 方法内部是怎么实现的
```
Vue.use = function (plugin) {
  if (plugin.installed) {
    return;
  }
  // additional parameters
  var args = toArray(arguments, 1);
  args.unshift(this);
  if (typeof plugin.install === 'function') {
    plugin.install.apply(plugin, args);
  } else {
    plugin.apply(null, args);
  }
  plugin.installed = true;
  return this;
};
```

其实也就是调用了这个 `install` 方法而已。
