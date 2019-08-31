# Node.js

**Node.js是一个基于Chrome V8引擎的JavaScript运行环境。 ** 

**Node.js使用了一个事件驱动、非阻塞式I/O的模型，使其轻量又高效。 ** 

**Node.js的包管理器npm,是全球最大的开源库生态系统。 ** 



## 流

一般我们处理数据有两种模式：`buffer`模式、`stream`模式

- `buffer`模式：取完数据一次性操作
- `stream`模式：边取数据边操作

在node中读取文件的方式有来两种，一个是利用fs模块，一个是利用流来读取。如果读取小文件，我们可以使用fs读取，fs读取文件的时候，是将文件一次性读取到本地内存。而如果读取一个大文件，一次性读取会占用大量内存，效率很低，这个时候需要用流来读取。流是将数据分割段，一段一段的读取，可以控制速率,效率很高,不会占用太大的内存。gulp的task任务，文件压缩，和http中的请求和响应等功能的实现都是基于流来实现的。

Node.js 中有四种基本的流类型：

- `Readable` - 可读的流 (例如` fs.createReadStream()`).
- `Writable` - 可写的流 (例如` fs.createWriteStream()`).
- `Duplex` - 可读写的流 (例如 `net.Socket`).
- `Transform` - 在读写过程中可以修改和变换数据的 Duplex 流 (例如` zlib.createDeflate()`)

```js
let fs = require('fs');
let rs = fs.createReadStream('./1.txt',{
    highWaterMark:3, //文件一次读多少字节,默认 64*1024
    flags:'r', //默认 'r'
    autoClose:true, //默认读取完毕后自动关闭
    start:0, //读取文件开始位置
    end:3, //流是闭合区间 包含start也含end
    encoding:'utf8' //默认null
});
```



## 可读流

可读流中的两种模式：

- 流动模式 ( flowing ) ：数据自动从系统底层读取，并通过事件，尽可能快地提供给应用程序。
- 暂停模式 ( paused )，必须显式的调用 `read()` 读取数据。

**可读流都开始于 暂停模式**

**暂停模式 切换到 流动模式**：
 1.添加 `data` 事件回调。
 2.调用 `resume()`。
 3.调用 `pipe()`。

**流动模式 切换到 暂停模式**
 1.如果没有管道目标，调用 `pause()`。
 2.如果有管道目标，移除所有管道目标，调用 `unpipe()` 移除多个管道目标。




### pipe

`pipe`方法的主要功能是在可读流与可写流之间连接一个管道。详细分为：

- 不断从来源可读流中获得一个指定长度的数据。
- 将获取到的数据写入目标可写流。
- 平衡读取和写入速度，防止读取速度大大超过写入速度时，出现大量滞留数据。

例：

```
readable.pipe(writable)
```



### 监听err/end/close事件

```js
//读取数据出错时触发
rs.on("err",()=>{
    console.log("发生错误")
});
//文件读取完毕后触发
rs.on('end',()=>{ 
    console.log("读取完毕");
});
//文件关闭时触发
rs.on("close",()=>{ 
    console.log("关闭")
});
```

### 监听open事件

* 文件被打开时触发

```js
rs.on("open",()=>{
   console.log("文件打开")
});
```

### 监听data事件

- 可读流这种模式它默认情况下是非流动模式(暂停模式)，它什么也不做，就在这等着

- 监听data事件，会让当前流切换到流动模式

- 流动模式会疯狂的触发data事件，直到读取完毕

  示例：

```js
//1.txt中内容为1234567890
let fs = require('fs');
let rs = fs.createReadStream('./1.txt',{
    highWaterMark:3, //文件一次读多少字节,默认 64*1024
    flags:'r', //默认 'r'
    autoClose:true, //默认读取完毕后自动关闭
    start:0, //读取文件开始位置
    end:3, //流是闭合区间 包含start也含end
    encoding:'utf8' //默认null
});
rs.on("open",()=>{
   console.log("文件打开")
});
//疯狂触发data事件 直到读取完毕
rs.on('data',(data)=>{
    console.log(data); //共读4个字节，但是highWaterMark为3，所以触发2次data事件，分别打印123  4
});
```

### pause事件

rs.pause() 暂停读取,会暂停data事件的触发，将流动模式转变非流动模式

调用监听器函数时不传入任何参数。

```js
rl.on('pause', () => {
  console.log('Readline 暂停');
});
```

### resume事件

