- ###1.  任意两个组件之间如何通信
- ###2. 发布订阅模式
- ###3. Redux 就那么回事

[上篇文章](https://www.jianshu.com/p/5c22d3908b41)说到了组件传值可以通过 `props` 来传值，

但是当我们遇到**嵌套组件很深的时候或者任意组件通讯的时候**，

比如说[这个时候](http://js.jirengu.com/keveb/4/edit?js,output)

![](https://upload-images.jianshu.io/upload_images/5780538-8973a6ec2bf8e645.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 当我们点击 `minus` 的时候只有当前组件的数据在变化，但是我们希望这个数据在变化的时候通知其他组件同时变化，所以，如果继续用 `props` 父子组件传递的思路的话，将会是下面这个样子。

![](https://upload-images.jianshu.io/upload_images/5780538-1e33b53c0fab62f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 蓝色箭头表明了数据流向，这种层层传递非常麻烦，而且很容易出现错误。

##### 所以 `eventHub` 就派上了用场。

正因为 `React` 是单向数据流，所以我们只能通过 `eventHub` 来传递数据

直接上手写一个 `eventHub` （即发布订阅模式）
```
// （暂时忽略一些细节）
const eventHub = {
  data: {},
  eventLists: {},
  on(name, fn) {
    (this.eventLists[name]
      ? this.eventLists[name]
      : (this.eventLists[name] = [])
    ).push(fn);
  },
  off(name, fn) {
    const index = this.eventLists[name].findIndex(v => v === fn);
    this.eventLists[name].splice(index, 1);
    if (this.eventLists[name].length === 0) {
      delete this.eventLists[name];
    }
  },
  trigger(name, ...arg) {
    this.eventLists[name].map(fn => fn.call(null, ...arg));
  }
};
``` 

利用这个简陋的 `eventHub` 我们就可以[实现](http://js.jirengu.com/jexujedopa/4/edit?js,output)任意组件的通讯

![](https://upload-images.jianshu.io/upload_images/5780538-5f4cc314042a39aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 可以看到在这个例子中，我们的数据还是通过 `props` 传递，但是我们在最上面已经提前发布了事件，之后只需要在组件中调用即可，从中还是遵循着单项数据流的原则。

好了，我们已经用自己的 `eventHub` 来实现了任意组件的通讯，



