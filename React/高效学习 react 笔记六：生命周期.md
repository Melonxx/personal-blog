先来看React v16.3之前的生命周期函数（图中实际上少了componentDidCatch)，如下图。

![](https://upload-images.jianshu.io/upload_images/5780538-ac6bc792f35118b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

constructor
componentWillMount
render
componentDidMount
componentShouldUpdate
componentWillUpdate
render
componentDidUpdate
componentWillUnmount
componentWillReceiveProps

所以，React v16.3之后的生命周期函数一览图成了这样。

![](https://upload-images.jianshu.io/upload_images/5780538-54ae280cf65102cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

总结一下：

用一个静态函数getDerivedStateFromProps来取代被deprecate的几个生命周期函数，就是强制开发者在render之前只做无副作用的操作，而且能做的操作局限在根据props和state决定新的state，而已。
