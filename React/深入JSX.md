## 点表示法用于JSX类型

还可以使用 JSX 中的点表示法来引用 React 组件。你可以方便地从一个模块中导出许多 React 组件。例如，有一个名为 `MyComponents.DatePicker` 的组件，你可以直接在 JSX 中使用它：
```
import React from 'react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
```

## 用户定义组件必须首字母大写

当元素类型以小写字母开头时，它表示一个内置的组件，如 `<div>` 或 `<span>`，将导致字符串 `'div'` 或 `'span'` 传递给 `React.createElement`。 以大写字母开头的类型，如 `<Foo />` 编译为 `React.createElement(Foo)`，并且它正对应于你在 `JavaScript` 文件中定义或导入的组件。

我们建议用大写开头命名组件。如果你的组件以小写字母开头，请在 `JSX` 中使用之前其赋值给大写开头的变量。

例如，下面的代码将无法按预期运行：
```
import React from 'react';

// 错误！组件名应该首字母大写:
function hello(props) {
  // 正确！div 是有效的 HTML 标签:
  return <div>Hello {props.toWhat}</div>;
}

function HelloWorld() {
  // 错误！React 会将小写开头的标签名认为是 HTML 原生标签:
  return <hello toWhat="World" />;
}
```
为了解决这个问题，我们将 `hello` 重命名为 `Hello`，然后使用 `<Hello />` 引用：
```
import React from 'react';

// 正确！组件名应该首字母大写:
function Hello(props) {
  // 正确！div 是有效的 HTML 标签:
  return <div>Hello {props.toWhat}</div>;
}

function HelloWorld() {
  // 正确！React 能够将大写开头的标签名认为是 React 组件。
  return <Hello toWhat="World" />;
}
```