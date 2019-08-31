# koa

Koa 是一个新的 web 框架，由 Express 幕后的原班人马打造， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。 通过利用 async 函数，Koa 帮你丢弃回调函数，并有力地增强错误处理。 Koa 并没有捆绑任何中间件， 而是提供了一套优雅的方法，帮助您快速而愉快地编写服务端应用程序。



## 1、安装

Koa 依赖 **node v7.6.0** 或 ES2015及更高版本和 async 方法支持.

你可以使用自己喜欢的版本管理器快速安装支持的 node 版本：

```
$ npm i koa			//目前主要用这个
```



## 2、应用

```js
const Koa = require('koa');			//引入koa
const app = new Koa();				//创建koa实例

app.use(ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

**在 koa 中全部是中间件 ，ctx 表示上下文 ，next 是中间件必有,用来表示下一个中间件执行（next()会返回Promise对象）**

`ctx`作为上下文使用,包括基本的`ctx.request`和`ctx.response`

另外**koa**通过`delegates`这个库对`request, response`一些常用属性或者方法,做了很多代理操作,可以直接通过`ctx`访问得到,比如`request.url`可以写成`ctx.url`。

**除此之外,**koa**还约定了一个中间件的存储空间`ctx.state`通过这个state可以储存一些的数据,比如用户数据**

另外类似`koa-views`这些渲染**view**层的中间件也会默认把`ctx.state`里面的属性作为**view**的上下文传入。




### 1、koa 封装：request  response

```js
const Koa = require("koa")
let app = new Koa()
app.use((ctx,next)=>{
    console.log(ctx.request)	//表示去拿koa封装的请求对象
    console.log(ctx.response) 	//表示去拿koa封装的响应对象
})
app.listen(3000)

// 原生的：ctx.req.xxx   ctx.res.xxx
// koa封装的：ctx.request.xxx	ctx.response.xxx 
// koa建议使用koa封装的
```

返回结果：

```js

-----------------这是koa封装的request
{ method: 'GET',		//请求方法为get
  url: '/',				//这是请求的地址
  header:				//请求头
  { host: 'localhost:3000',		
   connection: 'keep-alive',
   'cache-control': 'max-age=0',
   'upgrade-insecure-requests': '1',
   'user-agent':
   'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko)        Chrome/76.0.3809.100 Safari/537.36',
   'sec-fetch-mode': 'navigate',
   'sec-fetch-user': '?1',
   accept:
 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,a   pplication/signed-exchange;v=b3',
   'sec-fetch-site': 'none',
   'accept-encoding': 'gzip, deflate, br',
   'accept-language': 'zh-CN,zh;q=0.9' } }

---------------这是koa封装的response
{ status: 404,				//响应状态码
  message: 'Not Found',
  header: [Object: null prototype] {},
  body: undefined }			//请求主体

```

### 2、一些应用

```js
    console.log(ctx.req.url) // 通过原生的方式获取url
    console.log(ctx.request.url) // 通过koa封装的request对象来获取url
    console.log(ctx.request.req.url) // 通过koa封装的request对象得到原生的req对象，再得到url
    console.log(ctx.url) // 可以直接通过上下文得到url

    console.log(ctx.request.path) // 通过koa封装的request对象来获取path
    console.log(ctx.path) // 可以直接通过上下文得到path
    console.log(ctx.req.path)  // 会返回undeind,因为在原生的Node中,req上面是没有path

    ctx.response.res.end("<h1>hello</h1>")
    // ctx.response.end("<h1>你好</h1>")  // ctx.response中没有end方法
    ctx.res.end("hello3")
    ctx.response.body = "hello1"
    ctx.body = "hello2"
```

#### 1、ctx.query

**用来获取get请求参数，返回一个对象**

```
ctx.query;  //{ aid: '123' }       获取的是对象   用的最多的方式  **推荐
ctx.querystring;  //aid=123&name=zhangsan      获取的是一个字符串
ctx.url;   //获取url地址
 
//ctx里面的request里面获取get传值
 
ctx.request.url;	
ctx.request.query;   //{ aid: '123', name: 'zhangsan' }  对象
ctx.request.querystring;   //aid=123&name=zhangsan

```







## 3、中间件

### 1、异步中间件

```js
const Koa = require("koa")
const app = new Koa()
app.use(async (ctx,next)=>{
    console.log(1)
    // await 后面是异步
    await next()
    console.log(2)
})
app.use((ctx,next)=>{
    setTimeout(function(){
        console.log(3)
    },3000)
})
app.listen(3030)

