近期开始的了新的学习(react)，
本人学习有个习惯，
喜欢去探索由来和历史，
俗称吃瓜(手动滑稽)
>一般网上的视频教程只会教你怎么用 `react` 但是并不会告诉你 `react` 是当初是怎么来的，为了解决哪些痛点。

首先我们先来看一段 `js` [原生代码](https://jsbin.com/tiyowiy/3/edit?html,js,output)。

![](./images/react-origin/react-origin_(1).png)


这是一段很简单的代码，用于操作 `result` 的增减，
现在，
让我们来抽象的看待这个问题，
画个图来表示：

![](./images/react-origin/react-origin_(2).png)

- 不管你使用原生 `js` 还是 `jq` 都要通过dom提供的 `API`，经历先从页面取到内容，然后经过 `js` 操作以后再填回去。
- `react` 同学认为这太智障了，虽然简单，但是能不能砍掉上面的步骤(从dom取内容)或下面的步骤(填回内容)呢，从而更简便呢？
- 于是 `react` 同学砍掉了上面的步骤(砍掉下面的步骤不现实，因为js将无法通知页面)，直接在 `js` 中生成 `HTML` 然后自动页面中同步(用虚线表示)，然后当数据更新时，`react` 将重新生成一个对象，再自动的去更新原来页面的元素，从而使得页面中的数据也是最新值。
- 这样一来事情就变得简单了，代码量减少一半， `js` 再也不用去页面中取元素，只需再 `js` 中生成填回页面中即可。从来不去取页面中的元素，只去填东西。`react` 就是在这种理念下诞生了。

**接下里我们用这种理念重写一次上面的代码。**

根据上图的操作在页面中加入一个 `span`

![](./images/react-origin/react-origin_(3).png)

然后为了加个按钮（为了简化先在页面中手动增加）

![](./images/react-origin/react-origin_(4).png)

目前代码有点粗糙，但是为了简单明了演示上图的理念。
1. 在 `js` 中生成对象插入到页面中
2. 更新数据时，重新在 `js` 中生成对象同步更新原来的页面元素

接着我们开始优化一下[上图代码](https://jsbin.com/meticaf/1/edit?html,js,output)，并把按钮也放入 `js` 代码中：

![](./images/react-origin/react-origin_(5).png)

但是炸一看[这代码](https://jsbin.com/vevavur/1/edit?html,js,output)还是很傻`*`，于是我们继续来 **分三步** 更层次优化代码

1. 这个 `React.creatElement` 方法名字太长了，抽出
2. 这些个变量只用过一次，那我们就可以跳过声明变量，直接使用

于是代码就变成了这个样子：

![](./images/react-origin/react-origin_(6).png)


具有一双慧眼的你，应该也看的出来这样的代码很像一种东西把...

这也就是 `react` 最聪明的一点优化，

![请先忽略变量的转换](./images/react-origin/react-origin_(7).png)


我们惊讶的发现，这样的 `js` 代码和 `HTML` 标签上并没有什么区别，
于是聪明的 `react` 同学诞生了另一个想法(JSX)，我们能不能让用户写下面的代码，然后经过程序转换成上面的代码呢？
这样一来，最终的结果就是我们通过写下面的代码来替换（也就是等价于）上面的代码。
经过 `react` 同学一番折腾，`JSX` 语法上线了 （**敲黑板，重点**：我们并不是在写HTML代码，而是用HTML的形式来写JS代码）

Finally，代码变成了[这样子](https://jsbin.com/vudezis/10/edit?html,js,output)（`react` 最终的样子）

![](./images/react-origin/react-origin_(8).png)

本文完。

>PS
我们改变过后的类似 `HTML` 代码就是虚拟 `DOM`

还有回答一些弱智问题
为什么我们在写 `JSX` 的时候，绑定事件的时候函数名后面不带括号，
因为在一开始的时候我们在 `js` 中写的是对象 `{onClick: fn}` ，这里需要的fn是整个函数，如果写成 `{onClick: fn()}`，赋值给 `onClick` 的就是函数的返回值。











