### 什么是面向对象？

在《面向对象分析与设计》这本书中，Grady Booch 替我们做了总结，他认为，从人类的认知角度来说，对象应该是下列事物之一：

1. 一个可以触摸或者可以看见的东西；
2. 人的智力可以理解的东西；
3. 可以指导思考或行动（进行想象或施加动作）的东西。

### JavaScript 对象的特征

在我看来，不论我们使用什么样的编程语言，我们都先应该去理解对象的本质特征（参考 Grandy Booch《面向对象分析与设计》）。总结来看，对象有如下几个特点。

1. 对象具有唯一标识性：即使完全相同的两个对象，也并非同一个对象。
2. 对象有状态：对象具有状态，同一对象可能处于不同状态之下。
3. 对象具有行为：即对象的状态，可能因为它的行为产生变迁。

### JavaScript 对象的两类属性

对 JavaScript 来说，属性并非只是简单的名称和值，JavaScript 用一组特征（attribute）来描述属性（property）。

先来说第一类属性，数据属性。它比较接近于其它语言的属性概念。数据属性具有四个特征。

1. value：就是属性的值。
2. writable：决定属性能否被赋值。
3. enumerable：决定 for in 能否枚举该属性。
4. configurable：决定该属性能否被删除或者改变特征值。
在大多数情况下，我们只关心数据属性的值即可。

第二类属性是访问器（getter/setter）属性，它也有四个特征。

1. getter：函数或 undefined，在取属性值时被调用。
2. setter：函数或 undefined，在设置属性值时被调用。
3. enumerable：决定 for in 能否枚举该属性。
4. configurable：决定该属性能否被删除或者改变特征值。

访问器属性使得属性在读和写时执行代码，它允许使用者在写和读属性时，得到完全不同的值，它可以视为一种函数的语法糖。

我们通常用于定义属性的代码会产生数据属性，其中的 writable、enumerable、configurable 都默认为 true。我们可以使用内置函数 Object.getOwnPropertyDescripter 来查看，如以下代码所示
```
    var o = { a: 1 };
    o.b = 2;
    //a 和 b 皆为数据属性
    Object.getOwnPropertyDescriptor(o,"a") // {value: 1, writable: true, enumerable: true, configurable: true}
    Object.getOwnPropertyDescriptor(o,"b") // {value: 2, writable: true, enumerable: true, configurable: true}
```
我们在这里使用了两种语法来定义属性，定义完属性后，我们用 JavaScript 的 API 来查看这个属性，我们可以发现，这样定义出来的属性都是数据属性，writeable、enumerable、configurable 都是默认值为 true。

如果我们要想改变属性的特征，或者定义访问器属性，我们可以使用 Object.defineProperty，示例如下：
```
    var o = { a: 1 };
    Object.defineProperty(o, "b", {value: 2, writable: false, enumerable: false, configurable: true});
    //a 和 b 都是数据属性，但特征值变化了
    Object.getOwnPropertyDescriptor(o,"a"); // {value: 1, writable: true, enumerable: true, configurable: true}
    Object.getOwnPropertyDescriptor(o,"b"); // {value: 2, writable: false, enumerable: false, configurable: true}
    o.b = 3;
    console.log(o.b); // 2
```

### 原型链----请看我的原型链文章
### ES6 中的类

我们先看下类的基本写法：
```
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
  // Getter
  get area() {
    return this.calcArea();
  }
  // Method
  calcArea() {
    return this.height * this.width;
  }
}
```

在现有的类语法中，getter/setter 和 method 是兼容性最好的。

我们通过 get/set 关键字来创建 getter，通过括号和大括号来创建方法，数据型成员最好写在构造器里面。

类的写法实际上也是由原型运行时来承载的，逻辑上 JavaScript 认为每个类是有共同原型的一组对象，类中定义的方法和属性则会被写在原型对象之上。

### JavaScript 中的对象分类

我们可以把对象分成几类。

* 宿主对象（host Objects）：由 JavaScript 宿主环境提供的对象，它们的行为完全由宿主环境决定。

* 内置对象（Built-in Objects）：由 JavaScript 语言提供的对象。

   * 固有对象（Intrinsic Objects ）：由标准规定，随着 JavaScript 运行时创建而自动创建的对象实例。
   * 原生对象（Native Objects）：可以由用户通过 Array、RegExp 等内置构造器或者特殊语法创建的对象。
   * 普通对象（Ordinary Objects）：由{}语法、Object 构造器或者 class 关键字定义类创建的对象，它能够被原型继承。

