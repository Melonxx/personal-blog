大家都知道 position: fixed 是相对于视口（viewport）定位的。

但是这个「真理」会受另一个元素的影响……对，我知道你很震惊……

先看正常情况：

![正常情况](./images/html-position/html-position_(1).png)

网页右边是一个 iframe，红色的 .fixed 元素相对于 iframe 视口左上角定位，与我们预期一致。

接下来我在 .box 上面加一个 CSS3 中的属性，就会改变你的认知：

![amazing](./images/html-position/html-position_(2).png)

父容器加了 transform 之后，fixed 定位的元素居然相对于父容器定位。

天知道以后 CSS 会不会加更多元素来影响我已经认为是真理的事情？

我说一个更实际的问题吧，你敢在接手一个项目时，在任意一个元素上加 transform 属性吗？

你不敢！除非你把它的每个子元素的属性都检查一遍……确定没有 fixed 定位。


