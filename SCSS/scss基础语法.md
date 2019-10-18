```
$red-color: #6f0; // 变量
.userCard {
  width: 100px;
  &.active { // &符号
    background: yellow;
  }
  &-name {
    color: black;
    &:hover {
      color: $red-color;
    }
    font: { // 属性连写时
      size: 28px;
      weight: bold;
      family: 'Courier New', Courier, monospace;
    }
  }
```

#  一、变量以 `$` 开头
# 二、&、#{}(类似与js的模版字符串的变量写法`#{$font-size}`)
以下写法会被翻译成
```
.userCard {
  width: 100px;
  &.active {
    background: yellow;
  }
  &-name {
    color: black;
  }
}
```
这样
```
.userCard {
  width: 100px;
}
.userCard.active {
  background: yellow;
}
.userCard-name {
  color: black;
}
```
# 三、属性连写
以下写法
```
.userCard-name {
    color: black;
    font: {
      size: 28px;
      weight: bold;
      family: 'Courier New', Courier, monospace;
    }
}
```
等同于
```
.userCard-name {
    color: black;
    font-size: 28px;
    font-weight: bold;
    font-family: 'Courier New', Courier, monospace;
}
```
# 四、加减乘除运算（特别注意`/`在某些情况下会被css当作简写属性的间隔符）
# 五、函数
一个响应式移动端的函数栗子：
```
 @function px($npx) {
     @return $npx/375 * 100vw;
 }
```
六、@content
```
// 定义
@mixin phone {
  @media (max-width: 500px) {
    @content;
  }
}
// 使用
@include phone {
  /* // 这一注释部分会被直接代替上面函数`@content`
  > ul {
    display: none;
  }
   */
}
```