##### 宿主对象
JavaScript 宿主对象千奇百怪，但是前端最熟悉的无疑是浏览器环境中的宿主了。

在浏览器环境中，我们都知道全局对象是 window，window 上又有很多属性，如 document。

实际上，这个全局对象 window 上的属性，一部分来自 JavaScript 语言，一部分来自浏览器环境。

JavaScript 标准中规定了全局对象属性，w3c 的各种标准中规定了 Window 对象的其它属性。

宿主对象也分为固有的和用户可创建的两种，比如 document.createElement 就可以创建一些 dom 对象。

宿主也会提供一些构造器，比如我们可以使用 new Image 来创建 img 元素，这些我们会在浏览器的 API 部分详细讲解。
##### 内置对象·固有对象
我们在前面说过，固有对象是由标准规定，随着 JavaScript 运行时创建而自动创建的对象实例。

固有对象在任何 JS 代码执行前就已经被创建出来了，它们通常扮演者类似基础库的角色。我们前面提到的“类”其实就是固有对象的一种。

ECMA 标准为我们提供了一份固有对象表，里面含有 150+ 个固有对象。你可以通过[这个链接](https://www.ecma-international.org/ecma-262/9.0/index.html#sec-well-known-intrinsic-objects)查看。

但是遗憾的是，这个表格并不完整。所以在本篇的末尾，我设计了一个小实验（小实验：获取全部 JavaScript 固有对象），你可以自己尝试一下，数一数一共有多少个固有对象。

##### 内置对象·原生对象
我们把 JavaScript 中，能够通过语言本身的构造器创建的对象称作原生对象。在 JavaScript 标准中，提供了 30 多个构造器。按照我的理解，按照不同应用场景，我把原生对象分成了以下几个种类。

![](./images/04/04_(1).png)

通过这些构造器，我们可以用 new 运算创建新的对象，所以我们把这些对象称作原生对象。
几乎所有这些构造器的能力都是无法用纯 JavaScript 代码实现的，它们也无法用 class/extend 语法来继承。

这些构造器创建的对象多数使用了私有字段, 例如：

- Error: [[ErrorData]]
- Boolean: [[BooleanData]]
- Number: [[NumberData]]
- Date: [[DateValue]]
- RegExp: [[RegExpMatcher]]
- Symbol: [[SymbolData]]
- Map: [[MapData]]

这些字段使得原型继承方法无法正常工作，所以，我们可以认为，所有这些原生对象都是为了特定能力或者性能，而设计出来的“特权对象”。
##### 用对象来模拟函数与构造器：函数对象与构造器对象
我在前面介绍了对象的一般分类，在 JavaScript 中，还有一个看待对象的不同视角，这就是用对象来模拟函数和构造器。

事实上，JavaScript 为这一类对象预留了私有字段机制，并规定了抽象的函数对象与构造器对象的概念。

**函数对象的定义是：具有 [[call]] 私有字段的对象，构造器对象的定义是：具有私有字段 [[construct]] 的对象。**

JavaScript 用对象模拟函数的设计代替了一般编程语言中的函数，它们可以像其它语言的函数一样被调用、传参。任何宿主只要提供了“具有 [[call]] 私有字段的对象”，就可以被 JavaScript 函数调用语法支持。

我们可以这样说，任何对象只需要实现 [[call]]，它就是一个函数对象，可以去作为函数被调用。而如果它能实现 [[construct]]，它就是一个构造器对象，可以作为构造器被调用。

对于为 JavaScript 提供运行环境的程序员来说，只要字段符合，我们在上文中提到的宿主对象和内置对象（如 Symbol 函数）可以模拟函数和构造器。

当然了，用户用 function 关键字创建的函数必定同时是函数和构造器。

> 例如：Symbol是没有constructor,不能使用new去实例化对象。

##### 特殊行为的对象

除了上面介绍的对象之外，在固有对象和原生对象中，有一些对象的行为跟正常对象有很大区别。

它们常见的下标运算（就是使用中括号或者点来做属性访问）或者设置原型跟普通对象不同，这里我简单总结一下。

- Array：Array 的 length 属性根据最大的下标自动发生变化。
- Object.prototype：作为所有正常对象的默认原型，不能再给它设置原型了。
- String：为了支持下标运算，String 的正整数属性访问会去字符串里查找。
- Arguments：arguments 的非负整数型下标属性跟对应的变量联动。
- 模块的 namespace 对象：特殊的地方非常多，跟一般对象完全不一样，尽量只用于 import 吧。
- 类型数组和数组缓冲区：跟内存块相关联，下标运算比较特殊。
- bind 后的 function：跟原来的函数相关联。


