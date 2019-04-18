一直以为ES6的`let`和`const`也就这回事，和`var`最明显的区别就是块级作用域了，直到今天看到一道面试题，深究下来发现一些`let`奥秘。
#### 面试题：
```
for(
  let i = (setTimeout(()=>console.log(i), 2333), 0);
  i < 2;
  i++
){
  
}

// 问 2333 秒之后打印出什么
```

###### 讨论这题我们先回顾一下目前`let`的在for循环上的用法。
用`var`来进行for循环：
```
for(var i = 0; i < 2; i++){
  setTimeout(()=>console.log(i))
}
//由于是var，我们现在改写这个for循环，他相等于如下：
var i; // var变量提升
for(i = 0; i < 2; i++){
  setTimeout(()=>console.log(i))
}

//所以，毫无疑问这里的i打印出来的都是2（setTimeout异步会在i循环以后执行）
```
ES6的`let`解决了`var`这种看似不合理的问题。
```
for(let i = 0; i < 2; i++){
  setTimeout(()=>console.log(i))
}

//此时i将输出 0，1
```
本人在此之前对于此题`let`的最合理的理解([导师链接](https://xiedaimala.com/tasks/b2b61490-85e1-4b2f-a8a5-bc777a8203e0/#/video_tutorial/9d78a1aa-0d4d-4b53-8316-88e5d0ba1aa2))是js内部魔法会帮你新建另一个变量来保存每一次循环内i的值：
```
//再来看上道题
for(let i = 0; i < 2; i++){
  setTimeout(()=>console.log(i))
}
//js内部魔法的神奇之处，他会隐式帮你新建一个变量j，于是for循环将会变成如下：
for(let i = 0; i < 2; i++){
  let j = i;
  setTimeout(()=>console.log(j))
}
//根据let的块级作用域，setTimeout每次打印出来的j都是当前{}作用域下的j
```
###### 原以为自己已经对let的理解已经很深了，一看便认为打印出来是2，但是今天这道面试题又让我对fu*kJS涨姿势了...

![为什么是 0 ?!!!　　=。=](./images/js-let/js-let_(1).png)

###### 让我们重新来梳理一下：
##### 根据答案结果我们来倒推一下，首先for循环里第一句话`let i = (setTimeout(()=>console.log(i), 2333), 0);`会初始化一个 i 且这个 i 是单独存在一个作用域的，这个 i 只代表初始值。当第一句执行结束时这个 i 就封存了，然后在第二句话之前js内部魔法会自动记下这个 i 的值，于是乎`for`就将改写成如下这个样子：
```
for(
  let i = (setTimeout(()=>console.log(i), 2333), 0);
  let j = i;
  j < 2;
  j++
){
  
}
```
##### 也就是说执行到第二句话时，已经是新的一个 i(也就是上述代码的j) 了，从而得出for循环内第一句话是一个作用域，后面两句话又在另一个作用域内。我们也就可以把`i < 2`看成for循环的入口，`i++`看成for循环的出口，每次都是不停地在这个之间循环。
###### 这也许就是JS难学的原因把~~~
<br/>

最后，英文无障碍的胖友们，可以看看最近[他们讨论了一个 JS 题目](https://link.zhihu.com/?target=https%3A//www.youtube.com/watch%3Fv%3DNzokr6Boeaw%26list%3DPLNYkxOF6rcIAKIQFsNbV0JDws_G_bnNo9)，用以吐槽 JS 的 for 循环是多么复杂

HTTP 203 是 Youtube 上的一个栏目，讲一些有趣的知识。