**rs.resume()恢复data事件,继续读取，变为流动模式**

调用监听器函数时不传入任何参数。

```js
rl.on('resume', () => {
  console.log('Readline 恢复');
});
```



## 可写流

`createWriteStream` 方法有两个参数：

1.第一个参数是读取文件的路径
 2.第二个参数为 `options` 选项，其中有七个参数：

- flags：标识位，默认为 w；
- encoding：字符编码，默认为 utf8；
- fd：文件描述符，默认为 null；
- mode：权限位，默认为 0o666；
- autoClose：是否自动关闭文件，默认为 true；
- start：写入文件的起始位置；
- highWaterMark：一个对比写入字节数的标识，默认 16 * 1024。








## 一、gloable - 全局变量

### 1、__dirname

当前模块的目录名。

```js
console.log(__dirname);
// 打印: /Users/mjr			(就是当前的目录 )
console.log(path.dirname(__filename));
// 打印: /Users/mjr			(就是当前的目录 )
```

### 2、__filename

当前模块的文件名，这是当前的模块文件的绝对路径（符号链接会被解析）。

对于主程序，这不一定与命令行中使用的文件名相同。

示例：

```js
console.log(__filename);
// 打印: /Users/mjr/example.js
console.log(__dirname);
// 打印: /Users/mjr
```





## 二、http - HTTP

要使用 HTTP 服务器和客户端，必须 `require('http')`。

```js
const http = require("http")
```

```txt
简单了解：
http://localhost:3000/abc?name="zhang"#5678

http://  协议   http   https
localhost域名
3000 端口
/abc 路径名
name="zhang" 查询字符串
路径名 + 查询字符串 = 路径
#5678 hash
```

```
get请求：在地址栏中输入网址，a标签中的href，img标签中的src，script标签中的src，form中的method为get
post请求：表单中method指定post
```

```js
let http = require("http")
// // http协议：
//     请求：3个部分：
//         请求行  方法  路径  协议
//         请求头  浏览器添加 自己也可以添加
//         请求正文  给服务器的数据
//     响应：
//         状态码：200 请求成功了
//                 301 永久重写向 
//                 302 临时重写向 
//                 304 缓存
//                 404 服务器找不到请求的资源  
//                 401 无仅限访问 
//                 500 服务器挂了
//         响应头： 一堆的头
//         响应正文：服务器真实给你的数据
// function(req,res){}是回调函数，有请求过来后，会调用这个函数
let server = http.createServer(function(req,res){
    // req 表示请求，是一个可读流     res 表示响应，是 一个可写流
    // console.log(req) // IncomingMessage
    console.log(req.url) // /  前端的路由：一个url对应一个组件   后端路由：一个url对应资源  /shop?name=%22zhangsan%22
})

let port = 3000
server.listen(port,()=>{
    console.log(`服务器运行在${port}上面...`)
})

// 每一次改变代码，都是需要重启服务器，可装一个模块，叫nodemon，修改完代码后，会自动重启
// 安装：> npm i nodemon -g
// 使用：> nodemon 01-http.js
```



### 1、http.createServer

**`http.createServer([requestListener])`**

`requestListener`   请求处理函数，自动添加到 request 事件，函数传递两个参数：

​    request     请求对象，想知道req有哪些属性。

​    response   响应对象 ，收到请求后要做出的响应。

### 2、http.request

**`http.request(url [,otions] [, callback])`**

Node.js 为每个服务器维护多个连接以发出 HTTP 请求。 此函数允许显式地发出请求。

