近期在做项目时，发现前端通过base64上传的图片，通过node的fs模块处理之后图片变得异常的大。因此寻找node压缩图片的途径，也百度过很多方案，但是方法总是很**繁琐**，一个偶然的机会看到了[TinyPNG](http://www.baidu.com/link?url=sPV4y0zV3zF9Ce-AFJfFfA7IQvWS3d9Cy9yvFe5fsRy)提供的压缩图片的API。
>因为本人之前在压缩网络图片上用的都是这款应用，**无损压缩真的是非常好用，推荐！**
>感谢史上最好用的图片压缩解决方案 [tinify](https://tinypng.com/developers)

tinfy的API现已支持如下多种语言环境：
*   [HTTP](https://tinypng.com/developers/reference)
*   [RUBY](https://tinypng.com/developers/reference/ruby)
*   [PHP](https://tinypng.com/developers/reference/php)
*   [NODE.JS](https://tinypng.com/developers/reference/nodejs)
*   [PYTHON](https://tinypng.com/developers/reference/python)
*   [JAVA](https://tinypng.com/developers/reference/java)
*   [.NET](https://tinypng.com/developers/reference/dotnet)

###### 并且在node.js使用非常简单！
所有请求都将通过加密的[HTTPS](http://en.wikipedia.org/wiki/HTTP_Secure)连接进行
直接上代码（要使用API​​，您必须提供API密钥。您可以通过注册您的姓名和电子邮件地址来 [获取API密钥](https://tinypng.com/developers)。始终保密API密钥！）
```
npm install --save tinify
var tinify = require("tinify");
tinify.key = "YOUR_API_KEY"; // 自行注册
//您可以将任何JPEG或PNG图像上传到Tinify API进行压缩。
//我们将自动检测图像类型并相应地使用TinyPNG或TinyJPG引擎进行优化。只要您上传文件或提供图片的URL，就会开始压缩。
//您可以选择本地文件作为源，并将其写入另一个文件。
var source = tinify.fromFile("unoptimized.jpg");
 // 这里输入文件也支持网址形式：tinify.fromUrl("https://tinypng.com/images/panda-happy.png");
source.toFile("optimized.jpg");  
// 上述两行代码也支持连写哦！tinify.fromFile("unoptimized.jpg").toFile("optimized.jpg")
```
##### 只需4行代码即可搞定图片的无损压缩，用户体验完美：）
##### 到这里本文已经结束啦~爱折腾的小胖友可以接下来看看以下额外的自定义方法
*
*
*
###### 另外您可以指示API客户端通过HTTP代理发出所有请求。设置代理服务器的URL，可以选择包含凭据。
```
tinify.proxy = "http://user:pass@192.168.0.1:8080";
```
###### 您还可以从缓冲区（带二进制的字符串）上传图像并获取压缩的图像数据。
```
var fs = require("fs");
fs.readFile("unoptimized.jpg", function(err, sourceData) {
  if (err) throw err;
  tinify.fromBuffer(sourceData).toBuffer(function(err, resultData) {
    if (err) throw err;
    // ...
  });
});
```
###### 调整图像大小
使用API​​创建已上传图像的已调整大小的版本。通过让API处理调整大小，您可以避免自己编写此类代码，并且只需上传一次图像。调整大小的图像将被优化压缩，具有漂亮和清晰的外观。

您还可以利用智能裁剪来创建缩略图，这些缩略图专注于图像中最重要的视觉区域。

调整大小计为一次额外压缩。例如，如果您上传单个图像并检索优化版本加上2个已调整大小的版本，则总计为3次压缩。

要调整图像大小，请在`resize`图像源上调用该方法：
```
var source = tinify.fromFile("large.jpg");
var resized = source.resize({
  method: "fit",
  width: 150,
  height: 100
});
resized.toFile("thumbnail.jpg");
```
在`method`描述你的图像将被调整的方式。可以使用以下方法：
![](./images/photo2018-09-21/photo2018-09-21.png)

如果目标尺寸大于原始尺寸，则图像不会按比例放大。为了保护图像质量，可以防止向上扩展。
###### 保留元数据
您可以请求将特定元数据从上载的映像复制到压缩版本。目前支持保存copyright信息，GPS location和creation日期。保留元数据会增加压缩文件的大小，因此您应该只保留重要的元数据。

保留元数据不会算作额外的压缩。但是，在后台，将使用其他元数据再次创建图像。

要保留特定元数据，请在`preserve`图像源上调用该方法：
```
var source = tinify.fromFile("large.jpg");
var copyrighted = source.preserve("copyright", "creation");
copyrighted.toFile("optimized-copyright.jpg");
```
您可以提供以下选项以保留特定元数据。如果上载的图像中不存在请求的元数据，则不会添加元数据。

`copyright`
保留所有版权信息。这包括EXIF版权标记（JPEG），XMP权限标记（PNG）以及Photoshop版权标记或URL。最多使用90个字节，加上版权数据的长度。
`creation`
保留任何创建日期或时间。这是最初创建图像或照片的时刻。这包括EXIF原始日期时间标记（JPEG）或XMP创建时间（PNG）。使用大约70个额外字节。
`location` **（仅限JPEG）**
保留任何描述图像或照片拍摄位置的GPS位置数据。这包括EXIF GPS纬度和GPS经度标签（JPEG）。使用大约130个额外字节。




