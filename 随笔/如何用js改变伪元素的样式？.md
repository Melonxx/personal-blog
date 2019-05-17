在工作偶尔会遇到改变伪元素样式的场景，虽然也有很多解决方法，
但是这个方法能让你**像操作正常元素一样操作伪元素样式**。
```
function ruleSelector(selector) {
  function uni(selector) {
    return selector.replace(/::/g, ':')
  }
  // es6
  return Array.from(document.styleSheets).reduce((a,b) => {
    return a.concat(Array.from(b.cssRules))
  }, []).filter(x => {
    return uni(x.selectorText) === uni(selector);
  })
  // es5
  // return Array.prototype.filter.call(Array.prototype.concat.apply([], Array.prototype.map.call(document.styleSheets, function (x) {
  //   return Array.prototype.slice.call(x.cssRules);
  // })), function (x) {
  //   return uni(x.selectorText) === uni(selector);
  // });
}

var toggle = false,
  pseudo = ruleSelector("ref::before").slice(-1);

document.querySelector("article").onclick = function () {
  pseudo.forEach(function (rule) {
    if (toggle = !toggle)
      rule.style.color = "red";
    else
      rule.style.color = "black";
  });
}
```