- `url` 	发送链接的地址
- `options` 
  - `protocol`使用的协议。**默认值:** `'http:'`。
  - `host`请求发送至的服务器的域名或 IP 地址。**默认值:** `'localhost'`。
  - `hostname` `host` 的别名。为了支持 [`url.parse()`]，如果同时指定 `host` 和 `hostname`，则使用 `hostname`。
  - `family` 当解析 `host` 或 `hostname` 时使用的 IP 地址族。有效值为 `4` 或 `6`。如果没有指定，则同时使用 IP v4 和 v6。
  - `port`远程服务器的端口。**默认值:** `80`。
  - `localAddress`  为网络连接绑定的本地接口。
  - `socketPath`  Unix 域套接字。如果指定了 `host` 或 `port` 之一（它们指定了 TCP 套接字），则不能使用此选项。
  - `method` 一个字符串，指定 HTTP 请求的方法。**默认值:** `'GET'`。
  - `path`  请求的路径。应包括查询字符串（如果有）。例如 `'/index.html?page=12'`。当请求的路径包含非法的字符时，则抛出异常。目前只有空格被拒绝，但未来可能会有所变化。**默认值:** `'/'`。
  - `headers` 包含请求头的对象。
  - `auth`  基本的身份验证，即 `'user:password'`，用于计算授权请求头。
  - `agent` 控制 [`Agent`] 的行为。可能的值有：
    - `undefined` (默认): 对此主机和端口使用 [`http.globalAgent`]。
    - `Agent` 对象: 显式地使用传入的 `Agent`。
    - `false`: 使用新建的具有默认值的 `Agent`。
  - `createConnection`  当 `agent` 选项未被使用时，用来为请求生成套接字或流的函数。这可用于避免创建自定义的 `Agent` 类以覆盖默认的 `createConnection` 函数。详见 [`agent.createConnection()`]。任何[双工流]都是有效的返回值。
  - `timeout` : 指定套接字超时的数值，以毫秒为单位。这会在套接字被连接之前设置超时。
  - `setHost` : 指定是否自动添加 `Host` 请求头。**默认值:** `true`。
- `callback` 
- 返回: 



###  以下为http.ServerResponse 类--------------------------->



### 3、response.writeHead

**`response.writeHead(statusCode[, statusMessage] [, headers])`**

- `statusCode` [number]
- `statusMessage` [string]
- `headers` [object]

向请求发送响应头。 状态码是一个 3 位的 HTTP 状态码，如 `404`。 最后一个参数 `headers` 是响应头。 可以可选地将用户可读的 `statusMessage` 作为第二个参数。

### 4、response.write

**`response.write(chunk[, encoding] [, callback])`**

- `chunk` [string]或[buffer]
- `encoding` [string] **默认值:** `'utf8'`。
- `callback` [Function]
- 返回: [boolean]

向客户端写出内容，其中用双引号括起来的就是字符串，直接向客户端输出的部分，用&连接起来的部分就是一个变量，把该变量当时的值输出到客户端。

如果调用此方法并且尚未调用 [`response.writeHead()`]，则将切换到隐式响应头模式并刷新隐式响应头。

这会发送一块响应主体。 可以多次调用该方法以提供连续的响应主体片段。

### 5、response.end

**`request.end([data[, encoding]] [, callback])`**

- `data` [string] | [buffer]
- `encoding` [string]
- `callback` [Function]
- 返回: [this]

此方法向服务器发出信号，表明已发送所有响应头和主体，该服务器应该视为此消息已完成。 必须在每个响应上调用此 `response.end()` 方法。

如果指定了 `data`，则相当于调用 [`response.write(data, encoding)`] 之后再调用 `response.end(callback)`。

如果指定了 `callback`，则当响应流完成时将调用它。

### 6、response.statusCode

**`response.statusCode`**

当使用隐式的响应头时（没有显式地调用 `response.writeHead()`），此属性控制在刷新响应头时将发送到客户端的状态码。

```js
response.statusCode = 404;
//			状态码：200 请求成功了
//                 301 永久重写向 
//                 302 临时重写向 
//                 304 缓存
//                 404 服务器找不到请求的资源  
//                 401 无仅限访问 
//                 500 服务器挂了
```

响应头发送到客户端后，此属性表示已发送的状态码。

### 7、response.setHeader

**`response.setHeader(name, value)`**

为隐式响应头设置单个响应头的值。 如果此响应头已存在于待发送的响应头中，则其值将被替换。 在这里可以使用字符串数组来发送具有相同名称的多个响应头。 非字符串值将被原样保存。 因此 [`response.getHeader()`]可能返回非字符串值。 但是非字符串值将转换为字符串以进行网络传输。







### http.ClientRequest 类---------------------------------->

### 8、







## 三、url - URL

`url` 模块用于处理与解析 URL。 使用方法如下：

```js
const url = require('url');
```



### 1、url.parse

**`url.parse(urlString[, parseQueryString[, slashesDenoteHost]])`**

- `urlString` 要解析的 URL 字符串。
- `parseQueryString` 如果设为 `true`，则返回的 URL 对象的 `query` 属性会是一个使用 [`querystring`] 模块的 `parse()` 生成的对象。 如果设为 `false`，则 `query` 会是一个未解析未解码的字符串。 默认为 `false`。
- `slashesDenoteHost` 如果设为 `true`，则 `//` 之后至下一个 `/` 之前的字符串会解析作为 `host`。 例如， `//foo/bar` 会解析为 `{host: 'foo', pathname: '/bar'}` 而不是 `{pathname: '//foo/bar'}`。 默认为 `false`。

