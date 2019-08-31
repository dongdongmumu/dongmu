# vue中的一些知识点



## 一、vue中 关于$emit的用法

1、父组件可以使用 props 把数据传给子组件。
2、子组件可以使用 $emit 触发父组件的自定义事件。

vm.$emit( event, arg ) //触发当前实例上的事件

vm.$on( event, fn );//监听event事件后运行 fn； 



## 二、拦截器

在请求或响应被 `then` 或 `catch` 处理前拦截它们。

```vue
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

如果你想在稍后移除拦截器，可以这样：

```
var myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

可以为自定义 axios 实例添加拦截器

```vue
var instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```

## 三、管道符 `|`

示例：`command 1 | command 2 `

管道符的功能是把第一个命令command 1执行的结果作为command 2的参数传给command 2，

