今天在做订单中心的时候，ios端出现如下时间错误。

![](https://upload-images.jianshu.io/upload_images/5780538-4570e8ded1b2095b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

终发现原来 `new Date()` 在 **安卓** 和 **ios** 上的不同表现

当我们用[JavaScript](#)实例化一个日期对象时，我们可以这样用：
```
let date = new Date(); 
```
上面这段代码是获取当前日期，这段代码在 `Firefox、Chrome、Safari` 浏览器中都可以运行。但是如果我想根据字符串获取日期，问题就来了。看下面代码。

```
let date = new Date("2019-08-23 08:00");  
```

这段代码是获得字符中指定的日期，它 `Firefox、Chrome` 中就能运行，但是放在 `Safari` 就会报错，错误是 `NaN`

## 终发现，只要写成如下格式就可以修复NAN的“BUG”了
```
let date = new Date("2019/08/23 08:00");  
```

写了个正则，替换一下！
```
dateTime.replace(/-/g, "/");
```

end。