解析 URL 字符串并返回 URL 对象（需要加上第二个参数 true）。

如果 `urlString` 不是字符串，则抛出`TypeError`。

如果 `auth` 属性存在但无法解码，则抛出 `URIError`。



```js
let url = require("url")
let a = url.parse("sda");
console.log(a) 
//返回结果：
// Url {
//     protocol: null,		// 协议
//     slashes: null,		
//     auth: null,			// 认证
//     host: null,			// 主机
//     port: null,			// 端口
//     hostname: null,		// 主机名
//     hash: null,
//     search: null,		// 查询字符串
//     query: null,			// 查询字符串
//     pathname: 'sda',		// 路径名
//     path: 'sda',			// 路径
//     href: 'sda' 
//}
```



### 2、url.resolve

**`url.resolve(from, to)`**

- `from` 解析时相对的基本 URL。
- `to`  要解析的超链接 URL。

`url.resolve()` 方法会以一种 Web 浏览器解析超链接的方式把一个目标 URL 解析成相对于一个基础 URL。

例子：

```js
const url = require('url');
url.resolve('/one/two/three', 'four');         // '/one/two/four'
url.resolve('http://example.com/', '/one');    // 'http://example.com/one'
url.resolve('http://example.com/one', '/two'); // 'http://example.com/two'
```



## 四、process - 进程

### 1、process

`process` 对象是一个**全局变量**，它提供有关当前 Node.js 进程的信息并对其进行控制。 作为一个全局变量，它始终可供 Node.js 应用程序使用，无需使用 `require()`。

(直接使用即可，使用方法与`__filename`与`__dirname`类似)



### 2、process.nextTick

 **`process.nextTick(callback,[...args])`**

- `callback` 回调函数
- `...args` 当调用 `callback` 时传入的其他参数。

`process.nextTick()` 方法将 `callback` 添加到下一个时间点的队列。 一旦当轮的事件循环全部完成，则调用下一个时间点的队列中的所有回调。

这不是 [`setTimeout(fn, 0)`]的简单别名。 它的效率更高。 它会在事件循环的下一个时间点中触发任何其他 I/O 事件（包括定时器）之前运行。

```js
console.log('开始');
process.nextTick(() => {
  console.log('下一个时间点的回调');
});
console.log('调度');
// 输出:	开始
// 		 调度
//     	 下一个时间点的回调
```

补充：

```js
// nextTick > then
process.nextTick(function(){
    console.log("nextTick")
})
Promise.resolve().then(()=>{
    console.log("then")
})
```



## 五、path - 路径

`path` 模块提供用于处理文件路径和目录路径的实用工具。 它可以使用以下方式访问：

```js
const path = require('path');
```

注意：

​		path 内置模块  系统模块  核心模块

​		内置模块，直接require  require("path")

​		第三方模块，直接require  require("element-ui")

​		自定义模块，需要以./打头   require("./myxx")



### 1、path.basename

**`path.basename(path[, ext])`**

- `path` 
- `ext`  可选的文件扩展名。
- 返回一个字符串

`path.basename()` 方法返回 `path` 的最后一部分，尾部的目录分隔符将被忽略。

```js
let path = require("path")
let a = path.basename('/foo/bar/baz/asdf/quux.html');
console.log(a)

//返回结果：	quux.html
```

### 2、path.extname

**path.extname(path)**		返回一个字符串

`path.extname()` 方法返回 `path` 的扩展名，从最后一次出现 `.`（句点）字符到 `path` 最后一部分的字符串结束。 如果在 `path` 的最后一部分中没有 `.` ，或者如果 `path` 的基本名称（参阅 `path.basename()`）除了第一个字符以外没有 `.`，则返回空字符串。

```js
let path = require("path")
let a = path.extname('/foo/bar/baz/asdf/quux.html');
let b = path.extname("index.css")
console.log(a,b)

//返回结果：		.html .css
```

### 3、path.dirname

**`path.dirname(path)`**			返回一个字符串

`path.dirname()` 方法返回 `path` 的目录名，尾部的目录分隔符将被忽略.

