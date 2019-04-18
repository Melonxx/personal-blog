# Collections
**reject**`_.reject(list, predicate, [context])` 
Returns the values in **list** without the elements that the truth test (**predicate**) passes. The opposite of **filter**. **predicate** is transformed through [**iteratee**](https://underscorejs.org/#iteratee) to facilitate shorthand syntaxes.
>var odds = _.reject([1, 2, 3, 4, 5, 6], function(num){ return num % 2 == 0; });
=> [1, 3, 5]

**pluck**`_.pluck(list, propertyName) `
A convenient version of what is perhaps the most common use-case for **map**: extracting a list of property values.

>var stooges = [{name: 'moe', age: 40}, {name: 'larry', age: 50}, {name: 'curly', age: 60}];
_.pluck(stooges, 'name');
=> ["moe", "larry", "curly"]

**max**`_.max(list, [iteratee], [context])` 
Returns the maximum value in **list**. If an [**iteratee**](https://underscorejs.org/#iteratee) function is provided, it will be used on each value to generate the criterion by which the value is ranked. *-Infinity* is returned if **list** is empty, so an [isEmpty](https://underscorejs.org/#isEmpty) guard may be required. Non-numerical values in **list** will be ignored.

>var stooges = [{name: 'moe', age: 40}, {name: 'larry', age: 50}, {name: 'curly', age: 60}];
_.max(stooges, function(stooge){ return stooge.age; });
=> {name: 'curly', age: 60};

**min**`_.min(list, [iteratee], [context])` 
Returns the minimum value in **list**. If an [**iteratee**](https://underscorejs.org/#iteratee) function is provided, it will be used on each value to generate the criterion by which the value is ranked. *Infinity* is returned if **list** is empty, so an [isEmpty](https://underscorejs.org/#isEmpty) guard may be required. Non-numerical values in **list** will be ignored.

> 跟 `max` 相反。

**shuffle**`_.shuffle(list)` 
Returns a shuffled copy of the **list**, using a version of the [Fisher-Yates shuffle](http://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle).

>_.shuffle([1, 2, 3, 4, 5, 6]);
=> [4, 1, 6, 3, 5, 2]
打乱一个数组。

**sample**`_.sample(list, [n]) `
Produce a random sample from the **list**. Pass a number to return **n** random elements from the list. Otherwise a single random item will be returned.

>_.sample([1, 2, 3, 4, 5, 6]);
=> 4
>
>_.sample([1, 2, 3, 4, 5, 6], 3);
=> [1, 6, 2]
打乱并取出。

**size**`_.size(list) `
Return the **number** of values in the **list**.

>_.size([1, 2, 3, 4, 5]);
=> 5
>
>_.size({one: 1, two: 2, three: 3});
=> 3
能统计 `{...}` 的 ‘length’，不知道会不会用的到。

**partition**`_.partition(list, predicate)` 
Split **list** into two arrays: one whose elements all satisfy **predicate** and one whose elements all do not satisfy **predicate**. **predicate** is transformed through [**iteratee**](https://underscorejs.org/#iteratee) to facilitate shorthand syntaxes.

>_.partition([0, 1, 2, 3, 4, 5], v => v%2);
=> [[1, 3, 5], [0, 2, 4]]
按条件分组

# Arrays
**compact**`_.compact(list) `
Returns a copy of the list with all **falsy values removed**. In JavaScript, false, null, 0, "", undefined and NaN are all falsy.

>_.compact([0, 1, false, 2, '', 3]);
=> [1, 2, 3]
去除所有 `falsy`

**flatten**`_.flatten(array, [shallow]) `
Flattens a nested **array** (the nesting can be to any depth). If you pass **shallow**, the array will only be flattened a single level.

>_.flatten([1, [2], [3, [[4]]]]);
=> [1, 2, 3, 4];
>
>_.flatten([1, [2], [3, [[4]]]], true);
=> [1, 2, 3, [[4]]];
该方法在 `es10` 已出（`flat()`、`flatMap()`）

**without**`_.without(array, *values) `
Returns a copy of the **array** with all instances of the **values removed**.
>_.without([1, 2, 1, 0, 3, 1, 4], 0, 1);
=> [2, 3, 4]

**difference**`_.difference(array, *others) `
Similar to **without**, but returns the values from array that are not present in the **other arrays**.

>_.difference([1, 2, 3, 4, 5], [5, 2, 10]);
=> [1, 3, 4]
取出该数组与后面不同的元素

**union**`_.union(*arrays) `
Computes the **union** of the passed-in **arrays**: the list of unique items, in order, that are present in one or more of the **arrays**.
>_.union([1, 2, 3], [101, 2, 1, 10], [2, 1]);
=> [1, 2, 3, 101, 10]
虽然 `es6` 的 `set` 也支持去重，但是 `union` 还支持多参数。 

**intersection**`_.intersection(*arrays) `
Computes the list of values that are the **intersection** of all the arrays. Each value in the result is present in each of the arrays.

>_.intersection([1, 2, 3], [101, 2, 1, 10], [2, 1]);
=> [1, 2]
只取出多个数组相交(重复)的元素。

**uniq**`_.uniq(array, [isSorted], [iteratee])` Alias: **unique** 
Produces a duplicate-free version of the **array**, using *===* to test object equality. In particular only the first occurrence of each value is kept. If you know in advance that the **array** is sorted, passing *true* for **isSorted** will run a much faster algorithm. If you want to compute unique items based on a transformation, pass an [**iteratee**](https://underscorejs.org/#iteratee) function.

>_.uniq([1, 2, 1, 4, 1, 3]);
=> [1, 2, 4, 3]
好像和 `union` 差不多，但是多一个迭代器的选项。

**zip**`_.zip(*arrays) `
Merges together the values of each of the **arrays** with the values at the corresponding position. Useful when you have separate data sources that are coordinated through matching array indexes. Use with apply to pass in an array of arrays. If you're working with a matrix of nested arrays, this can be used to transpose the matrix.

>_.zip(['moe', 'larry', 'curly'], [30, 40, 50], [true, false, false]);
=> [["moe", 30, true], ["larry", 40, false], ["curly", 50, false]]

**unzip**`_.unzip(array)` 
The opposite of [zip](https://underscorejs.org/#zip). Given an **array** of arrays, returns a series of new arrays, the first of which contains all of the first elements in the input arrays, the second of which contains all of the second elements, and so on.

>_.unzip([["moe", 30, true], ["larry", 40, false], ["curly", 50, false]]);
=> [['moe', 'larry', 'curly'], [30, 40, 50], [true, false, false]]

**object**`_.object(list, [values])` 
Converts arrays into objects. Pass either a single list of [key, value] pairs, or a list of keys, and a list of values. Passing by pairs is the reverse of [pairs](https://underscorejs.org/#pairs). If duplicate keys exist, the last value wins.

>_.object(['moe', 'larry', 'curly'], [30, 40, 50]);
=> {moe: 30, larry: 40, curly: 50}
>
>_.object([['moe', 30], ['larry', 40], ['curly', 50]]);
=> {moe: 30, larry: 40, curly: 50}

**chunk**`_.chunk(array, length) `
Chunks an **array** into multiple arrays, each containing **length** or fewer items.

>var partners = _.chunk(_.shuffle(kindergarten), 2);
=> [["Tyrone", "Elie"], ["Aidan", "Sam"], ["Katrina", "Billie"], ["Little Timmy"]]
按第二个参数分组。

**range**`_.range([start], stop, [step]) `
A function to create flexibly-numbered lists of integers, handy for each and map loops. **start**, if omitted, defaults to 0; **step** defaults to 1. Returns a list of integers from start (inclusive) to **stop** (exclusive), incremented (or decremented) by **step**, exclusive. Note that ranges that **stop** before they **start** are considered to be zero-length instead of negative — if you'd like a negative range, use a negative **step**.

>_.range(10);
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
_.range(1, 11);
=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
_.range(0, 30, 5);
=> [0, 5, 10, 15, 20, 25]
_.range(0, -10, -1);
=> [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
_.range(0);
=> []

# Functions

**throttle**`_.throttle(function, wait, [options]) `
Creates and returns a new, throttled version of the passed function, that, when invoked repeatedly, will only actually call the original function at most once per every wait milliseconds. Useful for rate-limiting events that occur faster than you can keep up with.

By default, throttle will execute the function as soon as you call it for the first time, and, if you call it again any number of times during the wait period, as soon as that period is over. If you'd like to disable the leading-edge call, pass {leading: false}, and if you'd like to disable the execution on the trailing-edge, pass 
`{trailing: false}`.

>var throttled = _.throttle(updatePosition, 100);
$(window).scroll(throttled);
节流

If you need to cancel a scheduled throttle, you can call .cancel() on the throttled function.

**debounce**`_.debounce(function, wait, [immediate]) `
Creates and returns a new debounced version of the passed function which will postpone its execution until after wait milliseconds have elapsed since the last time it was invoked. Useful for implementing behavior that should only happen after the input has stopped arriving. For example: rendering a preview of a Markdown comment, recalculating a layout after the window has stopped being resized, and so on.

At the end of the wait interval, the function will be called with the arguments that were passed most recently to the debounced function.

Pass true for the immediate argument to cause debounce to trigger the function on the leading instead of the trailing edge of the wait interval. Useful in circumstances like preventing accidental double-clicks on a "submit" button from firing a second time.

>var lazyLayout = _.debounce(calculateLayout, 300);
$(window).resize(lazyLayout);
节流

If you need to cancel a scheduled debounce, you can call .cancel() on the debounced function.

**once**`_.once(function)`
Creates a version of the function that can only be called one time. Repeated calls to the modified function will have no effect, returning the value from the original call. Useful for initialization functions, instead of having to set a boolean flag and then check it later.

>var initialize = _.once(createApplication);
initialize();
initialize();
// Application is only created once.

**times**`_.times(n, iteratee, [context])` 
Invokes the given iteratee function **n** times. Each invocation of [**iteratee**](https://underscorejs.org/#iteratee) is called with an `index` argument. Produces an array of the returned values.

>_.times(3, function(n){ console.log(n); });
比如说替代:
for (let i=0;i<3;i++;) {
  console.log(i)
}



**compose**`_.compose(*functions) `
Returns the composition of a list of functions, where each function consumes the return value of the function that follows. In math terms, composing the functions **f()**, **g()**, and **h()** produces **f(g(h()))**.

>var greet    = function(name){ return "hi: " + name; };
var exclaim  = function(statement){ return statement.toUpperCase() + "!"; };
var welcome = _.compose(greet, exclaim);
welcome('moe');
=> 'hi: MOE!'
把后面的函数的返回值当做参数给前面的函数。

# Object

**allKeys**`_.allKeys(object) `
Retrieve all the names of object's own and inherited properties.

>function Stooge(name) {
  this.name = name;
}
Stooge.prototype.silly = true;
_.allKeys(new Stooge("Moe"));
=> ["name", "silly"]

**pairs**`_.pairs(object) `
Convert an object into a list of   [key, value]  pairs. The opposite of [object](https://underscorejs.org/#object).

>_.pairs({one: 1, two: 2, three: 3});
=> [["one", 1], ["two", 2], ["three", 3]]
把对象的键值对转换成数组。

**invert**`_.invert(object) `
Returns a copy of the object where the keys have become the values and the values the keys. For this to work, all of your object's values should be unique and string serializable.

>_.invert({Moe: "Moses", Larry: "Louis", Curly: "Jerome"});
=> {Moses: "Moe", Louis: "Larry", Jerome: "Curly"};
交换对象的键值对。

**pick**`_.pick(object, *keys) `
Return a copy of the **object**, filtered to only have **values** for the whitelisted keys (or array of valid keys). Alternatively accepts a predicate indicating which keys to pick.

>_.pick({name: 'moe', age: 50, userid: 'moe1'}, 'name', 'age');
=> {name: 'moe', age: 50}
_.pick({name: 'moe', age: 50, userid: 'moe1'}, function(value, key, object) {
  return _.isNumber(value);
});
=> {age: 50}

**omit**`_.omit(object, *keys) `
Return a copy of the **object**, filtered to omit the blacklisted **keys** (or array of keys). Alternatively accepts a predicate indicating which keys to omit.

>_.omit({name: 'moe', age: 50, userid: 'moe1'}, 'userid');
=> {name: 'moe', age: 50}
_.omit({name: 'moe', age: 50, userid: 'moe1'}, function(value, key, object) {
  return _.isNumber(value);
});
=> {name: 'moe', userid: 'moe1'}

**isEqual**`_.isEqual(object, other) `
Performs an optimized deep **comparison** between the two objects, to determine if they should be considered equal.

>var stooge = {name: 'moe', luckyNumbers: [13, 27, 34]};
var clone  = {name: 'moe', luckyNumbers: [13, 27, 34]};
stooge == clone;
=> false
_.isEqual(stooge, clone);
=> true

**isMatch**`_.isMatch(object, properties) `
Tells you if the keys and values in properties are contained in object.

>var stooge = {name: 'moe', age: 32};
_.isMatch(stooge, {age: 32});
=> true

**isEmpty**`_.isEmpty(object) `
Returns true if an enumerable object contains no values (no enumerable own-properties). For strings and array-like objects _.isEmpty checks if the length property is 0.

>_.isEmpty([1, 2, 3]);
=> false
_.isEmpty({});
=> true

**isArray**`_.isArray(object) `
Returns true if object is an Array.

>(function(){ return _.isArray(arguments); })();
=> false
_.isArray([1,2,3]);
=> true

**isObject**`_.isObject(value) `
Returns true if value is an Object. Note that JavaScript arrays and functions are objects, while (normal) strings and numbers are not.

>_.isObject({});
=> true
_.isObject(1);
=> false

**isNaN**`_.isNaN(object) `
Returns true if object is NaN.
Note: this is not the same as the native isNaN function, which will also return true for many other not-number values, such as undefined.

>_.isNaN(NaN);
=> true
isNaN(undefined);
=> true
_.isNaN(undefined);
=> false

**isNull**`_.isNull(object) `
Returns true if the value of object is null.

>_.isNull(null);
=> true
_.isNull(undefined);
=> false

**isUndefined**`_.isUndefined(value) `
Returns true if value is undefined.

>_.isUndefined(window.missingVariable);
=> true