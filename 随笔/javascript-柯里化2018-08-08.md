#### 柯里化 / 高阶函数
###### 柯里化：将 f(x,y) 变成 f(x=1)(y) 或 f(y=1)x
```
  //柯里化之前
  function sum(x,y){
      return x+y
  }
  //柯里化之后
  function addOne(y){
      return sum(1, y)
  }
  //柯里化之前
  function Handlebar(template, data){
      return template.replace('{{name}}', data.name)
  }
  //柯里化之后
  function Handlebar(template){
      return function(data){
          return template.replace('{{name}}', data.name)
      }
  }
```
##### 重点！！！柯里化可以将真实计算拖延到最后再做，也就是如果一个函数返回的函数参数比原函数要少一个那就是柯里化。
###### 关于柯里化的高级文章：

1.  [http://www.yinwang.org/blog-cn/2013/04/02/currying](http://www.yinwang.org/blog-cn/2013/04/02/currying "null")
2.  [https://zhuanlan.zhihu.com/p/31271179](https://zhuanlan.zhihu.com/p/31271179 "null")


###### 高阶函数：
在数学和计算机科学中，高阶函数是至少满足下列一个条件的函数：
接受一个或多个函数作为输入：forEach sort map filter reduce
输出一个函数：lodash.curry
不过它也可以同时满足两个条件：Function.prototype.bind

###### 习题：
请写出一个柯里化其他函数的函数 curry，这个函数能够将接受多个参数的函数，变成多个接受一个参数的函数，具体见示例：
```
function curry(???){
    ???
    return ???
}
var abc = function(a, b, c) {
    return [a, b, c];
};

var curried = curry(abc);

curried(1)(2)(3);
// => [1, 2, 3]

curried(1, 2)(3);
// => [1, 2, 3]

curried(1, 2, 3);
// => [1, 2, 3]
```

###### 本人答案(略有不足，日后完善)
```
function curry (fn) {
    var temp = [];
    var argnum = 0;
    return function () {
        if (fn.length == arguments.length) {
            return fn.apply(null, arguments)
        } else {
            if (temp.length == fn.length) {
                temp = [];
            }
            argnum += arguments.length
            temp = temp.concat(Array.prototype.slice.call(arguments))
            console.log(temp)
            console.log(argnum)
            if (argnum == fn.length) {
                argnum = 0;
                return temp
            }
            return arguments.callee
        }
    }
}
```
