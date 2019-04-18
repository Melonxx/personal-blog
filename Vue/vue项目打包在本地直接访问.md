1.打开项目文件夹。找到config文件夹里的index.js文件中的build对象，将assetsPublicPath中的“/”，改为“./”。

 

2.router文件下面的index.js路由配置文件不要配置mode: "history"（不用配置这个属性）

 

3.修改config里面的index.js里面的productionSourceMap为false,默认情况是true（true代表打包环境是开发环境，可以进行调试；false表示生产环境，正式上线的）

 

4.项目完成后用 npm run build可以打包项目; 打包配置在config文件夹下的 index.js 的 build对象下;如需在本地预览,

assetsPublicPath: './',改成相对路径; 如果你的css文件中引用了background相对路径,那么在打包后预览后是会出现资源找不到的情况的;

可以在build文件夹的utils.js下:新增这个publicPath这个属性;

![](./images/vue-build/vue-build_(1).png)
