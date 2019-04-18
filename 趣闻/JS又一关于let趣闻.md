#let到底有没有变量提升？
我在 StackOverflow 上闲逛的时候，无意中发现了一个是关于「let 到底有没有提升」的问题：

[Are variables declared with let or const not hoisted in ES6?](http://link.zhihu.com/?target=https%3A//stackoverflow.com/questions/31219420/are-variables-declared-with-let-or-const-not-hoisted-in-es6)

其中一个高票回答认为 [JS 中所有的声明（var/let/const/function/class），都存在提升](http://link.zhihu.com/?target=https%3A//stackoverflow.com/a/31222689/1262580)。

```
let x = 'global'
{
  console.log(x) // Uncaught ReferenceError: x is not defined
  // why not global？
  let x = 1
}
```
原因有两个

1. console.log(x) 中的 x 指的是下面的 x，而不是全局的 x
2. 执行 log 时 x 还没「初始化」，所以不能使用（也就是所谓的暂时死区）

看到这里，你应该明白了 let 到底有没有提升：

- let 的「创建」过程被提升了，但是初始化没有提升。
- var 的「创建」和「初始化」都被提升了。
- function 的「创建」「初始化」和「赋值」都被提升了。

我们再来看一个例子~

![let](./images/js-let/js-let_(2).png)

这个问题说明：如果 let x 的初始化过程失败了，那么

- x 变量就将永远处于 created 状态。
- 你无法再次对 x 进行初始化（初始化只有一次机会，而那次机会你失败了）。
- 由于 x 无法被初始化，所以 x 永远处在暂时死区（也就是盗梦空间里的 limbo）！
- 有人会觉得 JS 坑，怎么能出现这种情况；其实问题不大，因为此时代码已经报错了，后面的代码想执行也没机会。


参考[知乎文章](https://www.zhihu.com/question/62966713/answer/203898766)



