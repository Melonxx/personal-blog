精选的四十五道vue自测题，每五道题公布一次答案。
### 一、
html
```
<div id="app">
  <span ____________???____________>
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>
```
js
```
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```
#####问号处应该填入什么，才能使得 span 的 title 为 message 的值？
A: title="message"
B: :title="message"
C: bind:title="message"
D: v-bind:title="message"
E: title="{{message}}"
F: :title=message
G: v-bind:title=message

### 二、
我们在初始化一个 Vue 应用时，需要首先创建一个 Vue 实例：
```
var vm = new Vue(options)
```
#####其中 options 是一个对象，请问文档中说 options 可以包含哪些 key ？
A: data props propsData computed methods watch
B: el template render renderError
C: beforeCreate created beforeMount mounted beforeUpdate updated activated deactivated beforeDestroy destroyed errorCaptured
D: directives filters components parent mixins extends provide inject
E: name delimiters functional model inheritAttrs comments

### 三、
html
```
<div id="app">
    <span class=span-a>
      {{obj.a}} 
    </span>
    <span class=span-b>
      {{obj.b}}
    </span>
  </div>
```
js
```
var app = new Vue({
  el: '#app',
  data: {
    obj: {
      a: 'a',
    }
  },
})
app.obj.b = 'b'
```
#####请问最终 span-a 和 span-b 中分别展示什么字符串？
A: span-a 中显示a，span-b 中显示 b
B: span-a 中显示a，span-b 中显示 undefined
C: span-a 中显示a，span-b 中不显示

### 四、
html
```
<div id="app">
    <span class=span-a>
      {{obj.a}} 
    </span>
    <span class=span-b>
      {{obj.b}}
    </span>
  </div>
```
js
```
var app = new Vue({
  el: '#app',
  data: {
    obj: {
      a: 'a',
    }
  },
})
app.obj.a = 'a2'
app.obj.b = 'b'
```
#####请问最终 span-a 和 span-b 中分别展示什么字符串？
A: span-a 中显示a2，span-b 中不显示
B: span-a 中显示a2，span-b 中显示b
###五、
#####关于 [Object.freeze()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze "null") 正确的有
A: Object.freeze() 方法可以冻结一个对象，冻结指的是不能向这个对象添加新的属性，不能修改其已有属性的值，不能删除已有属性，以及不能修改该对象已有属性的可枚举性、可配置性、可写性。
B: const object2 = Object.freeze(object1); 其中 object2 和 object1 是同一个对象
C: 被冻结对象有一个属性也是对象，那么这个作为属性的对象也是被冻结的。
>***1.BDFG 2.ABCDE 3.C 4.B 5.AB***
###六、
```
var vm = new Vue({
  el: '#example',
  data: {message: 'hi'}
})
```
#####请问这个 vm 有哪些属性（API）？
A: `$data $props $el $options $parent $root $children $slots $scopedSlots $refs $isServer $attrs $listeners`
B: `$watch $set $delete`
C: `$on $once $off $emit`
D: `$mount $forceUpdate $nextTick $destroy`

###七、
#####关于模板语法说法正确的是
A: Vue.js 使用了基于 XML 的模板语法
B: 在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。
C: 结合响应系统，Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少。
D: 你也可以不用模板，直接写渲染 (render) 函数，使用可选的 JSX 语法。

###八、
```
<form v-on:submit.prevent="onSubmit">...</form>
```
#####请问 .prevent 在 Vue 被称作什么（三个汉字）？

###九、
#####模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让_______???______
请按照文档作答。
模板过重且难以维护
#####所以，对于 Vue 模板中任何复杂逻辑，你都应当使用_______???______
请按照文档作答。

