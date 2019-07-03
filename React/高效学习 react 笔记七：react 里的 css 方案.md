## 传统的css方案
#### css in html

直接在 `html` 里面写 `css` 或者用 `link` 引入

像这个样子
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>CSS IN HTML</title>
  <link rel="stylesheet" href="style.css"> // 这个样子
  <style> // 或者这个样子
    body {
      color: red;
    }
  </style>
</head>
<body>

</body>
</html>
```
react 里的 css 方案
####  css in react

也可以像传统的方式那样子引入 `css`，但是得用 `js` 的方法 `inport` 引入

像这样在style.css写好内容以后，在组件内引入即可生效

![](https://upload-images.jianshu.io/upload_images/5780538-38c9f612d22245a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是一但当我们的组件变得复杂时，问题就出现了：

![](https://upload-images.jianshu.io/upload_images/5780538-30dbf9f8839dfea9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我明明只想让 `App` 里面的 `class` 为 `title` 的元素变成红色，但 `Bar` 组件里面的 `title` 也同时被影响了！

##### 解决方法一： 在所有的组件样式前面加一个前缀，防止名字冲突

![](https://upload-images.jianshu.io/upload_images/5780538-71121dad795e8550.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们用 `.App-title` 才编写样式，防止了其他组件的冲突。 

这种方式虽然写起来有点麻烦，但是还好能接受，大不了都加一个前缀。

但是大部分的 `react` 开发者觉得这种写前缀的方式就是很不爽，就是不想写前缀，然而 `css` 已经没有语法支持这么做了，于是他们又创造出了很多 `js` 库用来写 `css`。

##### 解决方法二：使用其他 `css in js` 库(多达几十种)

这里就讲一种 `github` 上 `star` 数量最多的库 `styled-components`

1. 首先引入文件
2. 然后新建一个变量
3. 直接在组件中编写 

![](https://upload-images.jianshu.io/upload_images/5780538-236df0ae60d8f0e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

让我们再来看 `html` 上的结构

![](https://upload-images.jianshu.io/upload_images/5780538-4eb2478bf05c2395.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

自动的加上了随机字符串，避免了想 `class` 名字和加前缀的烦恼

这好像有点强迫症的感觉~，但是现实就是这就是 `react` 世界里目前最流行的一种方案。