//	结果：1 2 3
```

### 2、调用中间件

直接调用会返回一个Promise

```js
const Koa = require("koa")
const app = new Koa()
app.use((ctx,next)=>{
    // 调用一个中间件，返回promise
    let a = next()  // a是一个promise
    console.log(a) // Promise { 'hello' }
})
app.use((ctx,next)=>{
    return "hello"
})
app.listen(3030)
```

在一个中间件中，利用async+await --->  仅仅是把Promise状态 转化为普通值

```js
const Koa = require("koa")
const app = new Koa()
app.use(async (ctx,next)=>{
    // 调用一个中间件，返回promise
    let a = await next()  // a是一个promise
    console.log(a) 		  //'hello'
})
app.use((ctx,next)=>{
    return "hello"
})
app.listen(3030)
```

### 3、特性

在koa中，中间件也是从上向下执行的

```js
const Koa = require('koa');			//引入koa
const app = new Koa();				//创建koa实例

app.use((ctx,next)=>{
    console.log(1)
    console.log(2)
    next();
})
app.use((ctx,next)=>{
    console.log(3)
    console.log(4)
    next()
})
app.use((ctx,next)=>{
    console.log(5)
    console.log(6)
})
app.listen(3000);
//输出：1 2 3 4 5 6
```

但koa中间件的原理 和 express中间件的原理不一样

```js
const Koa = require('koa');			//引入koa
const app = new Koa();				//创建koa实例

app.use((ctx,next)=>{
    console.log(1)
    next()
    console.log(2)
})
app.use((ctx,next)=>{
    console.log(3)
    next()
    console.log(4)
})
app.use((ctx,next)=>{
    console.log(5)
    console.log(6)
})
app.listen(3000);

//输出：1 3 5 6 4 2
```



### 4、原理：

#### 1、简单原理:

```js
let app = {
    middlewares:[],
    use(fn){
        this.middlewares.push(fn)
    }
}
app.use((next)=>{
    console.log(1)
    console.log(2)
    next()
})
app.use((next)=>{
    console.log(3)
    console.log(4)
    next()
})
app.use((next)=>{
    console.log(5)
    console.log(6)
    next()
})
// koa中间件的原理
function dispatch(index){
    if(app.middlewares.length === index) return;
    let route = app.middlewares[index]
    route(()=>{dispatch(index+1)})
}
dispatch(0)
//输出：1 3 5 6 4 2
```

#### 2、简单原理升级：

```js
let app = {
    middlewares:[],
    use(fn){
        this.middlewares.push(fn)
    }
}
app.use((next)=>{
    console.log(1)
    console.log(2)
    next()
})
app.use((next)=>{
    console.log(3)
    console.log(4)
    next()
})
app.use((next)=>{
    console.log(5)
    console.log(6)
})
function compose(middlewares){
    return middlewares.reduceRight((a,b)=>()=>b(a))
}
let fn = compose(app.middlewares)
fn()	//结果：1 2 3 4 5 6
```

#### 3、简单原理再次升级：

```js
let app = {
    middlewares:[],
    use(fn){
        this.middlewares.push(fn)
    }
}
app.use((next)=>{
    console.log(1)
    console.log(2)
    next()
})
app.use((next)=>{
    console.log(3)
    console.log(4)
    next()
})
app.use((next)=>{
    console.log(5)
    console.log(6)
})
function compose(middlewares){
    return middlewares.reduce(function(a,b){
        return function(arg){
            return a(function(){
                return b(arg)
            })
        }
      
    })
    //以上函数等价于下式：
    // return middlewares.reduce((a,b)=>(arg)=>a(()=>b(arg)))
}
let fn = compose(app.middlewares);
fn(()=>{})
```



## 4、GET请求使用方法

在koa中，获取GET请求数据源头是koa中`request`对象中的`query`方法或`querystring`方法，`query`返回是格式化好的参数对象，`querystring`返回的是请求字符串，由于ctx对`request`的API有直接引用的方式，所以获取GET请求数据有两个途径。

是从上下文中直接获取
请求对象`ctx.query`，返回如` { a:1, b:2 }`
请求字符串 `ctx.querystring`，返回如` a=1&b=2`

是从上下文的request对象中获取
请求对象`ctx.request.query`，返回如` { a:1, b:2 }`
请求字符串 `ctx.request.querystring`，返回如`a=1&b=2`



## 5、第三方模块



### 1、koa-compose

安装：

```
npm install koa-compose
```

作用：组合给定的中间件并返回中间件。（将多个中间件合并成单一中间件,方便重用和导出）

示例：

```js
const Koa = require("koa")
const compose = require("koa-compose")
const app = new Koa()
let f1 = async (ctx,next)=>{
    console.log("f1")
    await next()
}
let f2 = async (ctx,next)=>{
    console.log("f2")
    await next()
}
let f3 = async (ctx,next)=>{
    console.log("f3")
    await next()
}

