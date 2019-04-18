/*1.尽量减少代码重复*/
当某些值相互依赖时，应该把他们的相互关系用代码表达出来
比如:
```swift
{
    padding: 6px 16px;
    border: 1px solid #000;
    background: #58a linear-gradient(#fff, #58a);
    border-radius: 4px;
    box-shadow: 0 1px 5px gary;
    color: white;
    text-shadow: 0 -1px 5px pink;
    font-size: 20px;
    line-height: 30px;
}
```
可以尝试改成em单位，如果你希望能根据父级建立关联（代码可维护性的最大要素就是尽量减少改动时要编辑的地方）
```swift
{
    padding: .3em .8em;    //这里的浮点数个人认为填上0，有利于代码可读性
    border: 1px solid #000;
    background: #58a linear-gradient(#fff, #58a);
    border-radius: .2em;
    box-shadow: 0 .05em .05em gary;
    color: white;
    text-shadow: 0 -.05em .05em pink;
    font-size: 125%;    //假设父级字号是16px
    line-height: 1.5;
}
```
/*2.代码易维护VS代码量少*/
```swift
{
    border-width: 10px 10px 10px 0;
}
{
    border-width: 10px;
    border-left-width: 0;
}
```
虽然上一条只需要一条声明搞定，但是如果把它拆成两条声明的话，改动起来就容易多了。
/*3.currentcolor*/
当前的标签所继承的文字颜色
/*4.推荐使用HSLA而不是RGBA来产生半透明的白色，因为他的字符长度更短，她的重复率更低*/
/*5.calc()方案*/
可以允许我们在css中进行运算
例如想把背景图片定位到距离底边10px距离右边20px的位置:
```swift
{
    background-position: calc(100% - 20px) calc(100% - 10px);
}
```
/*6.改变偏移中心点来居中*/
```swift
{
    transform: translate(-50, -50%);
}
```
此时使用top: 50%;left: 50%;达到居中。
/*7.减少使用 !important 优先级过高*/
/*8.层级(z-index)必须清晰明确，页面弹窗、气泡为最高级（最高级为999），不同弹窗气泡之间可在三位数之间调整；普通区块为10-90内10的倍数；区块展开、弹出为当前父层级上个位增加，禁止层级间盲目攀比。*/
/*9.属性编写顺序
  1.位置属性(position, top, right, z-index, display, float等)
  2.大小(width, height, padding, margin)
  3.文字系列(font, line-height, letter-spacing, color, text-align等)
  4.背景(background, border等)
  5.其他(animation, transition等)
*/
/*10.排版规范
  (1)使用4个空格，而不使用tab或者混用空格+tab作为缩进；
  (2)规则可以写成单行，或者多行，但是整个文件内的规则排版必须统一；

  单行形式书写风格的排版约束
  - 如果是在html中写内联的css，则必须写成单行；
  - 每一条规则的大括号 { 前后加空格 ；
  - 每一条规则结束的大括号 } 前加空格；
  - 属性名冒号之前不加空格，冒号之后加空格；
  - 每一个属性值后必须添加分号; 并且分号后空格；
  - 多个selector共用一个样式集，则多个selector必须写成多行形式 ；

  多行形式书写风格的排版约束
  - 每一条规则的大括号 { 前添加空格;
  - 多个selector共用一个样式集，则多个selector必须写成多行形式 ;
  - 每一条规则结束的大括号 } 必须与规则选择器的第一个字符对齐 ;
  - 属性名冒号之前不加空格，冒号之后加空格;
  - 属性值之后添加分号;
*/
/*11.更新中*/