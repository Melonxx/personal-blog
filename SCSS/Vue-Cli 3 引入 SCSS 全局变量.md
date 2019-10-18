#首先创建一个全局变量文件 `global.scss`
```
$theme-color: #efefef;
```
#编辑 `vue.config.js`
```
module.exports = {
  // ...
  css: {
    loaderOptions: {
      sass: {
        // 根据自己样式文件的位置调整
        data: `@import "@/styles/global.scss";`
      }
    }
  }
};
```
#现在就可以在任意地方使用global.scss中定义的变量了

```
<template>
  <div class="box"></div>
</template>
<script>
export default {
  data() {
    return {};
  }
};
</script>
<style lang="scss">
.box {
  background-color: $theme-color;
}
</style>
```
#如此简单

[阅读原文](https://blog.svend.cc/2019/03/20/vuecli3-global-scss-variable/)