//注意此处，使用方法
let all = compose([f1,f2,f3])
app.use(all)	

app.listen(3030)

//结果：f1 f2 f3
```



### 2、koa-bodyparser

安装：

```
npm install koa-bodyparser
```

使用方法：

```
ctx.request.body
```

作用：`koa-bodeyparser`可以把POST的参数解析到`ctx.request.body`中。

示例：

```js
const Koa = require("koa")
var bodyParser = require('koa-bodyparser');     //引入koa-bodyparser模块
var app = new Koa();
app.use(bodyParser());  // 使用中间件

// ()=>{}  当访问 / 时，调用它
app.use((ctx,next)=>{
    //console.log(ctx.request.body)  //这里显示的是post提交的数据
    ctx.body = ctx.request.body     //表示将提交的数据在页面上显示出来
})

app.listen(3030)
```



### 3、koa-router

安装：

```
npm install koa-router
```

作用：加上`koa-router`中间件后我们可以直接使用 get 和 post 来定位并且减少代码量

示例：

```js
const Koa = require("koa")
const Router = require('koa-router');
let app = new Koa();
let router = new Router();

app.use(router.routes())			//加载router中间件
app.use(router.allowedMethods());	//对异常码处理

let index = require(./public/index)	
router.use("./index",index)			//此处这两行表示将引入的index页面加上路由

router.get("/",(ctx,next)=>{
    ctx.body = "首页"
})
router.get("/my",(ctx,next)=>{
    ctx.body = "个人中心"
})
router.get("/setting",(ctx,next)=>{
    ctx.body = "设置"
})
app.listen(3030) 
```



### 4、koa-static

安装：

```
npm install koa-static
```

作用：用来加载静态资源

示例：

```js
const Koa = require("koa")
const static = require('koa-static');
var app = new Koa();

app.use(static(__dirname + '/public'));

app.listen(3030)

//在public文件夹中可以放一些静态资源，如html、css、js文件，这样的话在访问3030端口时，页面会将其加载为页面
```



### 5、koa-views

安装：( 注意不是koa-view )

```
npm install koa-views
```

作用：把数据渲染到模板中，然后把模板返回浏览器

示例：

```js
var views = require('koa-views');
 
// Must be used before any router is used
app.use(views(__dirname + '/views', {
  map: {
    html: 'underscore'
  }
}));
 
app.use(async function (ctx) {
  ctx.state = {
    session: this.session,
    title: 'app'
  };
 
  await ctx.render('user', {
    user: 'John'
  });
});
```



### 6、koa-art-template

安装：

```
npm i art-template
npm i koa-art-template		// 因为koa-art-template依赖于art-template
```

作用：配置模板引擎

示例：

```js
const Koa = require('koa');
const render = require('koa-art-template');
 
const app = new Koa();
render(app, {
  root: path.join(__dirname, 'view'),				//配置模板路径
  extname: '.html',									//设置扩展名
  debug: process.env.NODE_ENV !== 'production'		//是否开启调试模式
});
 
app.use(async ctx=>{
  await ctx.render('user');		//表示此处模板路径为 __dirname+view/user
});
 
app.listen(8080);
```

**渲染语法：**

1、子模版继承

```
{{include "../admin/public/"}}
```

2、输出（类似于vue)

```
<sapn>这是span图标 {{title}} </span>
```

3、循环

```
<ul>
	{{each list}}
	<li>
		{{$index}}
		{{$value.a}}
	</li>
	{{/each}}
</ul>
```

4、if 语句

```
{{if 条件}} 内容 {{/if}}
```

5、输出原文

```
{{@htmlH}}
```



### 7、koa-session

安装：

```
npm install koa-session
```

作用：

（官网）示例：

```js
const Koa = require('koa');
const session = require('koa-session');
const app = new Koa();
//配置session中间件	//如果使用这个模块的话就要加上下面的
app.keys = ['some secret hurr'];
const CONFIG = {		
  key: 'koa:sess', 		//cookie密钥（默认为koa：sess）
  maxAge: 86400000,		//以毫秒为单位（默认为一天），过期时间
  autoCommit: true, 	//自动提交头文件（默认为true）
  overwrite: true, 		//可以覆盖或不覆盖（默认为true）
  httpOnly: true,		//httpOnly与否（默认为true）
  signed: true, 		//是否签名（默认为true）
  rolling: false, 		//强制在每个响应上设置会话标识符cookie。到期时间重置为原始maxAge，重置到期倒计时。（默认为false）
  renew: false, 		//更新会话，因此我们可以始终保持用户登录。（默认为false）
};
app.use(session(CONFIG, app));		//这个也要加上



