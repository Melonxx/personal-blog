###1.javascript无动态作用域链
栗子如下：
```swift
var a = 1;
function s() {
	var a = 3;
	x();
}
function x() {
	console.log(a);
}
s(); //a = 1
```
词法作用域让foo()中的a通过RHS引用到了全局作用域中的a，因此会输出2；而动态作用域并不关心函数和作用域是如何声明以及在何处声明的，只关心它们从何处调用。换句话说，作用域是基于调用栈的，而不是代码中的作用域嵌套；JAVASCRIPT不具有动态作用域（但this机制在某种程度上很像动态作用域的）。
### 2.this指向问题
栗子如下：
```swift
function a() {
    function c() {
        return this;
     }
     return c();
}
var o = new a();
```
此时上面的this指向是全局对象还是对象o？；
### 3.重绘（Repaint）和回流（Reflow）
重绘和回流是渲染步骤中的一小节，但是这两个步骤对于性能影响很大。

- 重绘是当节点需要更改外观而不会影响布局的，比如改变 color 就叫称为重绘
- 回流是布局或者几何属性需要改变就称为回流。

回流必定会发生重绘，重绘不一定会引发回流。回流所需的成本比重绘高的多，改变深层次的节点很可能导致父节点的一系列回流。

所以以下几个动作可能会导致性能问题：

- 改变 window 大小
- 改变字体
- 添加或删除样式
- 文字改变
- 定位或者浮动
- 盒模型

**减少重绘和回流**
- 1. 使用 translate 替代 top
```
<div class="test"></div>
<style>
    .test {
        position: absolute;
        top: 10px;
        width: 100px;
        height: 100px;
        background: red;
    }
</style>
<script>
    setTimeout(() => {
        // 引起回流
        document.querySelector('.test').style.top = '100px'
    }, 1000)
</script>
```
- 2. 使用 `visibility` 替换 `display: none` ，因为前者只会引起重绘，后者会引发回流（改变了布局）

- 3. 把 DOM 离线后修改，比如：先把 DOM 给 `display:none` (有一次 Reflow)，然后你修改100次，然后再把它显示出来

- 4. 不要把 DOM 结点的属性值放在一个循环里当成循环里的变量
```
for(let i = 0; i < 1000; i++) {
    // 获取 offsetTop 会导致回流，因为需要去获取正确的值
    console.log(document.querySelector('.test').style.offsetTop)
}
```
- 5. 不要使用 table 布局，可能很小的一个小改动会造成整个 table 的重新布局

- 6. 动画实现的速度的选择，动画速度越快，回流次数越多，也可以选择使用 requestAnimationFrame

- 7. CSS 选择符从右往左匹配查找，避免 DOM 深度过深

- 8. 将频繁运行的动画变为图层，图层能够阻止该节点回流影响别的元素。比如对于 video 标签，浏览器会自动将该节点变为图层。

![](https://upload-images.jianshu.io/upload_images/5780538-bfd2cd5fdb8c7423.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

