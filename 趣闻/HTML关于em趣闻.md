### em的计算标准并不是父元素的字体大小？
###### 首先我们先来看百度的搜索结果（错误言论！）...

![百度上的错误言论！！！](./images/html-em/html-em_(1).png)

###### 网上以讹传讹这么多年了，一时难以消除，你一不小心记住了错误的知识就不好办了（在此之前本人也是一直误以为em是根据父元素的`font-size`计算出来的=。=）。

![](./images/html-em/html-em_(2).png)

##### 上面的例子很明显地说明 em 只跟当前元素的 font-size 有关。


###### 那么为什么网上的傻*说 em 是基于父元素的 font-size 呢？因为如下代码：
```
.parent{
  font-size: 12px;
}
.child{
  font-size: 2em;
}
```
因为这些傻*直接把 em 写到 font-size 上了，所以他就没有机会改子元素的 font-size，那么子元素的默认 font-size 就是父元素的 font-size。

所以这些傻*就说 em 是基于父元素的 font-size。

你只要改成
```
.child{
  font-size: 100px;
  height: 2em;
  border: 1px solid red;
}
```
##### 而且推荐的网站 MDN 也说得十分清楚：基于父元素只是 em 用在 font-size 上时的特例啊，看下图。

![](./images/html-em/html-em_(3).png)

#### 最后的话：学前端还是多看看权威的官网文档吧（推荐MDN），以免再次被这种错误坑害数十年。。。