```js
let path = require("path")
let a = path.dirname('/foo/bar/baz/asdf/quux.html');
let b = path.dirname("./css/index.css")
console.log(a,b)

//返回结果：  /foo/bar/baz/asdf  ./css
```

### 4、path.join

**`path.join([...paths])`**

- `...paths`  路径片段的序列。
- 返回一个字符串

`path.join()` 方法使用平台特定的分隔符作为定界符将所有给定的 `path` 片段连接在一起，然后规范化生成路径。

零长度的 `path` 片段会被忽略。 如果连接的路径字符串是零长度的字符串，则返回 `'.'`，表示当前工作目录。

如果任何路径片段不是字符串，则抛出 [`TypeError`]。

**注意：`..` 同` ../`是一个意思都代表上一级目录**

```js
let path = require("path")
let r = path.join("a/b/c","/sda/sa","/dsa","dasdawwqw",".")  // 把两个路径拼成一个路径
let a = path.join('foo', 'bar', 'baz/asdf', 'quux',"../","../");
console.log(r)
console.log(a)
//还有就是.. 同 ../是一个意思都代表上一级目录

//返回结果：     a\b\c\sda\sa\dsa\dasdawwqw
//              foo\bar\baz\
```

### 5、path.resolve

**`path.resolve([...paths])`**

- `...paths` 路径或路径片段的序列。
- 返回一个字符串

`path.resolve()` 方法将路径或路径片段的序列解析为绝对路径。零长度的 `path` 片段会被忽略。

如果没有传入 `path` 片段，则 `path.resolve()` 将返回当前工作目录的绝对路径。

```js
let path = require("path")
let a = path.resolve('/foo/bar', './baz');
console.log(a)

//返回结果：    d:\foo\bar\baz
```

**应用：在服务器端，路径尽量使用绝对路径。使用path模块，目的就是为了得到绝对路径。**





## 六、fs - 文件系统

`fs` 模块提供了一个 API，用于以模仿标准 POSIX 函数的方式与文件系统进行交互。

要使用此模块：

```js
const fs = require('fs');
```

所有文件系统操作都具有同步和异步的形式。 



### 1、fs.readFile

**`fs.readFile(path[, options], callback)`**

**异步**地读取文件的全部内容。回调会传入两个参数 `(err, data)`，其中`err`是错误信息，  `data` 是文件的内容。

```js
fs.readFile('/etc/passwd', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```



### 2、fs.createReadStream 

**`fs.createReadStream(path[, options])`**

- `path` [string]| [Buffer] | [URL]

- `options` [string]| [Object]
  - flags：标识位，默认为 r；
  - encoding：字符编码，默认为 null；
  - fd：文件描述符，默认为 null；
  - mode：权限位，默认为 0o666；
  - autoClose：是否自动关闭文件，默认为 true；
  - start：读取文件的起始位置；
  - end：读取文件的（包含）结束位置；
  - highWaterMark：最大读取文件的字节数，默认 64 * 1024。
- 返回: [fs.ReadStream] 。

1. 与可读流的 16 kb 的默认 `highWaterMark` 不同，此方法返回的流具有 64 kb 的默认 `highWaterMark`。

2. `options` 可以包括 `start` 和 `end` 值，以从文件中读取一定范围的字节而不是读取整个文件。 `start` 和 `end` 都包含在内并从 0 开始计数，允许的值在 [0, [`Number.MAX_SAFE_INTEGER`]] 的范围内。 如果指定了 `fd` 并且 `start` 被省略或 `undefined`，则 `fs.createReadStream()` 从当前文件位置开始顺序读取。 `encoding` 可以是 [`Buffer`] 接受的任何一种字符编码。

3. 如果指定了 `fd`，则 `ReadStream` 将忽略 `path` 参数并使用指定的文件描述符。 这意味着不会触发 `'open'` 事件。 `fd` 必须是阻塞的，非阻塞的 `fd` 应该传给 [`net.Socket`]。如果 `fd` 指向仅支持阻塞读取的字符设备（例如键盘或声卡），则在数据可用之前，读取操作不会完成。 这可以防止进程退出并且流自然关闭。

4. 如果 `autoClose` 为 `false`，则即使出现错误，也不会关闭文件描述符。 应用程序负责关闭它并确保没有文件描述符泄漏。 如果 `autoClose` 设为 `true`（默认行为），则在 `'error'` 或 `'end'` 事件时将自动关闭文件描述符。

