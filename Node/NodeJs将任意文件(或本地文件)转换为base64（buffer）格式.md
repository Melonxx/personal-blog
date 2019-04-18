很多图片音频等文件，有时候可能需要读取到数据中或者放到单文件的HTML中时，将它们转换成为base64格式是一个好方法，nodejs可以很方便的把文件转换为base64格式：
需要依赖库“fs”，“path”，“mime-types”，库mime-types可通过npm安装，具体的代码如下：

---------------------


```
const fs = require('fs');
const path = require('path');
const mineType = require('mime-types');  // 文件类型

let filePath = path.resolve('your/file/path');  // 如果是本地文件
let data = fs.readFileSync(filePath);
let bufferData = new Buffer(data,'base64'); 
let base64 = 'data:' + mineType.lookup(filePath) + ';base64,' + data; 
fs.writeFileSync(path.resolve('your/save/file/path'), base64, err => {...});
// fs.writeFile('your/save/file/path', base64, err => {...});
```