# Express

Express 基于 Node.js 平台，快速、开放、极简的 web 开发框架。

1. 安装Express

   `npm i express`

## 1、创建一个Express应用

```js
//载入express
const express = require("express")
//利用express创建一个应用
const app = express()
//提供一个http服务
app.get("/",function(req,res){
    res.end("hello world")
})
//监听端口
app.listen(3000,()=>{
    console.log("启动")
})

//当访问这个端口时，就会打印出hello world
```



## 二、应用生成器

在一个项目中，有css文件，js文件，图片，等等一些资源，我们可以通过应用生成器，去生成一个项目的大概结构。

### 1、安装生成器

全局安装生成器： `npm install -g express-generator`

查看全局安装的位置：`npm  root  -g`

### 2、创建应用

1. 在cmd窗口中输入`express 项目名` , 创建项目 

2. 然后需要安装依赖包，进入项目`cd 项目名` , 然后 `npm install`

3. 通过`npm start` 启动项目

4. 之后就可以通过设置的端口访问项目，默认为3000



## 三、路由

### 1、基本路由

**什么是路由？**

官方解释：路由是指如何定义应用的端点（URIs）以及如何响应客户端的请求。

民间解释：给一个请求，这个请求交给谁来处理，交给的过程就是路由。

**在express中，有两种实现路由的方式：**

  1，针对应用级别的，就是在app对象上设置

  2，针对Router实例对象的

例如：（1）、针对应用级别的路由

```js
const express = require("express")
const app = express()
app.get("/",function(req,res){
    res.end("hello world")
})
app.get("/index",function(req,res){
    res.end("主页面")
})
app.get("/login",function(req,res){
    res.end("登陆页面")
})
app.get("/my",function(req,res){
    res.end("个人中心")
})

app.listen(3000,()=>{
    console.log("启动")
})
```

（2）、Router实例对象的路由

```js
const express = require("express")
const router = express.Router()
router.get("/",function(req,res,next){
    res.render("hello world",{title:'express'})
})

module.exports = router;
```

**总结：**

1，对于上面的两种路由方式，重点掌握第一种。

2，创建一个针对应用级别的路由，是分步骤：

​	a,  通过express()创建一个app实例

​	b,  METHOD是一个HTTP的请求方法，如get请求或post请求， app.get(), app.post()

​	c,  path是**服务器**上的路径，是url中的路径部分，如 “/”  “/user”

​	d,  callback当路由匹配成功是要执行一个函数，在这个函数中有两个非常重要的参数，req，res,  req是指		   	  	incommingMessage, res是指serverResponse

 

### 2、路由方式

**什么是路由？**

对于一个请求，交给谁来处理，交给的过程就是路由。

路由的方式，不只有get， post还有很多，Express 定义了如下和 HTTP 请求对应的路由方法： **get**, **post**, put, head, delete, options, trace, copy, lock, mkcol,move, purge, propfind, proppatch, unlock, report, mkactivity, checkout, merge,m-search, notify, subscribe, unsubscribe, patch, search, 和 connect。

**一般常见的路由方式有两种：**

* get
* post

**哪些是get请求，哪些是post请求？**

* get:   直接输入一个网址，href，src，表单（method=”get”）
* post: 就是表单 通过设置method=”post”



### 3、路由句柄

路由句柄就是一个回调函数。如下：

**对于这个路由句柄，它必须要有两个参数，req, res.**

* req是指incommingmessage，它表示一些请求信息，这个信息，我们只能获取，不能修改。

* res是指serverResponse，它表示响应对象，可以做服务器上的任何事情。

对于一个路由句柄，可以设置多个。想让第二个回调函数起作用，那么就要用到第三个参数，叫next, 需要在第一个回调函数中，调用next()，就会执行下一个回调函数。我们还可以在第二个回调函数中，指定next， 这样的话，可以让第三个回调函数起作用。

```js
const express = require("express")
const app = express()
app.get("/",function(req,res,next){
    console.log("第一个")
    next();
},function(req,res,next){
    console.log("第二个")
    next();
},function(req,res,next){
    console.log("第三个")
    next();
})

app.listen(3000,()=>{
    console.log("启动")
})
//结果：启动	第一个	第二个	第三个
```



### 4、回调函数的两个参数

#### 1、req

req是作为回调函数的第一个参数 , 

req是incommingMessage对象，它和原生node中req还不太一样，它对原生中的req又进行一次封装，在原来的基础上，又增加了一些属性和方法：

##### 	1、query

可以将查询字符串变成对象

##### 	2、path

可以得到路径名

#### 2、res

作为回调函数的第二个参数