5. `mode` 用于设置文件模式（权限和粘滞位），但仅限于创建文件的情况。



### 3、fs.ReadStream

成功调用 `fs.createReadStream()` 将返回一个新的 `fs.ReadStream` 对象。

所有 `fs.ReadStream` 对象都是[可读流]。



### 4、fs.stat

**`fs.stat(path[, options], callback)`**

- `path`  [string]| [Buffer] | [URL]
- `options` [Object]
  - `bigint` [boolean] 返回的 `fs.Stats`对象中的数值是否应为 `bigint` 型。**默认值:** `false`。
- `callback` [Function]
  - `err` [error]
  - `stats` [`fs.stats`]

读取文件的状态 。 回调有两个参数 `(err, stats)`，其中 `stats` 是一个 [`fs.Stats`]对象。

不建议在调用 `fs.open()`、 `fs.readFile()` 或 `fs.writeFile()` 之前使用 `fs.stat()` 检查文件是否存在。 而是应该直接打开、读取或写入文件，如果文件不可用则处理引发的错误。

要检查文件是否存在但随后并不对其进行操作，则建议使用 [`fs.access()`]。



###  5、fs.Stats 类

`fs.Stats` 对象提供有关文件的信息。

从 [`fs.stat()`]、[`fs.lstat()`] 和 [`fs.fstat()`]及其同步方法返回的对象都属于此类型。 如果传给这些方法的 `options` 中的 `bigint` 为 true，则数值将为 `bigint` 型而不是 `number` 型。

















## 七、buffer - 缓冲器

**`Buffer` 类在全局作用域中，因此无需使用 `require('buffer').Buffer`。**

```txt
node中都是utf8  在utf8中一个汉字占三个字节
位     byte
字节     1Bit = 8byte
千字节     1KB = 1024Bit
兆     1MB = 1024KB
G     1GB = 1024MB
T     1T = 1024G
P     1P = 1024T
E     1E = 1024P
Z     1Z = 1024E
```

`toString()`没有写东西，默认就是utf8

0x打头表示一个16进制的数  `(0xe4).toString(2)` 把16进制转成2进制



### 1、Buffer.from(array)

- `array` 

使用八位字节数组 `array` 分配一个新的 `Buffer`。

```js
let a = Buffer.from([1,2,3,4,55,10,11,20]);
let b = Buffer.from(a)
console.log(a) 
console.log(b)     
//返回结果：    <Buffer 01 02 03 04 37 0a 0b 14>   
//             <Buffer 01 02 03 04 37 0a 0b 14>
```



### 2、Buffer.from(String)

**`Buffer.from(string[, encoding])`**

- `string`要编码的字符串。
- `encoding` : `string` 的字符编码。**默认值:** `'utf8'`。

创建一个包含 `string` 的新 `Buffer`。 `encoding` 参数指定 `string` 的字符编码。

```js
let a = Buffer.from('你好',"utf8");		//utf8为默认值，可写可不写
console.log(a)
console.log(a.toString())
//返回结果：    <Buffer e4 bd a0 e5 a5 bd>
//             你好     
```



### 3、Buffer.alloc

**`Buffer.alloc(size[, fill[, encoding]])`**

- `size` 新 `Buffer` 的所需长度。
- `fill`  用于预填充新 `Buffer` 的值。**默认值:** `0`。
- `encoding` 如果 `fill` 是一个字符串，则这是它的字符编码。**默认值:** `'utf8'`。

分配一个大小为 `size` 字节的新 `Buffer`。 如果 `fill` 为 `undefined`，则用零填充 `Buffer`。

```js
let a = Buffer.alloc(4);
console.log(a)    
//返回结果：   <Buffer 00 00 00 00>
```



### 4、Buffer.allocUnsafe

**`Buffer.allocUnsafe(size)`**

- `size`)新建的 `Buffer` 的长度。

创建一个大小为 `size` 字节的新 `Buffer`。 如果 `size` 大于 `buffer.constants.MAX_LENGTH`或小于 0，则抛出 `ERR_INVALID_OPT_VALUE`。 如果 `size` 为 0，则创建一个长度为零的 `Buffer`。

```js
let a = Buffer.allocUnsafe(4);
console.log(a)    
//返回结果：   <Buffer 08 07 00 00>
```



### 5、Buffer.concat

**`Buffer.concat(list[, totalLength])`**