###十、
html
```
<div id="app">
    <span>
      {{computedMessage}} 
      {{computedMessage}} 
      {{computedMessage}} 
    </span>
    <span>
      {{calcMessage()}}
      {{calcMessage()}}
      {{calcMessage()}}
    </span>
  </div>
```
js
```
var app = new Vue({
  el: '#app',
  data: {
    message: 'hi'
  },
  computed:{
    computedMessage(){
      console.log('computed')
      return 'computed ' + this.message
    },
  },
  methods:{
    calcMessage(){
      console.log('methods')
      return 'calc ' + this.message
    }
  }
})
```
##### 问，打印了几次「computed」，几次「methods」
A: 三次 computed，三次 methods
B: 一次 computed，一次 methods
C: 一次 computed，三次 methods
>***6.ABC 7.BCD 8.修饰符 9.计算属性 10.C***
###十一、
html
```
<div id="app">
    <span>
       {{obj.count}}
    </span>
    <div>
      <button @click="obj.count+=1">+1</button>
      <button @click="obj.count-=1">-1</button>
    </div>
    <div>
      你改了 {{modified}} 次
    </div>
  </div>
```
js
```
var app = new Vue({
  el: '#app',
  data: {
    obj:{count: 1},
    modified: 0
  },
  watch:{
    _______???________
      this.modified += 1
    }
  }
})
```
##### 要如何写代码才能监听 obj.count 的变化？
A: obj.count(){
B: obj(){
C: 'obj.count':function(){

###十二、
html
```
<div id="app">
    <span>
       {{obj.a}} {{obj.b}} {{obj.c}}
    </span>
    <div>
      <button @click="obj.a+=1">a+1</button>
      <button @click="obj.b+=1">b+1</button>
      <button @click="obj.c+=1">c+1</button>
    </div>
    <div>
      你改了 {{modified}} 次
    </div>
  </div>
```
js
```
var app = new Vue({
  el: '#app',
  data: {
    modified: 0,
    obj: {a:1,b:2,c:3}
  },
  created(){
      this.$watch('obj', ()=>{
        this.modified += 1
      }, ___________????_________)
  }
})
```
##### 接上题，如果 obj 有 N 个属性，要怎么才能监听所有属性呢？

###十三、
html
```
<div v-bind:style="{ color: activeColor, fontSize: fontSize }"></div>
```
js
```
data: {
  activeColor: 'red',
  fontSize: 30
}
```
result
```
<div style="color: red;"></div>
```
#####请问为什么 fontSize 没有生效？

###十四、
######Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做除了使 Vue 变得非常快之外，还有其它一些好处。
#####这样也不总是符合实际需求，所以 Vue 为你提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 _____??______ 属性即可
###十五、
#####关于 v-if 和 v-show 的描述，正确的有
A: 有 v-show 的元素会被渲染并保留在 DOM 中，v-show 只是简单地切换元素的 CSS 属性 display
B: v-show 不支持 <template> 元素，也不支持 v-else
C: v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
D: v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染
E: 一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。
>***11.C 12.{ deep: true } 13.没有单位 14.key 15.ABCDE***
###十六、
#####关于 v-for，正确的有
A: 可以使用 v-for 来迭代或遍历一个数组： <li v-for="(item, index) in items">
B: 可以使用 v-for 来迭代或遍历一个对象： <li v-for="(value, key, index) in object">
C: v-for 在遍历对象时，是按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下是一致的。

###十七、
代码：[http://jsbin.com/gacokit/8/edit?html,js,output](http://jsbin.com/gacokit/8/edit?html,js,output "null")

上面代码有一个 BUG，进行如下操作就会看到 BUG：

1.  点击3次 add 按钮
2.  点击前两个「点我」按钮，会看到两个 true
3.  删除其中一个 true
4.  发现依然有两个 true

![](./images/vue-examination/vue-examination_(1).gif)

#####请问出现 BUG 的原因是？（说出解决这个 BUG 的关键点即可，答案是一个英文单词）


###十八、
代码 [http://jsbin.com/sidazuy/2/edit?html,js,output](http://jsbin.com/sidazuy/2/edit?html,js,output "null")

上面代码中有一个 BUG：点击 add 后 this.items 变化了，但是页面却没有变化
```
add(){ 
    this.items[this.items.length] = {name:'xxx',clicked:false,id:Math.random()}
    console.log(this.items) //this.items 确实变化了
}
```
##### 请问这是为什么？
A: 由于 JavaScript 的限制，Vue 不能检测到 this.items[this.items.length] 的变化，所以页面没有重新渲染。
B: 如果改成 this.items.push 就可以解决这个 BUG 了，因为 Vue 在 this.items.push 方法上做了一些手脚
C: 使用 this.$set(this.items, this.items.length, ...) 能解决这个 BUG

###十九、
```
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```
#####这段代码的结果是？

A: 页面中显示 0 1 2 3 4 5 6 7 8 9 10
B: 页面中显示 1 2 3 4 5 6 7 8 9 10
C: 页面中显示 1 0

###二十、
代码： [http://jsbin.com/hiyuqak/2/edit?html,js,output](http://jsbin.com/hiyuqak/2/edit?html,js,output "null")
```
<div id="app">
    <table>
        <child v-for="(item,index) in items">
        </child>
    </table>
<button @click=add>add</button>
</div>
```
![](.images/vue-examination/vue-examination_(2).png)

#####点击 add 后发现 child 组件全都位于 table 外面，请问是什么原因？
A: html 中 table 的子元素不能是 child，因此 child 被解析到 table 的外面了
B: 可以使用 <tr is="child" 类似的语法来解决这个问题
>***16.ABC 17.key 18.ABC 19.B 20.AB***
###二十一、
#####Vue 事件处理相关知识中，$event 是什么？
A: $event 是指对应的事件信息
B: 对于原生元素（如 button、input）来说，$event 是原始的 DOM 事件
C: 对于自定义组件（如 child）来说，$event 是其自身 $emit 发出的第一个参数
D: 对于自定义组件（如 child）来说，$event 是其自身 $emit 发出的所有参数

###二十二、
##### Vue 提供的修饰符有
A: 事件修饰符：.stop .prevent .capture .self .once .passive
B: 键盘修饰符：.数字 .enter .tab .delete .esc .space .up .down .left .right .page-down .ctrl .alt .shift .meta .exact 等
C: 鼠标按钮修饰符：.left .right .middle

###二十三、
#####Vue 支持自定义的修饰符吗？[https://github.com/vuejs/vue/issues/6982](https://github.com/vuejs/vue/issues/6982 "null")

A: 支持
B: 不支持

###二十四、
#####Vue 文档中提到的「关注点分离原则」是什么？ [Google一下](https://www.google.com.hk/search?q=%E5%85%B3%E6%B3%A8%E7%82%B9%E5%88%86%E7%A6%BB "null")
A: 是处理复杂性的一个原则。由于关注点混杂在一起会导致复杂性大大增加，所以把不同的关注点分离开来分别处理
B: 关注点分离在计算机科学中，是将计算机程序分隔为不同部分的设计原则。每一部分会有各自的关注焦点。比如网站可以分离为前端和后端，前端的网页又可以分离为 HTML、CSS 和 JS 等。

###二十五、
html
```
<div id="app">
    <input type="text" v-model="value1" :value="value2">
  </div>
```
js
```
var app = new Vue({
  el: '#app',
  data: {
    value1:'1',
    value2:'2'
  },
})
```
##### 请问最终 input 的 value 是多少？
A: 1
B: 2
>***21.ABC 22.ABC 23.B 24.AB 25.A***
###二十六、
```
<select v-model="selected">
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```
#####如果用户选择了该 option，请问 vm.selected 的值为

A: 使用错误，option 的 :value 不能是对象
B: vm.selected 为 123
C: vm.selected 为 "number"
D: vm.selected 为 {"number":123} 对象

###二十七、
在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加` .__?__ `修饰符，从而转变为使用 change 事件进行同步：
```
<!-- 在“change”时而非“input”时更新 -->
<input v-model.__?__="msg" >
```

###二十八、
v-model 其实只是一个语法糖，因为
```
<input v-model="searchText">
```
等价于
```
<input
  v-bind:__A__="searchText"
  v-on:__B____="searchText = _____C______"
>
```
#####请问 A B C 三个空应该分别填入什么？

###二十九、
v-model 用到自定义组件上时，跟 input 稍有不同
```
<custom-input v-model="searchText"></custom-input>
```
等价于
```
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = ____?_____"
></custom-input>
```
#####请问空格处应该填入什么

###三十、
#####Vue.component('button-counter', options) 可以全局注册一个组件，关于这个 options 说法正确的是
A: 因为组件是可复用的 Vue 实例，所以它们与 new Vue 接收**几乎**相同的选项，例如 data、computed、watch、methods 以及生命周期钩子等。
B: 这里的 options 不接受 el
C: 这里的 options.data 选项必须是一个函数
>***26.D 27.lazy 28.value/input/`$event.target.value 29.`$event` 30.ABC***
###三十一、
```
<component v-bind:is="currentTabComponent"></component>
```
#####通过修改 currentTabComponent 的值，我们就可以让 component 在不同组件之间进行动态切换
A: 对
B: 错

###三十二、
*   [命名推荐](https://cn.vuejs.org/v2/style-guide/#%E5%9F%BA%E7%A1%80%E7%BB%84%E4%BB%B6%E5%90%8D-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90 "null")
#####遵循了 W3C 规范中的自定义组件名规则的组件名有
A: xdialog
B: x-dialog
C: xDialog
D: XDialog

###三十三、
#####关于「全局注册」和「局部注册」的描述，正确的有：
A: 一个组件，在全局注册之后，可以用在任何新创建的 Vue 根实例的模板中。
B: 使用 webpack 等构建工具时，全局注册所有的组件意味着即便你已经不再使用一个组件了，它仍然会被包含在你最终的构建结果中。这造成了用户下载的 JavaScript 的无谓的增加。这个时候可以考虑使用局部注册。

###三十四、
props 支持哪些写法？
假设有如下代码：
```
Vue.components('x-component', {
    ... //省略其他选项
    props: ____?_____
})
```
#####请问以下哪些代码可以放在 `____?_____` 处
A: ['title', 'likes', 'isPublished', 'commentIds', 'author']
B: {propA: Number}
C: {propB: [String, Number] }
D: {propC: { type: String, required: true } }
E: {propD: { type: Number, default: 100 }}
F: {propE: { type: Object, default: function () { return { message: 'hello' } } } }

###三十五、
BlogPost 组件的注册
```
Vue.component('blog-post', {
    props: {
        isPublished: Boolean
    }
})
```
html
```
<blog-post is-published></blog-post>
```
#####请问 blog-post 组件得到的 prop.isPublished 的值是多少？
A: undefined
B: true
>***31.A 32.B 33.AB 34.ABCDEF 35.B***
###三十六、
假设 post 只有 id 和 title 两个属性，
那么
```
<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>
```
可以缩写成
```
<blog-post v-bind="post"></blog-post>
```
A: 是的
B: 不行

###三十七、
#####关于单向数据流，对的有
A: 不应该在一个子组件内部改变（父组件传给它的）prop。如果这样做了，Vue 会在浏览器的控制台中发出警告。
B: 有些情况下，我们实在想要改变 prop，可以考虑定义一个本地的 data 属性，并将这个 prop 用作其初始值
C: 有些情况下，我们需要转换 prop 的值。那么可以使用计算属性
D: 假设某个 prop 是一个对象，如 props.user = {name:'frank'}，如果子组件改变了 user.name，那么 Vue 会检测不到这种违反约定的代码，导致父组件的状态受到影响。

###三十八、
假设 bootstrap-date-input 的模板是这样的：
```
<input type="date" class="form-control">
```
假设我是这样使用这个组件的：
```
<bootstrap-date-input type="input" class="hi"></bootstrap-date-input>
```
请问最终得到的 HTML 是什么？
A: <input type="input" class="form-control hi">
B: <input type="input" class="hi">
C: <input type="date" class="form-control hi">
D: <input type="date" class="hi">

###三十九、
接上题，上题中的 type 和 class 会替换或合并到 bootstrap-date-input 组件的根元素上。
#####如果我希望「禁用特性继承」，请问应该使用什么选项？

###四十、
```
<text-document v-bind:title.sync="doc.title"></text-document>
```
等价于
```<text-document
  v-bind:___A___="doc.title"
  v-on:___B___="doc.title = ___C___"
></text-document>
```
#####A、B、C 三处分别填写什么？
>***36.A 37.ABCD 38.A 39.[inheritAttrs: false](https://cn.vuejs.org/v2/guide/components-props.html#%E7%A6%81%E7%94%A8%E7%89%B9%E6%80%A7%E7%BB%A7%E6%89%BF) 40.title/update:title/$event
###四十一、
关于插槽的描述，正确的有

A. 假设有一个组件 `<navigation-link>`，如果 `<navigation-link>` 的 template 里没有包含一个 `<slot>` 元素，则任何传入 `<navigation-link>` 的内容都会被抛弃。
    例如

```
      <navigation-link>
        你好
      </navigation-link>
```

   其中的你好将会被 Vue 删除。
B. 使用具名插槽 `<slot name="xxx'>` 可以实现一个组件拥有多个插槽
C. 可以使用 `<slot>默认内容</slot>` 来指定插槽里的默认内容
D. 可以用 this.$slots 来读取传入的**所有**插槽内容（建议用 [jsbin](http://jsbin.com/wayuri/3/edit?html,js,output "null") 验证一下）
E. 通过设置 slot 属性可以设置内容对应的插槽名字
F. 通过设置 slot-scope 属性，可以从子组件中获取数据

###四十二、
我们之前曾经在一个多标签的界面中使用 is 特性来切换不同的组件：
```
<component v-bind:is="currentTabComponent"></component>
```
当在这些组件之间切换的时候，你有时会想保持这些组件的状态，以避免反复重渲染导致的性能问题。

为了解决这个问题，我们可以用一个 `<_____?_____>` 元素将其动态组件包裹起来。

###四十三、
关于异步组件，正确的有：
A. 异步组件是指，在需要用到它的时候才从服务器加载一个组件
B. 如果项目支持 webpack 2+ 和 ES 2015+，那么可以这样实现异步组件
```
  Vue.component(
      'async-webpack-example',
      () => import('./my-async-component')
  )
```
C. 如果项目支持 webpack 2+ 和 ES 2015+，那么可以这样实现异步组件
```
  new Vue({
      // ...
      components: {
          'my-component': () => import('./my-async-component')
      }
  })
```
D. 根据 Vue 的文档来说，异步组件就是动态组件

###四十四、
这里有一个关于依赖注入的例子：[http://jsbin.com/zuvuhot/3/edit?html,js,output](http://jsbin.com/zuvuhot/3/edit?html,js,output "null")
看完例子后请判断对错
html
```
<div id="app">
    <component-one>
      <component-two>
        <component-three>

        </component-three>
      </component-two>
    </component-one>

  </div>
```
js
```
Vue.component('component-one', {
  template: `
    <div><slot></slot></div>
  `,
  provide:{
    provide1(){
      console.log('这是component-one提供的函数')
    }
  }
})
Vue.component('component-two', {
  template: `
    <div><slot></slot></div>
  `,

})
Vue.component('component-three', {
  template: `
    <button @click="provide1">Click me</button>
  `,
  inject: ['provide1']
})

var app = new Vue({
  el: '#app',

})
```
A: 点击 component-three 里的 Click me 按钮，将产生报错，因为这个组件没有这个 method
B: 点击 component-three 里的 Click me 按钮，将不会报错，因为这个组件通过依赖注入（provide/inject）得到了 provide1 方法

###四十五、
#####关于事件 API，正确的有
A: 我们可以通过 `$on(eventName, eventHandler)` 侦听一个事件
B: 我们可以通过 `$once(eventName, eventHandler)` 一次性侦听一个事件
C: 我们可以通过 `$off(eventName, eventHandler)` 停止侦听一个事件
D: 我们可以通过 `$trigger(eventName,eventData)` 触发一个事件
>***41.ABCEF 42.keep-alive 43.ABC 44.B 45.ABC***

>[来源](https://xiedaimala.com/courses/0d531a6f-40a7-4120-a8f6-9e816ff9d51c#/common)
>有疑惑的小胖友，欢迎在底下留言哦~