是指ServerRespose对象，响应对象，它和原生node中res还不太一样，它对原生中的res又进行一次封装，在原来的基础上，又增加了一些属性和方法：

##### 		1、send

直接发送响应的内容，可以是普通字符串，也可以是html标签，不需要写其它的头信息：

##### 		2、sendFile

**`res.sendFile(path)`**

用来发送文件，可以用来发送一个html文件，

##### 		3、json

**`res.json(string)`**

可以用来发送一个json格式的字符串：

##### 		4、render

##### 		5、download

##### 		6、redirect

**`res.redirect("path")`**

重定向



## 四、中间件

Express 是一个自身功能极简，完全是由路由和中间件构成一个的 web 开发框架：从本质上来说，一个 Express 应用就是在调用各种中间件。

**官方**：中间件（Middleware） 是一个函数，它可以访问请求对象（request object (req)）, 响应对象（response object (res)）, 和 web 应用中处于请求-响应循环流程中的中间件，一般被命名为 next 的变量。

**民间**：所谓的中间件，就是指一个回调函数，在这个回调函数中，有三个参数，**`req, res, next`**。它和上午所说的路由句柄差不多。

**中间件可以做什么？**

* 执行任何代码。
* 修改请求和响应对象。 
* 终结请求-响应循环。  
* 调用堆栈中的下一个中间件

**在express中都有哪些中间件？**

* 应用级中间件  
* 路由级中间件 
* 错误处理中间件  
* 内置中间件 
* 第三方中间件

一个 Express 应用就是在调用各种中间件。路由也属于中间件。

### 1、应用级中间件

所谓的应用级别的，就是指通过app对象来调用。

**如何使用应用级别中间件？**

* app.use([path]);

* app.METHOD( );  METHOD是指get, post

对于app.use()方式的中间件，如果说没有写path，就说明所有的请求都会使用这个中间件。

对于app.METHOD,实际上就是路由，从这个方面来说，路由也是中间件的一种。

```js
const express = require("express")
const app = express()
app.use("/",function(req,res,next){
    console.log("2222")
    next();
})
app.use("/home",function(req,res,next){
    console.log("1111")
    next();
})
app.get("/",function(req,res,next){
    console.log(3333)
    res.send("0000000")
})

app.listen(3000,()=>{
    console.log("启动")
})

//在访问localhost:3000/home时， 会打印出2222 1111 ，也就是说在访问/home时也会同时访问到/  
//在访问localhost:3000时，会打印出2222 3333 ，不会执行路径为/home的app.use
//如果两个app.use访问的地址相同的话，谁在前面先执行谁
//如果没有路径或者路径为"*"的话就表示所有的路由都会匹配到
```



### 2、路由级中间件

路由级别的是中间件，是指由express.Router对象来调用的。

```js
const express = require("express")
const app = express()
const router = express.Router()
//在app中添加一个router中间件
app.use(router)
//通过router对象实现路由处理
router.get("/home",function(req,res){
    res.send("<h1>路由级中间件</h1>")
})
app.listen(3000,()=>{
    console.log("启动")
})

//访问网址后在页面打印出路由级中间件
```



### 3、错误处理中间件

错误处理中间件和其他中间件定义类似，只是要使用 4 个参数，而不是 3 个，其签名如下：` (err, req, res, next)`。

```js
const express = require("express")
const app = express()

app.use("/",function(req,res,next){
    console.log("1111")
    next(new Error("错了"));		//抛出错误
})
app.use("/",function(err,req,res,next){		//错误处理，多了个参数err
    console.log(err)
    next();
})
app.get("/",function(req,res,next){		//这个函数依然会被调用
    console.log(3333)
    res.send("0000000")
})
app.listen(3000,()=>{
    console.log("启动")
})

```



### 4、内置中间件

Express 中唯一内置的中间件函数是 `express.static`。此函数基于 `serve-static`，负责提供 Express 应用程序的静态资源。

**`express.static(root, [options])`**

* `root` 自变量指定从其中提供静态资源的根目录。

* 可选的 `options` 对象可以具有以下属性：
  * dotfiles	是否对外输出文件名以点（.）开头的文件。有效值包括“allow”、“deny”和“ignore”
  * etag  启用或禁用 etag 生成
  * extensions   用于设置后备文件扩展名
  * index  发送目录索引文件。设置为 `false` 可禁用建立目录索引。
  * lastModified     将 `Last-Modified` 的头设置为操作系统上该文件的上次修改日期。有效值包括 `true` 或 `false`。
  * maxAge    设置 Cache-Control 头的 max-age 属性
  * redirect   路径名是目录时重定向到结尾的“/”
  * setHeaders   用于设置随文件一起提供的 HTTP 头的函数。





















































































