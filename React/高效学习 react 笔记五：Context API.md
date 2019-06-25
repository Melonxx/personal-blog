假设我们现在遇到如下场景：
```
function fn1(){
  fn2()
}
function fn2(){
  fn3()
}
function fn3(){
  fn4()
}
function fn4(){
  console.log(...) // 我想在这里打印出n的值
}

{
  let n = 99;
  fn1()
}
```

显然最简单的思路不就是直接把 `n` 当作参数，一层一层往下传不就好了。
```
function fn1(n){
  fn2(n)
}
function fn2(n){
  fn3(n)
}
function fn3(n){
  fn4(n)
}
function fn4(n){
  console.log(n) // 99
}

{
  let n = 99;
  fn1(n)
}
```
这样我们就能在 `fn4` 中打印出 `n` 的值。

但是这样一层层的传递似乎也很麻烦，那我们能不能创造出一个简便的方法呢~

我们再试试把变量 `n` 提升成全局变量：

```
let n = 99
function fn1(){
  fn2()
}
function fn2(){
  fn3()
}
function fn3(){
  fn4()
}
function fn4(){
  console.log(n) // 99
}

{
  fn1()
}
```
这样我们确实能快速打印出 `n` 的变量，但是我们为此创建了一个全局变量，这又引发了了一个更麻烦的问题！**全局变量污染**

所以我们能不能发明一种更好的方法呢~

比如说 “局部的全局变量” ， 只能 `n` 在函数中能访问，外部访问不到：

```
{
  let x = {}
  window.setN = function (key, data) {
    x[key] = data
  }
  window.fn1 = function fn1(){
    fn2()
  }
  function fn2(){
    fn3()
  }
  function fn3(){
    fn4()
  }
  function fn4(){
    console.log(x.n) // 99
  }
}

{
  window.setN('n', 99)
  window.fn1()
}
```

> 这样一来，我们通过向外部暴露一个 `set` 方法就能去改变 `n` 的值，`fn4` 又能直接访问 `n` 的值，这种操作就是今天要介绍的 **Context API**

我们拿出[上文](https://www.jianshu.com/p/b3c8f576f8a6)的[例子](http://js.jirengu.com/xusoh/6/edit?js,output)来改写。

我们直接通过 `const moneyContext = React.createContext()` 创建一个 `context`，

然后将父组件通过 `moneyContext.Provider` 包起来，并传递一个 `value={money.money}` 的 `props`

接下来我们如果在哪个子组件中使用，只需要在子组件外层包一个 `<moneyContext.Consumer>` 然后通过函数的形式，在函数的参数中拿到传递的值即可。

[完整代码在此](http://js.jirengu.com/hazil/7/edit?js,output)

这样我们就在子组件中直接传递了 `money`，但是如果这个变量是我们在父组件中生命的，子组件中无法更改（永远不要试图去更改从 `props` 传递下来的值），那我们如果想在子组件中也更改这个值呢~

很显然，我们当初在 `value` 中传递的是值，我们直接传递一个包括更改 `money` 方法的对象不就好了，

所以代码就变成了[这个样子](http://js.jirengu.com/viqiq/6/edit?js,output)

> 最终我们用 `Context` 也达到了和 `redux` 的效果。并且写法也很像 (Provider) ，
> 所以似乎再简单应用上，我们完全可以用 `Context` 代替 `Redux`。  :）



