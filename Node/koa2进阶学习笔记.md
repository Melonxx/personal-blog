## koa2特性
* 只提供封装好http上下文、请求、响应，以及基于async/await的中间件容器。
* 利用ES7的async/await的来处理传统回调嵌套问题和代替koa@1的generator，但是需要在node.js 7.x的harmony模式下才能支持async/await。
* 中间件只支持 async/await 封装的，如果要使用koa@1基于generator中间件，需要通过中间件koa-convert封装一下才能使用。

## generator中间件在koa@1中的使用
> generator 中间件在koa v1中可以直接use使用
```
const koa = require('koa')  // koa v1
const loggerGenerator  = require('./middleware/logger-generator')
const app = koa()

app.use(loggerGenerator())

app.use(function *( ) {
    this.body = 'hello world!'
})

app.listen(3000)
console.log('the server is starting at port 3000')
```
## generator中间件在koa@2中的使用
> generator 中间件在koa v2中需要用koa-convert封装一下才能使用
```
const Koa = require('koa') // koa v2
const convert = require('koa-convert')
const loggerGenerator  = require('./middleware/logger-generator')
const app = new Koa()

app.use(convert(loggerGenerator()))

app.use(( ctx ) => {
    ctx.body = 'hello world!'
})

app.listen(3000)
console.log('the server is starting at port 3000')
```
## 原生方法解析出POST请求上下文中的表单数据
>原理：对于POST请求的处理，koa2没有封装获取参数的方法，需要通过解析上下文context中的原生node.js请求对象req，将POST表单数据解析成query string（例如：a=1&b=2&c=3），再将query string 解析成JSON格式（例如：{"a":"1", "b":"2", "c":"3"}）

> 注意：ctx.request是context经过封装的请求对象，ctx.req是context提供的node.js原生HTTP请求对象，同理ctx.response是context经过封装的响应对象，ctx.res是context提供的node.js原生HTTP请求对象。

