#### 事件委托是什么？有什么好处？
* 假设父元素有一堆儿子，我不监听儿子们，而是监听父亲，看触发事件的是哪个儿子，这就是事件委托。
* 还可以监听还没有出生的儿子（动态生成的元素节点），省监听器。
事件委托嘛，随手就来一个
```
function listen(element, eventType, selector, fn) {
  const el = document.querySelector(element)
  el.addEventListener(eventType, e => {
    if (e.target.matches(selector)) {
      fn.call(e.target, e)
    }
  })
}
```
相信大部分写出的代码都是这样子的把，

本着能用就好，

应付面试官也够用，

就这么将就把，

但是这串代码却是存在 `BUG` 的。

再让我们继续来看看一个工资 12k+ 的前端写的时间委托代码
```
function listen(element, eventType, selector, fn) {
  const f_element = document.querySelector(element)
  f_element.addEventListener(eventType, e => {
    let el = e.target
    while (!el.matches(selector)) {
      if (f_element === el) {
        el = null
        break
      }
      el = el.parentNode
    }
    el && fn.call(el, e)
  })
  return f_element
}
```

最后，拿去轻松愉快撸代码把~