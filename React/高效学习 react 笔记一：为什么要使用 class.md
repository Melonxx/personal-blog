## 组件
如果我们写一个功能复杂的页面，都写在 js 里，那么你的文件将会变成[下面这个样子](https://jsbin.com/kenayey/edit?html,js,output)：

![](https://upload-images.jianshu.io/upload_images/5780538-40fd364a308f4c0e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


`render` 函数里面将会有一坨的代码，非常不优雅
但是此时就有一个奇怪的现象，为什么同样的 `html` 代码写在 `html` 文件里面就不会觉得丑呢？
因为国际惯例认为习惯， `js` 文件一般不会这么复杂，所以产生了组件的概念。

我们把[代码分开](https://jsbin.com/zenowac/edit?html,js,output)（为做演示，先暂时放在一个文件里面）
![](https://upload-images.jianshu.io/upload_images/5780538-91151c4b533e0af1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是此时遇到一个 `bug` 当我点击 `➕1` 的时候，两个组件同时加了1：

![](https://upload-images.jianshu.io/upload_images/5780538-b2abf4488ace5af2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

让我们来整理一下逻辑：点击 `➕1` 以后，变量 `number` 加1，接着执行 `render` 函数，`box1` 和 `box2` 同时重新渲染，因为用的同一个变量 `number` 所以结果就是两个组件同时加上了1。

所以我们[试试](https://jsbin.com/nagofet/edit?html,js,output)给他们两个组件各自的变量和函数。

![](https://upload-images.jianshu.io/upload_images/5780538-17f9acfd3d62da63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

虽然解决了 “bug”， 但是变成了这个样子，代码已经惨不忍睹了（全局变量太多了）。

此时能不能用**函数传参**试试呢，在 `react` 里面也就是 `props`，
让我们先试试用[函数传参](https://jsbin.com/yelivix/edit?html,js,output)把~
我们给这两个组件各自的名字

![](https://upload-images.jianshu.io/upload_images/5780538-a12144b4871cc3d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过函数传参，我们分别给这两个组件各自的名字，
这也就是 `react` 另一个聪明之处，**他把属性理解为对象的key和value**，与js的对象完美对应起来了，
既然可以传 `n ame` 了，那我们能不能现在传 `number` 呢？

> 答案是不行。因为 `react` 规定，永远不要试图去改变别人船费你的 `props`。 

但是这该怎么办呢？我们怎么优雅的给这两个组件各自的 `number` 呢？

## Class

> js 里面有什么东西又能满足函数的功能，又可以有自己的作用域、变量 呢

###**于是这里就用上了 `class`**

试图用 `class` [改写](https://jsbin.com/yelivix/edit?html,js,output)刚刚的代码

![](https://upload-images.jianshu.io/upload_images/5780538-78791d189362e09a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> `react` 规定，变量**必须**放在 `this.state` 里面，更改变量**必须**通过 `this.setState` 方法，否则无法自动更新dom。 

> **注意** `this.setState` 有可能是异步的，所以你无法像这样写 `➕2` 的代码
> ```
> clickAdd(){
>     this.setState({
>       number: this.state.number + 1
>     })
>     this.setState({
>       number: this.state.number + 1
>     })
>   }
> ```
> 此时你应当使用 `this.setState` 的回调函数形式
> ```
> this.setState({
>       number: this.state.number + 1
>     })
>     this.setState((state) =>{
>       return {
>         number: state.number + 1
>       }
>     })
> ```

本文完。
