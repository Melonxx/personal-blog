```
function foo(){
  try{
    return 0;
  } catch(err) {

  } finally {
    console.log("a")
  }
}

console.log(foo());
```
很明，finally 确实执行了，而且 return 语句也生效了，foo() 返回了结果 0。

虽然 return 执行了，但是函数并没有立即返回，又执行了 finally 里面的内容，这样的行为违背了很多人的直觉。

如果在这个例子中，我们在 finally 中加入 return 语句，会发生什么呢？
```
function foo(){
  try{
    return 0;
  } catch(err) {

  } finally {
    return 1;
  }
}

console.log(foo());
```
通过实际执行，我们看到，`finally` 中的 `return` “覆盖”了 `try` 中的 `return`。在一个函数中执行了两次 `return`，这已经超出了很多人的常识，也是其它语言中不会出现的一种行为。

面对如此怪异的行为，我们当然可以把它作为一个孤立的知识去记忆，但是实际上，这背后有一套机制在运作。

这一机制的基础正是 `JavaScript` 语句执行的完成状态，我们用一个标准类型来表示：**Completion Record**（Completion Record 用于描述异常、跳出等语句执行过程）。

### Completion Record 表示一个语句执行完之后的结果，它有三个字段：

- [[type]] 表示完成的类型，有 break continue return throw 和 normal 几种类型；
- [[value]] 表示语句的返回值，如果语句没有，则是 `empty`；
- [[target]] 表示语句的目标，通常是一个 `JavaScript` 标签（标签在后文会有介绍）。

>JavaScript 正是依靠语句的 Completion Record 类型，方才可以在语句的复杂嵌套结构中，实现各种控制。接下来我们要来了解一下 JavaScript 使用 Completion Record 类型，控制语句执行的过程。

首先我们来看看语句有几种分类。

![](https://upload-images.jianshu.io/upload_images/5780538-e877d151fc4dab6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 普通的语句
>在 JavaScript 中，我们把不带控制能力的语句称为普通语句。普通语句有下面几种。

普通语句执行后，会得到 [[type]] 为 `normal` 的 `Completion Record`，`JavaScript` 引擎遇到这样的 `Completion Record`，会继续执行下一条语句。

这些语句中，只有表达式语句会产生 [[value]]，当然，从引擎控制的角度，这个 `value` 并没有什么用处。

如果你经常使用 `chrome` 自带的调试工具，可以知道，输入一个表达式，在控制台可以得到结果，但是在前面加上 `var`，就变成了 `undefined`。

![](https://upload-images.jianshu.io/upload_images/5780538-955ded1e76505ccc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>Chrome 控制台显示的正是语句的 Completion Record 的 [[value]]。

#### 语句块
>语句块就是拿大括号括起来的一组语句，它是一种语句的复合结构，可以嵌套

语句块本身并不复杂，我们需要注意的是语句块内部的语句的 `Completion Record` 的 [[type]] 如果不为 `normal`，会打断语句块后续的语句执行。

比如我们考虑，一个 [[type]] 为 `return` 的语句，出现在一个语句块中的情况。

从语句的这个 `type` 中，我们大概可以猜到它由哪些特定语句产生，我们就来说说最开始的例子中的 `return`。

`return` 语句可能产生 `return` 或者 `throw` 类型的 `Completion Record`。我们来看一个例子。

先给出一个内部为普通语句的语句块：
```
{
  var i = 1; // normal, empty, empty
  return i; // return, 1, empty
  i ++; 
  console.log(i)
} // return, 1, empty
```
但是假如我们在 block 中插入了一条 return 语句，产生了一个非 normal 记录，那么整个 block 会成为非 normal。这个结构就保证了非 normal 的完成类型可以穿透复杂的语句嵌套结构，产生控制效果。

#### 控制型语句
> 控制型语句带有 `if`、`switch` 关键字，它们会对不同类型的 `Completion Record` 产生反应。

控制类语句分成两部分，一类是对其内部造成影响，如 `if、switch、while/for、try`。另一类是对外部造成影响如 `break、continue、return、throw`，这两类语句的配合，会产生控制代码执行顺序和执行逻辑的效果，这也是我们编程的主要工作。

一般来说， `for/while - break/continue` 和 `try - throw` 这样比较符合逻辑的组合，是大家比较熟悉的，但是，实际上，我们需要控制语句跟 `break 、continue 、return 、throw` 四种类型与控制语句两两组合产生的效果。

![](https://upload-images.jianshu.io/upload_images/5780538-a9ac239deb17f007.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过这个表，我们不难发现知识的盲点，也就是我们最初的的 `case` 中的 `try` 和 `return` 的组合了。

因为 `finally` 中的内容必须保证执行，所以 `try/catch` 执行完毕，即使得到的结果是非 `normal` 型的完成记录，也必须要执行 `finally`。

而当 `finally` 执行也得到了非 `normal` 记录，则会使 `finally` 中的记录作为整个 `try` 结构的结果。

#### 带标签的语句

> 实际上，任何 JavaScript 语句是可以加标签的，在语句前加冒号即可：
```
    firstStatement: var i = 1;
```
大部分时候，这个东西类似于注释，没有任何用处。唯一有作用的时候s是：与完成记录类型中的 `target` 相配合，用于跳出多层循环。
```
    outer: while(true) {
      inner: while(true) {
          break outer;
      }
    }
    console.log("finished")
```
`break/continue` 语句如果后跟了关键字，会产生带 `target` 的完成记录。一旦完成记录带了 `target`，那么只有拥有对应 `label` 的循环语句会消费它。


