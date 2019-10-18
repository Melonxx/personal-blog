#### [上文](https://www.jianshu.com/p/6acecc5ef433)我们已经用自己的 `eventHub` 来完成了任意组件之间的通讯，接下来我们使用 `redux` 来达到相同的功能。

直接看官网的例子，我们直接开始改写[这样](http://js.jirengu.com/pisak/6/edit?js,output)

![image.png](https://upload-images.jianshu.io/upload_images/5780538-ba46b6efd526e1a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> 可以看到思路和我们的 `eventHub` 差不多，只不过 `redux` 使用了更多新名词（吐槽...）订阅事件用 `subscribe` ， 触发事件用 `dispatch`，触发事件的名字用 `type` ，传递的参数为 `payload`。

虽然我们现在已经用 `redux` 来改写了这个例子，但是这个方法还是不够优雅，

我们在改变数据的时候，还是需要数据一层一层的往下传，对比于最早的 `props` 只是少了一层一层往上传而已，但是这始终还是没有解决问题。

#### 所以为了解决这个问题，于是我们现在要使用 **react-redux** 再来改造这个例子

整体依旧不变，我们只是为了解决一层一层往下传递的问题，所以我们只需要用 `Provider` 把 `App` 包起来，在用 `connect` 连接子组件即可。

[完整结果看这里](http://js.jirengu.com/xusoh/6/edit?js,output)

![](https://upload-images.jianshu.io/upload_images/5780538-fbc8296ae5afd13c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>对比用 `redux` ，`react-redux` 不需要在额外去订阅 `render` 函数，不需要再通过 `props` 一层一层的去传递数据，只需要用 `Provider` 将 `App` 组件包装，再用 `connect` 将子组件包装即可使用。 

在这个栗子中我们得以看出：

> 我们通过 `<ReactRedux.Provider store={store}><App /></ReactRedux.Provider>` 把 `App` 包起来之后，我们只需再在组件内使用 `ReactRedux.connect` 这个 `API`，然后在第一个括号里，传入 `mapStateToProps` 和 `mapDispatchToProps` 这两个变量以后，我们就能直接在组建内使用 `props.数据` 使用，这样就避免了最初通过 `props` 一层一层的传递数据。

ps：又多学了两个 `API` ：）（手动狗头）


