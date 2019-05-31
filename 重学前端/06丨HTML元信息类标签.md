### [head 标签](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/head)

首先我们先来了解一下 `head` 标签，`head` 标签本身并**不携带任何信息**，它主要是作为盛放其它语义类标签的容器使用。

head 标签规定了自身必须是 html 标签中的第一个标签，它的内容**必须包含一个 title**，并且最多只能包含一个 base。如果文档作为 iframe，或者有其他方式指定了文档标题时，可以允许不包含 `title` 标签。

### [title 标签](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/title)


你还记得吗，我们的语义类标签中也有一组表示标题的标签：`h1-h6`。

`heading` 和 `title` 两个英文单词意义区分十分微妙，在中文中更是找不到对应的词汇来区分。但是实际使用中，两者确实有一定区别。

>在 `HTML` 标准中，特意讨论了这个问题。我们思考一下，假设有一个介绍蜜蜂跳舞求偶仪式的科普页面，我们试着把以下两个文字分别对应到 `title` 和 `h1`。
>
>- 蜜蜂求偶仪式舞蹈
>- 舞蹈
>
>在听 / 看正确答案前，你不妨先想想，自己的答案是什么呢？为什么？

好了，思考之后，我们来看看正确答案。正确答案是“蜜蜂求偶仪式舞蹈”放入 `title`，“舞蹈”放入 `h1`。

我来讲一讲为什么要这样放呢？这主要是考虑到 `title` 作为元信息，可能会被用在浏览器收藏夹、微信推送卡片、微博等各种场景，这时侯往往是上下文缺失的，所以 **`title` 应该是完整地概括整个网页内容的**。

**而 `h1` 则仅仅用于页面展示**，它可以默认具有上下文，并且有链接辅助，所以可以简写，即便无法概括全文，也不会有很大的影响。

### [base 标签](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/base)

`base` 标签实际上是个**历史遗留标签**。它的作用是给页面上所有的 URL 相对地址提供一个基础。

`base` 标签**最多只有一个**（若有多个就使用第一个），它改变全局的链接地址，它是一个非常危险的标签，容易造成跟 JavaScript 的配合问题，所以在实际开发中，我比较建议你使用 JavaScript 来代替 base 标签。

### [meta 标签](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta)

在 `head` 中可以出现任意多个 `meta` 标签。**一般的 `meta` 标签由 `name` 和 `content` 两个属性来定义**。`name` 表示元信息的名，`content` 则用于表示元信息的值。

##### 具有 http-equiv 属性的 meta
具有 `http-equiv` 属性的 `meta` 标签，表示执行一个命令，这样的 `meta` 标签可以不需要 `name` 属性了。

例如，下面一段代码，相当于添加了 `content-type` 这个 `http` 头，并且指定了 `http` 编码方式。
```
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
```
除了 `content-type`，还有以下几种命令：

- content-language 指定内容的语言；
- default-style 指定默认样式表；
- refresh 刷新；
- set-cookie 模拟 http 头 set-cookie，设置 cookie；
- x-ua-compatible 模拟 http 头 x-ua-compatible，声明 ua 兼容性；
- content-security-policy 模拟 http 头 content-security-policy，声明内容安全策略。

##### name 为 viewport 的 meta
实际上，meta 标签可以被**自由定义**，只要写入和读取的双方约定好 `name` 和 `content` 的格式就可以了。

我们来介绍一个 `meta` 类型，它**没有在 `HTML` 标准中定义，却是移动端开发的事实标准**：它就是 name 为 `viewport` 的 `meta`。
>orz `viewport` 竟然是自定义的

这类 `meta` 的 `name` 属性为 `viewport`，它的 `content` 是一个复杂结构，是用逗号分隔的键值对，键值对的格式是 `key=value`。

它能表示的全部属性如下：
- width：页面宽度，可以取值具体的数字，也可以是 device-width，表示跟设备宽度相等。
- height：页面高度，可以取值具体的数字，也可以是 device-height，表示跟设备高度相等。
- initial-scale：初始缩放比例。
- minimum-scale：最小缩放比例。
- maximum-scale：最大缩放比例。
- user-scalable：是否允许用户缩放。

对于已经做好了移动端适配的网页，应该把用户缩放功能禁止掉，宽度设为设备宽度，一个标准的 `meta` 如下：
```
<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no">
```

##### 其它预定义的 meta
在 HTML 标准中，还定义了一批 `meta` 标签的 `name`，可以视为一种有约定的 `meta`，我在这里列出来，你可以简单了解一下。

application-name：如果页面是 Web application，用这个标签表示应用名称。

- author: 页面作者。
- description：页面描述，这个属性可能被用于搜索引擎或者其它场合。
- generator: 生成页面所使用的工具，主要用于可视化编辑器，如果是手写 HTML 的网页，不需要加这个 meta。
- keywords: 页面关键字，对于 SEO 场景非常关键。
- referrer: 跳转策略，是一种安全考量。
- theme-color: 页面风格颜色，实际并不会影响页面，但是浏览器可能据此调整页面之外的 UI（如窗口边框或者 tab 的颜色）。

[others](http://www.alenqi.site/2018/03/04/complete-tags/)