- `list`  要合并的 `Buffer` 数组或 `Uint8Array` 数组。
- `totalLength` 合并后 `list` 中的 `Buffer` 实例的总长度。
- 返回:一个新`Buffer`

返回一个合并了 `list` 中所有 `Buffer` 实例的新 `Buffer`。

如果 `list` 中没有元素、或 `totalLength` 为 0，则返回一个长度为 0 的 `Buffer`。

如果没有提供 `totalLength`，则计算 `list` 中的 `Buffer` 实例的总长度。 但是这会导致执行额外的循环用于计算 `totalLength`，因此如果已知长度，则明确提供长度会更快。

如果提供了 `totalLength`，则会强制转换为无符号整数。 如果 `list` 中的 `Buffer` 合并后的总长度大于 `totalLength`，则结果会被截断到 `totalLength` 的长度。

```js
let b1 = Buffer.from("中")
let b2 = Buffer.from("国")
let c = Buffer.concat([b1,b2])  
console.log(c)  
console.log(c.toString()) 
//返回结果： <Buffer e4 b8 ad e5 9b bd>   
//          中国
```



## 八、querystring - 查询字符串

`querystring` 模块提供用于解析和格式化 URL 查询字符串的实用工具。 它可以使用以下方式访问：

```js
const querystring = require('querystring');
```



### 1、querystring.parse

**`querystring.parse(str[, sep[, eq[, options]]])`**

- `str` 要解析的 URL 查询字符串。
- `sep`  用于在查询字符串中分隔键值对的子字符串。**默认值:** `'&'`。
- `eq`  用于在查询字符串中分隔键和值的子字符串。**默认值:** `'='`。
- `options`
  - `decodeURIComponent` 解码查询字符串中的百分比编码字符时使用的函数。**默认值:** `querystring.unescape()`。
  - `maxKeys` 指定要解析的键的最大数量。指定 `0` 可移除键的计数限制。**默认值:** `1000`。

`querystring.parse()` 方法将 URL 查询字符串 `str` 解析为键值对的集合。

```js
let querystring = require("querystring")
let a = querystring.parse("flag=asd & qwe=jkl & zxc=uio")
let b = querystring.parse("foo=abc & abc=12 & abc=asd");
console.log(a)    
console.log(b) 
//返回结果：
//      [Object: null prototype] { flag: 'asd ', ' qwe': 'jkl ', ' zxc': 'uio' }
//      [Object: null prototype] { foo: 'abc ', ' abc': [ '12 ', 'asd' ] }
```





## 九、events - 事件触发器

大多数 Node.js 核心 API 构建于惯用的异步事件驱动架构，其中某些类型的对象（又称触发器，Emitter）会触发命名事件来调用函数（又称监听器，Listener）。

所有能触发事件的对象都是 `EventEmitter` 类的实例。 这些对象有一个 `eventEmitter.on()` 函数，用于将一个或多个函数绑定到命名事件上。 事件的命名通常是驼峰式的字符串，但也可以使用任何有效的 JavaScript 属性键。。

当 `EventEmitter` 对象触发一个事件时，所有绑定在该事件上的函数都会被同步地调用。 被调用的监听器返回的任何值都将会被忽略并丢弃。

例子，一个简单的 `EventEmitter` 实例，绑定了一个监听器。 `eventEmitter.on()` 用于注册监听器， `eventEmitter.emit()` 用于触发事件。

```js
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();

myEmitter.on('event', () => {
  console.log('触发事件');
});
myEmitter.emit('event');	//结果：  触发事件
```



```js
// node是基于事件驱动的  events
// let EventEmitter = require("events")

class EventEmitter{
    constructor(){
        this._events = {}
    }
    on(evenName,callback){
        if(this._events[evenName]){
            this._events[evenName].push(callback)
        }else{
            this._events[evenName] = [callback]
        }
    }
    emit(eventName){
        this._events[eventName].forEach(fn=>{
            fn()
        })
    }
}

let e = new EventEmitter()
e.on("看报纸",function(){
    console.log("正在看报纸1...")
})
```

`eventEmitter.emit()` 方法可以传任意数量的参数到监听器函数。 当监听器函数被调用时， `this` 关键词会被指向监听器所绑定的 `EventEmitter` 实例。也可以使用 ES6 的箭头函数作为监听器。但 `this` 关键词不会指向 `EventEmitter` 实例：

`EventEmitter` 会按照监听器注册的顺序同步地调用所有监听器。 所以必须确保事件的排序正确，且避免竞态条件。













