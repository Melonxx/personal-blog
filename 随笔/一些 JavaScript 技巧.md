# 一、 `&&` 和 `||` 逻辑运算符
- ##### 例一

假设我们想返回一个变量的长度，但是我们不知道变量的类型。
我们可以使用 `if/else` 语句来检查 `foo` 是可接受的类型，但是这可能会变得非常冗长。或运行可以帮助我们简化操作：
```
return (foo || []).length
```
如果变量 `foo` 是 `true`，它将被返回。否则，将返回空数组的长度: `0`。

- ##### 例二
你是否遇到过访问嵌套对象属性的问题？ 你可能不知道对象或其中一个子属性是否存在，这可能会导致令人沮丧的错误。
假设我们想在 `this.state` 中访问一个名为 `data` 的属性，但是在我们的程序成功返回一个获取请求之前，`data` 是未定义的。
根据我们使用它的位置，调用 `this.state.data` 可能会阻止我们的应用程序运行。 为了解决这个问题，我们可以将其做进一步的判断：
```
if (this.state.data) {
  return this.state.data;
} else {
  return 'Fetching Data';
}
```
但这似乎很重复。 `||` 运算符提供了更简洁的解决方案：

```
return (this.state.data || 'Fetching Data');
```
一个新特性: `Optional Chaining`

#### **一个新特性: Optional Chaining**

过去在 Object 属性链的调用中，很容易因为某个属性不存在而导致之后出现`Cannot read property xxx of undefined`的错误。

那 `optional chaining` 就是添加了`?.`这么个操作符，它会先判断前面的值，如果是 `null` 或 `undefined`，就结束调用、返回 `undefined`。

例如，我们可以将上面的示例重构为 `this.state.data?.()`。或者，如果我们主要关注`state` 是否已定义，我们可以返回`this.state?.data`。

该提案目前处于第1阶段，作为一项实验性功能。 你可以在[这里](https://github.com/tc39/proposal-optional-chaining)阅读它，你现在可以通过Babel使用你的JavaScript，将 [@babel/plugin-proposal-optional-chaining](https://babeljs.io/docs/en/babel-plugin-proposal-optional-chaining)添加到你的`.babelrc`文件中。

# 二、`!!` 转换为布尔值
```
const isTrue  = !0;
const isFalse = !1;
const alsoFalse = !!0;
console.log(isTrue); // Result: true
console.log(typeof true); // Result: "boolean"  
```
# 三、转换为数字
- `+`
```
let num1 = "15";
num1 = +num1;
console.log(num1); // Result: 15
let num2 = "1.5";
num2 = +num2;
console.log(num2); // Result: 1.5
```
这也可以用于将布尔值转换为数字，如下所示
```
 console.log(+true);  // Return: 1
 console.log(+false); // Return: 0
```
- `~~`
在某些上下文中，+将被解释为连接操作符，而不是加法操作符。当这种情况发生时(你希望返回一个整数，而不是浮点数)，您可以使用两个波浪号:~~。

连续使用两个波浪有效地否定了操作，因为— ( — n — 1) — 1 = n + 1 — 1 = n。 换句话说，`~—16` 等于`15`。
```
let num1 = "15";
num1 = ~~num1;
console.log(num1); // Result: 15
let num2 = "1.5";
num2 = ~~num2;
console.log(num2); // Result: 1
```
- `|`
```
console.log(23.9 | 0);  // Result: 23
console.log(-23.9 | 0); // Result: -23
```
# 四、数组截断
如果要从数组的末尾删除值，有比使用 `splice()` 更快的方法。
例如，如果你知道原始数组的大小，您可以重新定义它的length属性，就像这样
```
let array = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
array.length = 4;
console.log(array); // Result: [0, 1, 2, 3]
```
这是一个特别简洁的解决方案。但是，我发现 `slice()` 方法的运行时更快。如果速度是你的主要目标，考虑使用：
```
let array = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
array = array.slice(0, 4);
console.log(array); // Result: [0, 1, 2, 3]
```
# 五、获取数组中的最后一项
数组方法 `slice()` 可以接受负整数，如果提供它，它将接受数组末尾的值，而不是数组开头的值。
```
let array = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
console.log(array.slice(-1)); // Result: [9]
console.log(array.slice(-2)); // Result: [8, 9]
console.log(array.slice(-3)); // Result: [7, 8, 9]
```
# 六、格式化JSON代码
最后，你之前可能已经使用过 `JSON.stringify`，但是您是否意识到它还可以帮助你缩进 `JSON`？
`stringify()` 方法有两个可选参数：一个 `replacer` 函数，可用于过滤显示的 `JSON` 和一个空格值。
```
console.log(JSON.stringify({ alpha: 'A', beta: 'B' }, null, '\t'));
// Result:
// '{
//     "alpha": A,
//     "beta": B
// }'
```