具体koa2 API文档可见 [https://github.com/koajs/koa/blob/master/docs/api/context.md#ctxreq](https://github.com/koajs/koa/blob/master/docs/api/context.md#ctxreq)
```
// 解析上下文里node原生请求的POST参数
function parsePostData( ctx ) {
  return new Promise((resolve, reject) => {
    try {
      let postdata = "";
      ctx.req.addListener('data', (data) => {
        postdata += data
      })
      ctx.req.addListener("end",function(){
        let parseData = parseQueryStr( postdata )
        resolve( parseData )
      })
    } catch ( err ) {
      reject(err)
    }
  })
}

// 将POST请求参数字符串解析成JSON
function parseQueryStr( queryStr ) {
  let queryData = {}
  let queryStrList = queryStr.split('&')
  console.log( queryStrList )
  for (  let [ index, queryStr ] of queryStrList.entries()  ) {
    let itemList = queryStr.split('=')
    queryData[ itemList[0] ] = decodeURIComponent(itemList[1])
  }
  return queryData
}
```
栗子🌰
```
const Koa = require('koa')
const app = new Koa()

app.use( async ( ctx ) => {

  if ( ctx.url === '/' && ctx.method === 'GET' ) {
    // 当GET请求时候返回表单页面
    let html = `
      <h1>koa2 request post demo</h1>
      <form method="POST" action="/">
        <p>userName</p>
        <input name="userName" /><br/>
        <p>nickName</p>
        <input name="nickName" /><br/>
        <p>email</p>
        <input name="email" /><br/>
        <button type="submit">submit</button>
      </form>
    `
    ctx.body = html
  } else if ( ctx.url === '/' && ctx.method === 'POST' ) {
    // 当POST请求的时候，解析POST表单里的数据，并显示出来
    let postData = await parsePostData( ctx )
    ctx.body = postData
  } else {
    // 其他请求显示404
    ctx.body = '<h1>404！！！ o(╯□╰)o</h1>'
  }
})

// 解析上下文里node原生请求的POST参数
function parsePostData( ctx ) {
  return new Promise((resolve, reject) => {
    try {
      let postdata = "";
      ctx.req.addListener('data', (data) => {
        postdata += data
      })
      ctx.req.addListener("end",function(){
        let parseData = parseQueryStr( postdata )
        resolve( parseData )
      })
    } catch ( err ) {
      reject(err)
    }
  })
}

// 将POST请求参数字符串解析成JSON
function parseQueryStr( queryStr ) {
  let queryData = {}
  let queryStrList = queryStr.split('&')
  console.log( queryStrList )
  for (  let [ index, queryStr ] of queryStrList.entries()  ) {
    let itemList = queryStr.split('=')
    queryData[ itemList[0] ] = decodeURIComponent(itemList[1])
  }
  return queryData
}

app.listen(3000, () => {
  console.log('[demo] request post is starting at port 3000')
})
```
## 原生koa2实现静态资源服务器
>代码目录：
>├── static # 静态资源目录
>│   ├── css/
>│   ├── image/
>│   ├── js/
>│   └── index.html
>├── util # 工具代码
>│   ├── content.js # 读取请求内容
>│   ├── dir.js # 读取目录内容
>│   ├── file.js # 读取文件内容
>│   ├── mimes.js # 文件类型列表
>│   └── walk.js # 遍历目录内容
>└── index.js # 启动入口文件

**index**
```
const Koa = require('koa')
const path = require('path')
const content = require('./util/content')
const mimes = require('./util/mimes')

const app = new Koa()

// 静态资源目录对于相对入口文件index.js的路径
const staticPath = './static'

// 解析资源类型
function parseMime( url ) {
  let extName = path.extname( url )
  extName = extName ?  extName.slice(1) : 'unknown'
  return  mimes[ extName ]
}

app.use( async ( ctx ) => {
  // 静态资源目录在本地的绝对路径
  let fullStaticPath = path.join(__dirname, staticPath)

  // 获取静态资源内容，有可能是文件内容，目录，或404
  let _content = await content( ctx, fullStaticPath )

  // 解析请求内容的类型
  let _mime = parseMime( ctx.url )

  // 如果有对应的文件类型，就配置上下文的类型
  if ( _mime ) {
    ctx.type = _mime
  }

  // 输出静态资源内容
  if ( _mime && _mime.indexOf('image/') >= 0 ) {
    // 如果是图片，则用node原生res，输出二进制数据
    ctx.res.writeHead(200)
    ctx.res.write(_content, 'binary')
    ctx.res.end()
  } else {
    // 其他则输出文本
    ctx.body = _content
  }
})

app.listen(3000)
console.log('[demo] static-server is starting at port 3000')
```
**util/content.js**
```
const path = require('path')
const fs = require('fs')

// 封装读取目录内容方法
const dir = require('./dir')

// 封装读取文件内容方法
const file = require('./file')


/**
 * 获取静态资源内容
 * @param  {object} ctx koa上下文
 * @param  {string} 静态资源目录在本地的绝对路径
 * @return  {string} 请求获取到的本地内容
 */
async function content( ctx, fullStaticPath ) {

  // 封装请求资源的完绝对径
  let reqPath = path.join(fullStaticPath, ctx.url)

  // 判断请求路径是否为存在目录或者文件
  let exist = fs.existsSync( reqPath )

  // 返回请求内容， 默认为空
  let content = ''

  if( !exist ) {
    //如果请求路径不存在，返回404
    content = '404 Not Found! o(╯□╰)o！'
  } else {
    //判断访问地址是文件夹还是文件
    let stat = fs.statSync( reqPath )

    if( stat.isDirectory() ) {
      //如果为目录，则渲读取目录内容
      content = dir( ctx.url, reqPath )

    } else {
      // 如果请求为文件，则读取文件内容
      content = await file( reqPath )
    }
  }

  return content
}

module.exports = content
```
**util/dir.js**
```
const url = require('url')
const fs = require('fs')
const path = require('path')

// 遍历读取目录内容方法
const walk = require('./walk')

/**
 * 封装目录内容
 * @param  {string} url 当前请求的上下文中的url，即ctx.url
 * @param  {string} reqPath 请求静态资源的完整本地路径
 * @return {string} 返回目录内容，封装成HTML
 */
function dir ( url, reqPath ) {

  // 遍历读取当前目录下的文件、子目录
  let contentList = walk( reqPath )

  let html = `<ul>`
  for ( let [ index, item ] of contentList.entries() ) {
    html = `${html}<li><a href="${url === '/' ? '' : url}/${item}">${item}</a>` 
  }
  html = `${html}</ul>`

  return html
}

module.exports = dir
```
**util/file.js**
```
const fs = require('fs')

/**
 * 读取文件方法
 * @param  {string} 文件本地的绝对路径
 * @return {string|binary} 
 */
function file ( filePath ) {

 let content = fs.readFileSync(filePath, 'binary' )
 return content
}

module.exports = file
```
**util/walk.js**
```
const fs = require('fs')
const mimes = require('./mimes')

/**
 * 遍历读取目录内容（子目录，文件名）
 * @param  {string} reqPath 请求资源的绝对路径
 * @return {array} 目录内容列表
 */
function walk( reqPath ){

  let files = fs.readdirSync( reqPath );

  let dirList = [], fileList = [];
  for( let i=0, len=files.length; i<len; i++ ) {
    let item = files[i];
    let itemArr = item.split("\.");
    let itemMime = ( itemArr.length > 1 ) ? itemArr[ itemArr.length - 1 ] : "undefined";

    if( typeof mimes[ itemMime ] === "undefined" ) {
      dirList.push( files[i] );
    } else {
      fileList.push( files[i] );
    }
  }


  let result = dirList.concat( fileList );

  return result;
};

module.exports = walk;
```
**util/mime.js**
```
let mimes = {
  'css': 'text/css',
  'less': 'text/css',
  'gif': 'image/gif',
  'html': 'text/html',
  'ico': 'image/x-icon',
  'jpeg': 'image/jpeg',
  'jpg': 'image/jpeg',
  'js': 'text/javascript',
  'json': 'application/json',
  'pdf': 'application/pdf',
  'png': 'image/png',
  'svg': 'image/svg+xml',
  'swf': 'application/x-shockwave-flash',
  'tiff': 'image/tiff',
  'txt': 'text/plain',
  'wav': 'audio/x-wav',
  'wma': 'audio/x-ms-wma',
  'wmv': 'video/x-ms-wmv',
  'xml': 'text/xml'
}

module.exports = mimes
```
## koa2使用cookie
koa提供了从上下文直接读取、写入cookie的方法
* ctx.cookies.get(name, [options]) 读取上下文请求中的cookie
* ctx.cookies.set(name, value, [options]) 在上下文中写入cookie

>koa2 中操作的cookies是使用了npm的cookies模块，源码在[https://github.com/pillarjs/cookies](https://github.com/pillarjs/cookies)，所以在读写cookie的使用参数与该模块的使用一致。


```
const Koa = require('koa')
const app = new Koa()

app.use( async ( ctx ) => {

  if ( ctx.url === '/index' ) {
    ctx.cookies.set(
      'cid', 
      'hello world',
      {
        domain: 'localhost',  // 写cookie所在的域名
        path: '/index',       // 写cookie所在的路径
        maxAge: 10 * 60 * 1000, // cookie有效时长
        expires: new Date('2017-02-15'),  // cookie失效时间
        httpOnly: false,  // 是否只用于http请求中获取
        overwrite: false  // 是否允许重写
      }
    )
    ctx.body = 'cookie is ok'
  } else {
    ctx.body = 'hello world' 
  }

})

app.listen(3000, () => {
  console.log('[demo] cookie is starting at port 3000')
})
```
## koa2实现session
koa2原生功能只提供了cookie的操作，但是没有提供session操作。session就只用自己实现或者通过第三方中间件实现。在koa2中实现session的方案有一下几种
* 如果session数据量很小，可以直接存在内存中
* 如果session数据量很大，则需要存储介质存放session数据
##### 数据库存储方案
- 将session存放在MySQL数据库中
- 需要用到中间件
  1 koa-session-minimal 适用于koa2 的session中间件，提供存储介质的读写接口 。
  2 koa-mysql-session 为koa-session-minimal中间件提供MySQL数据库的session数据读写操作。
  3 将sessionId和对于的数据存到数据库
- 将数据库的存储的sessionId存到页面的cookie中
- 根据cookie的sessionId去获取对于的session信息
```
const Koa = require('koa')
const session = require('koa-session-minimal')
const MysqlSession = require('koa-mysql-session')

const app = new Koa()

// 配置存储session信息的mysql
let store = new MysqlSession({
  user: 'root',
  password: 'abc123',
  database: 'koa_demo',
  host: '127.0.0.1',
})

// 存放sessionId的cookie配置
let cookie = {
  maxAge: '', // cookie有效时长
  expires: '',  // cookie失效时间
  path: '', // 写cookie所在的路径
  domain: '', // 写cookie所在的域名
  httpOnly: '', // 是否只用于http请求中获取
  overwrite: '',  // 是否允许重写
  secure: '',
  sameSite: '',
  signed: '',

}

// 使用session中间件
app.use(session({
  key: 'SESSION_ID',
  store: store,
  cookie: cookie
}))

app.use( async ( ctx ) => {

  // 设置session
  if ( ctx.url === '/set' ) {
    ctx.session = {
      user_id: Math.random().toString(36).substr(2),
      count: 0
    }
    ctx.body = ctx.session
  } else if ( ctx.url === '/' ) {

    // 读取session信息
    ctx.session.count = ctx.session.count + 1
    ctx.body = ctx.session
  } 

})

app.listen(3000)
console.log('[demo] session is starting at port 3000')
```
## 单元测试
>测试是一个项目周期里必不可少的环节，开发者在开发过程中也是无时无刻进行“人工测试”，如果每次修改一点代码，都要牵一发动全身都要手动测试关联接口，这样子是禁锢了生产力。为了解放大部分测试生产力，相关的测试框架应运而生，比较出名的有mocha，karma，jasmine等。虽然框架繁多，但是使用起来都是大同小异。

##### 安装测试相关框架
```
npm install --save-dev mocha chai supertest
```
* supertest 模块是http请求测试库，用来请求API接口
* mocha 模块是测试框架
* chai 模块是用来进行测试结果断言库，比如一个判断 1 + 1 是否等于 2

**例子目录**
```
.
├── index.js # api文件
├── package.json
└── test # 测试目录
    └── index.test.js # 测试用例
```
**所需测试demo**
```
const Koa = require('koa')
const app = new Koa()

const server = async ( ctx, next ) => {
  let result = {
    success: true,
    data: null
  }

  if ( ctx.method === 'GET' ) { 
    if ( ctx.url === '/getString.json' ) {
      result.data = 'this is string data'
    } else if ( ctx.url === '/getNumber.json' ) {
      result.data = 123456
    } else {
      result.success = false
    }
    ctx.body = result
    next && next()
  } else if ( ctx.method === 'POST' ) {
    if ( ctx.url === '/postData.json' ) {
      result.data = 'ok'
    } else {
      result.success = false
    }
    ctx.body = result
    next && next()
  } else {
    ctx.body = 'hello world'
    next && next()
  }
}

app.use(server)

module.exports = app

app.listen(3000, () => {
  console.log('[demo] test-unit is starting at port 3000')
})
```
**开始写测试用例**
```
const supertest = require('supertest')
const chai = require('chai')
const app = require('./../index')

const expect = chai.expect
const request = supertest( app.listen() )

// 测试套件/组
describe( '开始测试demo的GET请求', ( ) => {

  // 测试用例
  it('测试/getString.json请求', ( done ) => {
      request
        .get('/getString.json')
        .expect(200)
        .end(( err, res ) => {
            // 断言判断结果是否为object类型
            expect(res.body).to.be.an('object')
            expect(res.body.success).to.be.an('boolean')
            expect(res.body.data).to.be.an('string')
            done()
        })
  })
})
```
**执行测试用例**
```
# node.js <= 7.5.x
./node_modules/.bin/mocha  --harmony

# node.js = 7.6.0
./node_modules/.bin/mocha
```
>注意：
 1.如果是全局安装了mocha，可以直接在当前项目目录下执行 mocha --harmony 命令
 2.如果当前node.js版本低于7.6，由于7.5.x以下还直接不支持async/awiar就需要加上--harmony