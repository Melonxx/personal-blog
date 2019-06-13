在 ES3 和更早的版本中，JavaScript 本身还没有异步执行代码的能力，这也就意味着，宿主环境传递给 JavaScript 引擎一段代码，引擎就把代码直接顺次执行了，这个任务也就是宿主发起的任务。

但是，在 ES5 之后，JavaScript 引入了 Promise，这样，不需要浏览器的安排，JavaScript 引擎本身也可以发起任务了。

>由于我们这里主要讲 JavaScript 语言，那么采纳 JSC 引擎的术语，我们把**宿主发起的任务称为宏观任务**，把 **JavaScript 引擎发起的任务称为微观任务**。

# 宏观和微观任务
>JavaScript 引擎等待宿主环境分配宏观任务，在操作系统中，通常等待的行为都是一个事件循环，所以在 Node 术语中，也会把这个部分称为事件循环。

这里每次的循环的过程，其实都是一个宏观任务。我们可以大概理解：宏观任务的队列就相当于事件循环。

在宏观任务中，JavaScript 的 Promise 还会产生异步代码，JavaScript 必须保证这些异步代码在一个宏观任务中完成，因此，每个宏观任务中又包含了一个微观任务队列：

![](https://upload-images.jianshu.io/upload_images/5780538-10e3e4c486ba5200.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有了宏观任务和微观任务机制，我们就可以实现 JS 引擎级和宿主级的任务了，例如：Promise 永远在队列尾部添加微观任务。setTimeout 等宿主 API，则会添加宏观任务。

#### 我们来看端代码
```
    var r = new Promise(function(resolve, reject){
        console.log("a");
        resolve()
    });
    setTimeout(()=>console.log("d"), 0)
    r.then(() => console.log("c"));
    console.log("b")
```
我们发现，不论代码顺序如何，d 必定发生在 c 之后，因为 Promise 产生的是 JavaScript 引擎内部的微任务，而 setTimeout 是浏览器 API，它产生宏任务。

为了理解微任务始终先于下一个宏任务，我们设计一个实验：执行一个耗时 1 秒的 Promise。
```
    setTimeout(()=>console.log("d"), 0)
    var r = new Promise(function(resolve, reject){
        resolve()
    });
    r.then(() => { 
        var begin = Date.now();
        while(Date.now() - begin < 1000);
        console.log("c1") 
        new Promise(function(resolve, reject){
            resolve()
        }).then(() => console.log("c2"))
    });
```
这里我们强制了 1 秒的执行耗时，这样，我们可以确保任务 c2 是在 d 之后被添加到任务队列。

我们可以看到，即使耗时一秒的 c1 执行完毕，再 enque 的 c2，仍然先于 d 执行了，这很好地解释了微任务优先的原理。

通过一系列的实验，我们可以总结一下如何分析异步执行的顺序：

- 首先我们分析有多少个宏任务；
- 在每个宏任务中，分析有多少个微任务；
- 根据调用次序，确定宏任务中的微任务执行次序；
- 根据宏任务的触发规则和调用次序，确定宏任务的执行次序；
- 确定整个顺序。

#### 再看两个难一点的🌰
```
async function t1 () {
  console.log(1)
  console.log(2)
  await Promise.resolve().then(() => console.log('t1p'))
  console.log(3)
  console.log(4)
}

async function t2() {
  console.log(5)
  console.log(6)
  await Promise.resolve().then(() => console.log('t2p'))
  console.log(7)
  console.log(8)
}

t1()
t2()

console.log('end')
```
```
async function t1 () {
  console.log(1)
  console.log(2)
  await new Promise(resolve => {
    setTimeout(() => {
      console.log('t1p')
      resolve()
    }, 1000)
  })
  await console.log(3)
  console.log(4)
}

async function t2() {
  console.log(5)
  console.log(6)
  await Promise.resolve().then(() => console.log('t2p'))
  console.log(7)
  console.log(8)
}

t1()
t2()

console.log('end')
```
> 对于这两个例子我好难解释清楚，总之就是遇到 `await` 就可以像下面这样理解： 
```
// 正常代码
await Promise.resolve().then(() => console.log('t2p'))
console.log(7)
console.log(8)

// 类似等价于
Promise.resolve().then(() => console.log('t2p'))
  .then(() => {
    console.log(7)
    console.log(8)
})
```