app.use(ctx => {
  if (ctx.path === '/favicon.ico') return;
  let n = ctx.session.views || 0;
  ctx.session.views = ++n;
  ctx.body = n + ' views';
});
 
app.listen(3000);
console.log('listening on port 3000');
```

**cookie **

cookie 是储于访问者的计算机中的变量，当访问一个服务器时，服务器会给浏览器返回一堆信息，
这一堆信息存储在cookie中，服务器给浏览器种植cookie。再去请求服务器，会带上cookie。

**session **

session是另一种记录客户状态的机制，不同的是Cookie 保存在客户端浏览器中，而
session 保存在服务器上。
当浏览器访问服务器并发送第一次请求时，服务器端会创建一个session 对象，生
成一个类似于 key : value 的键值对， 然后将 key(cookie) 返回到浏览器(客户)端，浏览
器下次再访问时，携带 key(cookie) ，找到对应的 session(value) 。客户的信息都保存
在 session 中。
	1、cookie 数据存放在客户的浏览器上，session 数据放在服务器上。
	2、cookie 不是很安全，别人可以分析存放在本地的 COOKIE 并进行 COOKIE 欺骗
考虑到安全应当使用session。
	3、session 会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能
考虑到减轻服务器性能方面，应当使用 COOKIE。
	4、单个cookie 保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20 个cookie。



### 8、koa-jsonp

一个  koajs  支持GET / POST请求 JSONP 流的友好 JSONP 中间件。







### 9、koa-multer

可以使用`koa-multer`进行文件的上传和表单的提交,主要是可以用来上传图片

#### 1、安装：

```
npm i koa-router
```

#### 2、使用：

```js
//引入
const multer = require('koa-multer');

//以下是配置
var storage = multer.diskStorage({
	//定义文件保存路径
	destination:function(req,file,cb){
	    cb(null,'./upload/');//null不用管，后面的路径根据具体情况而定。如果不存在的话会自动创建一个路径
	},                       //注意这里有个，
	//修改文件名
	filename:function(req,file,cb){
	    var fileFormat = (file.originalname).split(".");
    	    cb(null,Date.now() + "." + fileFormat[fileFormat.length - 1]);
	}
})
 
var upload = multer({ storage: storage });

```

#### 3、接收

在后端接收时必须是post

``` 
router.post("doAdd", upload.single('img_url'),  async ctx => { } )
```

其中的` img_url `必需与 提交图片 的input输入框的 name 值保持一致，
即 

```
<input type="file" id="file" name="img_url" class="col-xs-10 col-sm-5" />	
```

**注意，在这里要加上 `upload.single('img_url')`这句话**

前端form表单上必须要加**`enctype="multipart/form-data"` ,**后端需要创建一个文件夹，用来接收文件



### 10、富文本编辑器

#### 1、安装

```
npm i koa2-ueditor
```

#### 2、引用与配置

```
const ueditor = require("koa2-ueditor")   //引入富文本编辑器

//配置富文本编辑器
router.all('/editor/controller', ueditor(['public', {
   "imageAllowFiles": [".png", ".jpg", ".jpeg"],
   "imagePathFormat": "/upload/ueditor/image/{yyyy}{mm}{dd}/{filename}"  // 保存为原文件名
}]))
```

#### 3、引入静态文件

在模板中引入静态资源，把官网的静态资源放在public文件夹下

```
	<!-- 引入富文本编辑器相关的js -->
	<script type="text/javascript" charset="utf-8" src="/ueditor/ueditor.config.js"></script>
	<script type="text/javascript" charset="utf-8" src="/ueditor/ueditor.all.min.js"> </script>
	<script type="text/javascript" charset="utf-8" src="/ueditor/lang/zh-cn/zh-cn.js"></script>
```

#### 4、添加标签，显示

在需要添加富文本编辑器的地方写入如下标签

```
<script name="content" id="editor" type="text/plain" style="width:600px;height:300px;"></script>
```

























































































