## 编写灵活、稳定、高质量的 HTML 和 CSS 代码的规范。

---------------------

# ------HTML------

### 语法
*   用两个空格来代替制表符（tab） -- 这是唯一能保证在所有环境下获得一致展现的方法（不过现在的编辑器都可以改tab为两个空格）。
*   嵌套元素应当缩进一次（即两个空格）。
*   对于属性的定义，确保全部使用双引号，绝不要使用单引号。
*   不要在自闭合（self-closing）元素的尾部添加斜线 -- [HTML5 规范](http://dev.w3.org/html5/spec-author-view/syntax.html#syntax-start-tag)中明确说明这是可选的。
*   不要省略可选的结束标签（closing tag）（例如，`</li>` 或 `</body>`）。
```
<!DOCTYPE html>
<html>
  <head>
    <title>Page title</title>
  </head>
  <body>
    <img src="images/company-logo.png" alt="Company">
    <h1 class="hello-world">Hello, world!</h1>
  </body>
</html>
```

### 语言属性
根据 HTML5 规范：

> 强烈建议为 html 根元素指定 lang 属性，从而为文档设置正确的语言。这将有助于语音合成工具确定其所应该采用的发音，有助于翻译工具确定其翻译时所应遵守的规则等等。

更多关于 `lang` 属性的知识可以从 [此规范](http://www.w3.org/html/wg/drafts/html/master/semantics.html#the-html-element) 中了解。

这里列出了[语言代码表](http://reference.sitepoint.com/html/lang-codes)。


```
//推荐使用属性值 cmn-Hans-CN（简体, 中国大陆），但是考虑浏览器和操作系统的兼容性，目前仍然使用 zh-CN 属性值
<html lang="zh-CN">
  <!-- ... -->
</html>
```
###### 更多关于 Languages Tags ：

[W3C Language tags in HTML and XML](http://www.w3.org/International/articles/language-tags/)
[网页头部的声明应该是用 lang=”zh” 还是 lang=”zh-cn”？](http://www.zhihu.com/question/20797118?utm_source=weibo&utm_medium=weibo_share&utm_content=share_question&utm_campaign=share_sidebar)

###HTML代码大小写
HTML标签名、类名、标签属性和大部分属性值统一用小写
```
/* Bad */
<div class="DEMO"></div>
<DIV CLASS="DEMO"></DIV>

/* Good*/
<div class="demo"></div>
```
###特殊字符引用
文本可以和字符引用混合出现。这种方法可以用来转义在文本中不能合法出现的字符。

在 HTML 中不能使用小于号 “<” 和大于号 “>”特殊字符，浏览器会将它们作为标签解析，若要正确显示，在 HTML 源代码中使用字符实体
```
/* Bad */
<a href="#">more>></a>

/* Good*/
<a href="#">more&gt;&gt;</a>
```
更多关于符号引用：[#Character references](http://www.w3.org/TR/html5/syntax.html#character-references)
###纯数字输入框
使用 `type="tel"` 而不是 `type="number"`
```
<input type="tel">
```
### IE 兼容模式

IE 支持通过特定的 `<meta>` 标签来确定绘制当前页面所应该采用的 IE 版本。除非有强烈的特殊需求，否则最好是设置为 **edge mode**，从而通知 IE 采用其所支持的最新的模式。

[阅读这篇 stack overflow 上的文章](http://stackoverflow.com/questions/6771258/whats-the-difference-if-meta-http-equiv-x-ua-compatible-content-ie-edge-e)可以获得更多有用的信息。
```
<meta http-equiv="X-UA-Compatible" content="IE=Edge">
```
### 字符编码(一般编辑器都能自动生成~)
通过明确声明字符编码，能够确保浏览器快速并容易的判断页面内容的渲染方式。这样做的好处是，可以避免在 HTML 中使用字符实体标记（character entity），从而全部与文档编码一致（一般采用 UTF-8 编码）。
```
<head>
  <meta charset="UTF-8">
</head>
```
###单行注释
一般用于简单的描述，如某些状态描述、属性描述等

注释内容前后各一个空格字符，注释位于要注释代码的上面，单独占一行
```
/* Bad */
<div>...</div><!-- Comment Text -->	
	
<div><!-- Comment Text -->
    ...
</div>


/* Good*/
<!-- Comment Text -->
<div>...</div>
```
### 模块注释
一般用于描述模块的名称以及模块开始与结束的位置

注释内容前后各一个空格字符，<!-- S Comment Text --> 表示模块开始，<!-- E Comment Text --> 表示模块结束，模块与模块之间相隔一行
```
/* Bad */
<!-- S Comment Text A -->
<div class="mod_a">
    ...
</div>
<!-- E Comment Text A -->
<!-- S Comment Text B -->	
<div class="mod_b">
    ...
</div>
<!-- E Comment Text B -->


/* Good*/
<!-- S Comment Text A -->	
<div class="mod_a">
    ...
</div>
<!-- E Comment Text A -->
	
<!-- S Comment Text B -->	
<div class="mod_b">
    ...
</div>
<!-- E Comment Text B -->
```
### 嵌套模块注释
当模块注释内再出现模块注释的时候，为了突出主要模块，嵌套模块不再使用
```
<!-- S Comment Text -->
<!-- E Comment Text -->
而改用
<!-- /Comment Text -->
注释写在模块结尾标签底部，单独一行。
<!-- S Comment Text A -->
<div class="mod_a">
		
    <div class="mod_b">
        ...
    </div>
    <!-- /mod_b -->
    	
    <div class="mod_c">
    	...
    </div>
    <!-- /mod_c -->
		
</div>
<!-- E Comment Text A -->
```
### 引入 CSS 和 JavaScript 文件

根据 HTML5 规范，在引入 CSS 和 JavaScript 文件时一般不需要指定 `type` 属性，因为 `text/css` 和 `text/javascript` 分别是它们的默认值。

##### HTML5 spec links

*   [Using link](http://www.w3.org/TR/2011/WD-html5-20110525/semantics.html#the-link-element)
*   [Using style](http://www.w3.org/TR/2011/WD-html5-20110525/semantics.html#the-style-element)
*   [Using script](http://www.w3.org/TR/2011/WD-html5-20110525/scripting-1.html#the-script-element)
```
<!-- External CSS -->
<link rel="stylesheet" href="code-guide.css">

<!-- In-document CSS -->
<style>
  /* ... */
</style>

<!-- JavaScript -->
<script src="code-guide.js"></script>
```
### 实用为王
尽量遵循 HTML 标准和语义，但是不要以牺牲实用性为代价。任何时候都要尽量使用最少的标签并保持最小的复杂度。
属性顺序
HTML 属性应当按照以下给出的顺序依次排列，确保代码的易读性。

* class
* id, name
* data-*
* src, for, type, href, value
* title, alt
* role, aria-*

class 用于标识高度可复用组件，因此应该排在首位。id 用于标识具体组件，应当谨慎使用（例如，页面内的书签），因此排在第二位。
```
<a class="..." id="..." data-toggle="modal" href="#">
  Example link
</a>

<input class="form-control" type="text">

<img src="..." alt="...">
```
### 布尔（boolean）型属性

布尔型属性可以在声明时不赋值。XHTML 规范要求为其赋值，但是 HTML5 规范不需要。

更多信息请参考 [WhatWG section on boolean attributes](http://www.whatwg.org/specs/web-apps/current-work/multipage/common-microsyntaxes.html#boolean-attributes)：

> 元素的布尔型属性如果有值，就是 true，如果没有值，就是 false。

如果*一定*要为其赋值的话，请参考 WhatWG 规范：

> 如果属性存在，其值必须是空字符串或 [...] 属性的规范名称，并且不要在首尾添加空白符。

**简单来说，就是不用赋值。**
```
<input type="text" disabled>

<input type="checkbox" value="1" checked>

<select>
  <option value="1" selected>1</option>
</select>
```
### JavaScript 生成的标签
通过 JavaScript 生成的标签让内容变得不易查找、编辑，并且降低性能。能避免时尽量避免。

---------------------
# ------图片------
常见的图片格式有 GIF、PNG8、PNG24、JPEG、WEBP，根据图片格式的特性和场景需要选取适合的图片格式。

### [](https://guide.aotu.io/docs/image/format.html#%E5%86%85%E5%AE%B9%E5%9B%BE "内容图")内容图

内容图多以商品图等照片类图片形式存在，颜色较为丰富，文件体积较大

*   优先考虑 JPEG 格式，条件允许的话优先考虑 WebP 格式
*   尽量不使用PNG格式，PNG8 色位太低，PNG24 压缩率低，文件体积大

### [](https://guide.aotu.io/docs/image/format.html#%E8%83%8C%E6%99%AF%E5%9B%BE "背景图")背景图

背景图多为图标等颜色比较简单、文件体积不大、起修饰作用的图片

*   PNG 与 GIF 格式，优先考虑使用 PNG 格式,PNG格式允许更多的颜色并提供更好的压缩率
*   图像颜色比较简单的，如纯色块线条图标，优先考虑使用 PNG8 格式，避免不使用 JPEG 格式
*   图像颜色丰富而且图片文件不太大的（40KB 以下）或有半透明效果的优先考虑 PNG24 格式
*   图像颜色丰富而且文件比较大的（40KB - 200KB）优先考虑 JPEG 格式
*   条件允许的，优先考虑 WebP 代替 PNG 和 JPEG 格式

### 图片大小
中国普通家庭的宽带基本能达到8Mbps，实际速率大约为500—900KB/s，全国3G/4G用户占有比超过了50%，为了保证图片能更好地加载展示给用户看，团队约定：

**PC平台单张的图片的大小不应大于 200KB。**
**移动平台单张的图片的大小不应大于 100KB。**
*（图片的大小约定标准随全国网速的改变而改变）*

### 图片质量

*   上线的图片都应该经过压缩处理，压缩后的图片不应该出现肉眼可感知的失真区域
*   60质量的JPEG格式图片与质量大于60的相比，肉眼已看不出明显的区别，因此保存 JPEG 图的时候，质量一般控制在60，若保真度要求高的图片可适量提高到 80，图片大小控制在 200KB 以内

###图片引入
HTML 中图片引入不需添加 width、height 属性，alt 属性应该写上
CSS 中图片引入不需要引号
```
/* Bad */
<img src="" width="" height="" >
// CSS 中图片引入不需要引号
.jdc {
    background-image: url(icon.png);
}

/* Good*/
<img src="" alt="" >
```
### CSS Sprites 使用建议
* 适合使用频率高更新频率低的小图标
* 尽量不留太多的空白
* 体积较大的图片不合并
* 确保要合并的小图坐标数值和合并后的 Sprites 图尺寸均为偶数
### Data URIs（base64编码）使用建议
* 适合更新频率高的小图片，如某些具备自定义功能的标题icon等
* 转换成 Base64 编码的图片应小于 2KB
* 移动端不使用 Base64 编码
* 要兼容 IE6/IE7 的不使用
---------------------

# ------CSS------
### 语法

*   用两个空格来代替制表符（tab） -- 这是唯一能保证在所有环境下获得一致展现的方法。
*   为选择器分组时，将单独的选择器单独放在一行。
*   为了代码的易读性，在每个声明块的左花括号前添加一个空格。
*   声明块的右花括号应当单独成行。
*   每条声明语句的 `:` 后应该插入一个空格。
*   为了获得更准确的错误报告，每条声明都应该独占一行。
*   所有声明语句都应当以分号结尾。最后一条声明语句后面的分号是可选的，但是，如果省略这个分号，你的代码可能更易出错。
*   对于以逗号分隔的属性值，每个逗号后面都应该插入一个空格（例如，`box-shadow`）。
*   不要在 `rgb()`、`rgba()`、`hsl()`、`hsla()` 或 `rect()` 值的*内部*的逗号后面插入空格。这样利于从多个属性值（既加逗号也加空格）中区分多个颜色值（只加逗号，不加空格）。
*   对于属性值或颜色参数，省略小于 1 的小数前面的 0 （例如，`.5` 代替 `0.5`；`-.5px` 代替 `-0.5px`）。
*   十六进制值应该全部小写，例如，`#fff`。在扫描文档时，小写字符易于分辨，因为他们的形式更易于区分。
*   尽量使用简写形式的十六进制值，例如，用 `#fff` 代替 `#ffffff`。
*   为选择器中的属性添加双引号，例如，`input[type="text"]`。[只有在某些情况下是可选的](http://mathiasbynens.be/notes/unquoted-attribute-values#css)，但是，为了代码的一致性，建议都加上双引号。
*   避免为 0 值指定单位，例如，用 `margin: 0;` 代替 `margin: 0px;`。

对于这里用到的术语有疑问吗？请参考 Wikipedia 上的 [syntax section of the Cascading Style Sheets article](http://en.wikipedia.org/wiki/Cascading_Style_Sheets#Syntax)。
```
/* Bad CSS */
.selector, .selector-secondary, .selector[type=text] {
  padding:15px;
  margin:0px 0px 15px;
  background-color:rgba(0, 0, 0, 0.5);
  box-shadow:0px 1px 2px #CCC,inset 0 1px 0 #FFFFFF
}

/* Good CSS */
.selector,
.selector-secondary,
.selector[type="text"] {
  padding: 15px;
  margin-bottom: 15px;
  background-color: rgba(0,0,0,.5);
  box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;
}
```
### 声明顺序

相关的属性声明应当归为一组，并按照下面的顺序排列：

* 1.  Positioning
* 2.  Box model
* 3.  Typographic
* 4.  Visual

由于定位（positioning）可以从正常的文档流中移除元素，并且还能覆盖盒模型（box model）相关的样式，因此排在首位。盒模型排在第二位，因为它决定了组件的尺寸和位置。

其他属性只是影响组件的*内部（inside）*或者是不影响前两组属性，因此排在后面。

完整的属性列表及其排列顺序请参考 [Recess](http://twitter.github.com/recess)。
```
.declaration-order {
  /* Positioning */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  /* Box-model */
  display: block;
  float: right;
  width: 100px;
  height: 100px;

  /* Typography */
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;

  /* Visual */
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;

  /* Misc */
  opacity: 1;
}
```
### 不要使用 `@import`

与 `<link>` 标签相比，`@import` 指令要慢很多，不光增加了额外的请求次数，还会导致不可预料的问题。替代办法有以下几种：

*   使用多个 `<link>` 元素
*   通过 Sass 或 Less 类似的 CSS 预处理器将多个 CSS 文件编译为一个文件
*   通过 Rails、Jekyll 或其他系统中提供过 CSS 文件合并功能

请参考 [Steve Souders 的文章](http://www.stevesouders.com/blog/2009/04/09/dont-use-import/)了解更多知识。
```
<!-- Use link elements -->
<link rel="stylesheet" href="core.css">

<!-- Avoid @imports -->
<style>
  @import url("more.css");
</style>
```
###媒体查询（Media query）的位置
将媒体查询放在尽可能相关规则的附近。不要将他们打包放在一个单一样式文件中或者放在文档底部。如果你把他们分开了，将来只会被大家遗忘。下面给出一个典型的实例。
```
.element { ... }
.element-avatar { ... }
.element-selected { ... }

@media (min-width: 480px) {
  .element { ...}
  .element-avatar { ... }
  .element-selected { ... }
}
```
###带前缀的属性
当使用特定厂商的带有前缀的属性时，通过缩进的方式，让每个属性的值在垂直方向对齐，这样便于多行编辑。
```
/* Prefixed properties */
.selector {
  -webkit-box-shadow: 0 1px 2px rgba(0,0,0,.15);
          box-shadow: 0 1px 2px rgba(0,0,0,.15);
}
```
### 单行规则声明
对于只包含一条声明的样式，为了易读性和便于快速编辑，建议将语句放在同一行。对于带有多条声明的样式，还是应当将声明分为多行。

这样做的关键因素是为了错误检测 -- 例如，CSS 校验器指出在 183 行有语法错误。如果是单行单条声明，你就不会忽略这个错误；如果是单行多条声明的话，你就要仔细分析避免漏掉错误了。
```
/* Single declarations on one line */
.span1 { width: 60px; }
.span2 { width: 140px; }
.span3 { width: 220px; }

/* Multiple declarations, one per line */
.sprite {
  display: inline-block;
  width: 16px;
  height: 15px;
  background-image: url(../img/sprite.png);
}
.icon           { background-position: 0 0; }
.icon-home      { background-position: 0 -20px; }
.icon-account   { background-position: 0 -40px; }
```
### 简写形式的属性声明

在需要显示地设置所有值的情况下，应当尽量限制使用简写形式的属性声明。常见的滥用简写属性声明的情况如下：

*   `padding`
*   `margin`
*   `font`
*   `background`
*   `border`
*   `border-radius`

大部分情况下，我们不需要为简写形式的属性声明指定所有值。例如，HTML 的 heading 元素只需要设置上、下边距（margin）的值，因此，在必要的时候，只需覆盖这两个值就可以。过度使用简写形式的属性声明会导致代码混乱，并且会对属性值带来不必要的覆盖从而引起意外的副作用。

在 MDN（Mozilla Developer Network）上一篇非常好的关于[shorthand properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Shorthand_properties) 的文章，对于不太熟悉简写属性声明及其行为的用户很有用。
```
/* Bad example */
.element {
  margin: 0 0 10px;
  background: red;
  background: url("image.jpg");
  border-radius: 3px 3px 0 0;
}

/* Good example */
.element {
  margin-bottom: 10px;
  background-color: red;
  background-image: url("image.jpg");
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
}
```
### Less 和 Sass 中的嵌套

避免不必要的嵌套。这是因为虽然你可以使用嵌套，但是并不意味着应该使用嵌套。只有在必须将样式限制在父元素内（也就是后代选择器），并且存在多个需要嵌套的元素时才使用嵌套。

扩展阅读：

*   [Nesting in Sass and Less](http://markdotto.com/2015/07/20/css-nesting/)
```
// Without nesting
.table > thead > tr > th { … }
.table > thead > tr > td { … }

// With nesting
.table > thead > tr {
  > th { … }
  > td { … }
}
```
### Less 和 Sass 中的操作符
为了提高可读性，在圆括号中的数学计算表达式的数值、变量和操作符之间均添加一个空格。
```
// Bad example
.element {
  margin: 10px 0 @variable*2 10px;
}

// Good example
.element {
  margin: 10px 0 (@variable * 2) 10px;
}
```

### class 命名
class 名称中只能出现小写字符和破折号（dashe）（不是下划线，也不是驼峰命名法）。破折号应当用于相关 class 的命名（类似于命名空间）（例如，`.btn` 和 `.btn-danger`）。
避免过度任意的简写。`.btn` 代表 *button*，但是 `.s` 不能表达任何意思。
class 名称应当尽可能短，并且意义明确。
使用有意义的名称。使用有组织的或目的明确的名称，不要使用表现形式（presentational）的名称。
基于最近的父 class 或基本（base） class 作为新 class 的前缀。
使用 `.js-*` class 来标识行为（与样式相对），并且不要将这些 class 包含到 CSS 文件中。
在为 Sass 和 Less 变量命名时也可以参考上面列出的各项规范。
```
/* Bad example */
.t { ... }
.red { ... }
.header { ... }

/* Good example */
.tweet { ... }
.important { ... }
.tweet-header { ... }
```
### 选择器

*   对于通用元素使用 class ，这样利于渲染性能的优化。
*   对于经常出现的组件，避免使用属性选择器（例如，`[class^="..."]`）。浏览器的性能会受到这些因素的影响。
*   选择器要尽可能短，并且尽量限制组成选择器的元素个数，建议不要超过 3 。
*   **只有**在必要的时候才将 class 限制在最近的父元素内（也就是后代选择器）（例如，不使用带前缀的 class 时 -- 前缀类似于命名空间）。

扩展阅读：

*   [Scope CSS classes with prefixes](http://markdotto.com/2012/02/16/scope-css-classes-with-prefixes/)
*   [Stop the cascade](http://markdotto.com/2012/03/02/stop-the-cascade/)
```
/* Bad example */
span { ... }
.page-container #stream .stream-item .tweet .tweet-header .username { ... }
.avatar { ... }

/* Good example */
.avatar { ... }
.tweet-header .username { ... }
.tweet .avatar { ... }
```
### 代码组织
* 以组件为单位组织代码段。
* 制定一致的注释规范。
* 使用一致的空白符将代码分隔成块，这样利于扫描较大的文档。
* 如果使用了多个 CSS 文件，将其按照组件而非页面的形式分拆，因为页面会被重组，而组件只会被移动。
```
/*
 * Component section heading
 */

.element { ... }


/*
 * Component section heading
 *
 * Sometimes you need to include optional context for the entire component. Do that up here if it's important enough.
 */

.element { ... }

/* Contextual sub-component or modifer */
.element-heading { ... }
```
----------------------------
# 命名规范
### 图片命名
**图片命名建议以以下顺序命名：**
图片业务（可选） +（mod_）图片功能类别（必选）+ 图片模块名称（可选） + 图片精度（可选）

- 图片业务：
pp_：拍拍
wx_：微信
sq_：手Q
jd_：京东商城

- 图片功能类别：
mod_：是否公共，可选
icon：模块类固化的图标
logo：LOGO类
spr：单页面各种元素合并集合
btn：按钮
bg：可平铺或者大背景

- 图片模块名称：
goodslist：商品列表
goodsinfo：商品信息
userava tar：用户头像

- 图片精度：
普清：@1x
Retina：@2x | @3x

如下面例子：
>公共模块：
wx_mod_btn_goodlist@2x.png
wx_mod_btn_goodlist.png
mod_btn_goodlist.png 
>
>非公共模块：
wx_btn_goodlist@2x.png
wx_btn_goodlist.png
btn_goodlist.png

### HTML/CSS文件命名

确保文件命名总是以字母开头而不是数字，且字母一律小写，以下划线连接且不带其他标点符号，如：
```
<!-- HTML -->
jdc.html
jdc_list.html
jdc_detail.html

<!-- SASS -->
jdc.scss
jdc_list.scss
jdc_detail.scss
```
### ClassName命名

祖先模块不能出现下划线，除了是全站公用模块，如 mod_ 系列的命名
```
/* Bad */
<div class="modulename_info">
	<div class="modulename_info_son"></div>
	<div class="modulename_info_son"></div>
	...		
</div>


/* Good*/
<div class="modulename">
	<div class="modulename_info">
		<div class="modulename_son"></div>
		<div class="modulename_son"></div>
		...
	</div>
</div>
<!-- 这个是全站公用模块，祖先模块允许直接出现下划线 -->
<div class="mod_info">
	<div class="mod_info_son"></div>
	<div class="mod_info_son"></div>
	...		
</div>
```
在子孙模块数量可预测的情况下，严格继承祖先模块的命名前缀

><div class="modulename">
>　　<div class="modulename_cover"></div>
>　　<div class="modulename_info"></div>
></div>

当子孙模块超过4级或以上的时候，可以考虑在祖先模块内具有识辨性的独立缩写作为新的子孙模块
```
/* Bad */
<div class="modulename">
	<div class="modulename_cover"></div>
	<div class="modulename_info">
    	<div class="modulename_info_user">
    		<div class="modulename_info_user_img">
    			<img src="" alt="">
    			<div class="modulename_info_user_img_tit"></div>
    			<div class="modulename_info_user_img_txt"></div>
    			...
    		</div>
    	</div>
    	<div class="modulename_info_list"></div>
	</div>
</div>


/* Good*/
<div class="modulename">
	<div class="modulename_cover"></div>
	<div class="modulename_info">
    	<div class="modulename_info_user">
    		<div class="modulename_info_user_img">
    			<img src="" alt="">
    			<!-- 这个时候 miui 为 modulename_info_user_img 首字母缩写-->
    			<div class="miui_tit"></div>
    			<div class="miui_txt"></div>
    			...
    		</div>
    	</div>
    	<div class="modulename_info_list"></div>
	</div>
</div>
```
##### 模块命名
全站公共模块：以 mod_ 开头
```
<div class="mod_yours"></div>
```
业务公共模块：以 业务名_mod_ 开头
```
<div class="paipai_mod_yours"></div>
```
##### 常用命名推荐
***注意：ad、banner、gg、guanggao 等有机会和广告挂勾的字眠不建议直接用来做ClassName，因为有些浏览器插件（Chrome的广告拦截插件等）会直接过滤这些类名，因此***

><div class="ad"></div>　　　// 这种广告的英文或拼音类名不应该出现

**栗子🌰**
ClassName|	含义
--|--
about	|关于
account	|账户
arrow	|箭头图标
article	|文章
aside	|边栏
audio	|音频
avatar	|头像
bg,background	|背景
bar	|栏（工具类）
branding	|品牌化
crumb,breadcrumbs	|面包屑
btn,button	|按钮
caption	|标题，说明
category	|分类
chart	|图表
clearfix	|清除浮动
close	|关闭
col,column	|列
comment	|评论
community	|社区
container|	容器
content	|内容
copyright	|版权
current	|当前态，选中态
default	|默认
description	|描述
details	|细节
disabled	|不可用
entry	|文章，博文
error	|错误
even	|偶数，常用于多行列表或表格中
fail	|失败（提示）
feature	|专题
fewer	|收起
field	|用于表单的输入区域
figure	|图
filter	|筛选
first	|第一个，常用于列表中
footer	|页脚
forum	|论坛
gallery	|画廊
group	|模块，清除浮动
header	|页头
help	|帮助
hide	|隐藏
hightlight	|高亮
home	|主页
icon|	图标
info,information	|信息
last	|最后一个，常用于列表中
links	|链接
login	|登录
logout	|退出
logo	|标志
main	|主体
menu	|菜单
meta	|作者、更新时间等信息栏，一般位于标题之下
module	|模块
more	|更多（展开）
msg,message	|消息
nav,navigation	|导航
next	|下一页
nub	|小块
odd	|奇数，常用于多行列表或表格中
off	|鼠标离开
on	|鼠标移过
output	|输出
pagination	|分页
pop,popup	|弹窗
preview	|预览
previous	|上一页
primary	|主要
progress	|进度条
promotion	|促销
rcommd,recommendations|	推荐
reg,register	|注册
save	|保存
search	|搜索
secondary	|次要
section	|区块
selected	|已选
share	|分享
show	|显示
sidebar	|边栏，侧栏
slide	|幻灯片，图片切换
sort	|排序
sub	|次级的，子级的
submit	|提交
subscribe	|订阅
subtitle	|副标题
success	|成功（提示）
summary	|摘要
tab	|标签页
table	|表格
txt,text	|文本
thumbnail	|缩略图
time|	时间
tips	|提示
title|	标题
video	|视频
wrap	|容器，包，一般用于最外层
wrapper	|容器，包，一般用于最外层

-----------------------------

#JS
## 编码规范
### 单行代码块
在单行代码块中使用空格
```
/* Bad */
function foo () {return true}
if (foo) {bar = 0}


/* Good*/
function foo () { return true }
if (foo) { bar = 0 }
```
### 大括号风格
在编程过程中，大括号风格与缩进风格紧密联系，用来描述大括号相对代码块位置的方法有很多。在 JavaScript 中，主要有三种风格，如下：
- One True Brace Style
```
if (foo) {
  bar()
} else {
  baz()
}
```
- Stroustrup
```
if (foo) {
  bar()
}
else {
  baz()
}
```
- Allman
```
if (foo)
{
  bar()
}
else
{
  baz()
}
```
> 个人喜欢使用 `One True Brace Style `风格
### 变量命名
当命名变量时，主流分为驼峰式命名（variableName）和下划线命名（variable_name）两大阵营。
> 个人喜欢使用下划线命名 =。=

### 拖尾逗号
在 ECMAScript5 里面，对象字面量中的拖尾逗号是合法的，但在 IE8（非 IE8 文档模式）下，当出现拖尾逗号，则会抛出错误。

拖尾逗号的例子：
```
var foo = {
  name: 'foo',
  age: '22',
}
```
拖尾逗号的好处是，简化了对象和数组添加或删除元素，我们只需要修改新增的行即可，并不会增加差异化的代码行数。
>因为拖尾逗号有好也有不好，所以团队约定允许在最后一个元素或属性与闭括号 ] 或 } 在不同行时，可以（但不要求）使用拖尾逗号。当在同一行时，禁止使用拖尾逗号。

### 逗号空格
逗号前后的空格可以提高代码的可读性，团队约定在逗号后面使用空格，逗号前面不加空格.
```
/* Bad */
var foo = 1,bar = 2
var foo = 1 , bar = 2
var foo = 1 ,bar = 2


/* Good*/
var foo = 1, bar = 2
```
### 逗号风格
逗号分隔列表时，在 JavaScript 中主要有两种逗号风格：

- 标准风格，逗号放置在当前行的末尾
- 逗号前置风格，逗号放置在下一行的开始位置
```
/* Bad */
var foo = 1
,
bar = 2

var foo = 1
, bar = 2

var foo = ['name'
          , 'age']


/* Good*/
var foo = 1,
    bar = 2

var foo = ['name',
            'age']
```
>emmmmmm，还是使用标准风格看起来顺眼~
### 计算属性的空格
团队约定在对象的计算属性内，禁止使用空格
```
/* Bad */
obj['foo' ]
obj[ 'foo']
obj[ 'foo' ]


/* Good*/
obj['foo']
```
### 函数调用
为了避免语法错误，团队约定在函数调用时，禁止使用空格
```
/* Bad */
fn ()
fn
()


/* Good*/
fn()
```
### 对象字面量的键值缩进
团队约定对象字面量的键和值之间不能存在空格，且要求对象字面量的冒号和值之间存在一个空格
```
/* Bad */
var obj = { 'foo' : 'haha' }


/* Good*/
var obj = { 'foo': 'haha' }
```
### 构造函数首字母大写
在 JavaScript 中 `new` 操作符用来创建某个特定类型的对象的一个实例，该类型的对象是由一个构造函数表示的。由于构造函数只是常规函数，唯一区别是使用 `new` 来调用。所以我们团队约定构造函数的首字母要大小，以此来区分构造函数和普通函数。
```
/* Bad */
var fooItem = new foo()


/* Good*/
var fooItem = new Foo()
```
### 构造函数的参数
在 JavaScript 中，通过 `new` 调用构造函数时，如果不带参数，可以省略后面的圆括号。但这样会造成与整体的代码风格不一致，所以团队约定使用圆括号
```
/* Bad */
var person = new Person


/* Good*/
var person = new Person()
```
### 链式调用
链式调用如果放在同一行，往往会造成代码的可读性差，但有些时候，短的链式调用并不会影响美观。所以本规范约定一行最多只能有四个链式调用，超过就要求换行。
### 空行
空白行对于分离代码逻辑有帮助，但过多的空行会占据屏幕的空间，影响可读性。团队约定最大连续空行数为 2
```
/* Bad */
var a = 1



var b = 2


/* Good*/
var a = 1


var b = 2
```
### 函数声明的空格
当格式化一个函数，函数名或 function 关键字与左括号之间允许有空白。命名函数要求函数名和 function 关键字之间有空格，但是匿名函数要求不加空格。
>团队约定函数括号前要加空格
```
/* Bad */
function func(x) {
  // ...
}


/* Good*/
function func (x) {
  // ...
}
```
### 操作符的空格
团队约定操作符前后都需要添加空格
```
/* Bad */
var sum = 1+2



/* Good*/
var sum = 1 + 2
```


-----------------------------
# 编辑器配置

将你的编辑器按照下面的配置进行设置，以避免常见的代码不一致和差异：

*   用两个空格代替制表符（soft-tab 即用空格代表 tab 符）。
*   保存文件时，删除尾部的空白符。
*   设置文件编码为 UTF-8。
*   在文件结尾添加一个空白行。

参照文档并将这些配置信息添加到项目的 `.editorconfig` 文件中。例如：[Bootstrap 中的 .editorconfig 实例](https://github.com/twbs/bootstrap/blob/master/.editorconfig)。更多信息请参考 [about EditorConfig](http://editorconfig.org/)。




