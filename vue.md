# `vue.js`

`Vue`(读音类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，`Vue`被设计为可以自底向上逐层应用。`Vue` 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，`Vue `也完全能够为复杂的单页应用提供驱动。



```txt
参考文档：

	掘金：夜夕i	https://juejin.im/post/5d529884f265da03a1484e56
	
	掘金：bbuh	 https://juejin.im/post/5b2b65d66fb9a00e420d8a82
	
	
```





## 一、vue 基础



### 1. vue.js 的基本架构

```vue
<div id="app">
    
</div>
<!-- 引入vue.js文件 -->
<script src="../js/vue.js"></script>
<script>
    var app = new Vue({
        el:"#app",
        data:{
            
        }
    })
</script>
```
### 2. vue的一些指令

指令（Directives）是**写在html标签上**的带有 **v-** 前缀的特殊属性。指令的职责就是当其表达式的值改变时相应地将某些行为应用到 DOM 上。

常用指令包括如下:

-  数据绑定指令:把数据作为元素的内容显示出来。

> ​	**v-text**	
>
> ​	**v-html**	
>
> ​	**v-model**	

-  属性指令：把数据设置成为元素的属性.例class,src等。

> ​	**v-bind**	

- 条件指令:

> ​	**v-if	 v-elseif	 v-else**	
>
> ​	**v-show**	

-  循环指令

> ​	**v-for**	



#### 1.  v-text 

`v-text` 等价于插值语法{{ }}。只能解析文本，将解析之后的文本作为标签的内容。

```vue
<p v-text="ad"></p>   ==  <p>{{ad}}</p>			

data:{ad:"abcdefg"}
```



#### 2. v-html

  与`v-text`类似。但它可以解析html标签。

```vue
<p v-html="ac">	</p>    

data：{ac:"<h1>这是一个标题</h1>"}
```



#### 3. v-model

实现双向数据绑定（v-model 是  @input + :value 的一个语法糖）

对标签有要求。它不能加在span标签上：因为span不能获取用户的输入，用户不能直接去修改span的内容。

它更加适应加在表单元素上。例如 input。用户可以去更改input的内容。

```vue
p>{{ad}}</p>
<input v-model="ad" type="text"/>			//data:{ad:"初始值，也可以为空"}
```



#### 4. v-bind

<标签 **v-bind:**属性名=”data配置项设置的属性名 ”>

作用：当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM

​	有两个属性因为用的非常的多，所以它单独地设置了特殊用法：

​						style				Class

```vue
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>
```



#### 5. v-if

` v-if ` 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 true 的时候被渲染。

也可以用 `v-else` 添加一个“else 块”：

```vue
<p v-if="false">1111111</p>
<p v-else>22222222</p>

<!--v-else 必须紧跟在带v-if 或者 v-else-if 的元素的后面，否则它将不会被识别。可以使用 v-else-if 指令来形成嵌套条件循环-->

<p v-if="true">1111111</p>	

<!--v-if指令后的true或false也可用一个表达式来代替,但要用引号包起来-->
```

​			**因为 `v-if` 是一个指令，所以必须将它添加到一个元素上。但是如果想切换多个元素呢？此时可以把一个 `<template>` 元素当做不可见的包裹元素，并在上面使用 `v-if`。最终的渲染结果将不包含 `<template>` 元素。**





#### 6. v-show

用于根据条件展示元素的选项是 `v-show` 指令。用法与`v-if`大致一样

**不同的是带有 `v-show` 的元素始终会被渲染并保留在 DOM 中。`v-show` 只是简单地切换元素的 CSS 属性 `display`。**即`v-show=false`等于`display=none`

```vue
<p v-show="true">1111111</p>
```





#### 7. v-if 	VS 	v-show

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。



#### 8. v-for

**<标签 v-for=“(value,key,index) in obj”></标签>**

```vue
<p v-for="value in 10">{{value}}</p>		//从一到十
```

1. obj就是定义在data配置项中的数据。

2. 它会产生多个标签

3. Value,index,key只是形参名，可以更改，并根据需要，可按顺序省略

4. 可供循环的数据有：
   - 数组		对数组循环时，第三个参数没有用。
   - 对象
   - 整数         对整数循环时，从一开始，索引从0开始
   
   
   
   v-for 可用来循环数组 循环对象 整数
   
   ​        vue 2.5 版本以上，使用v-for，需要加上key  key要唯一
   
   ​        template上面是不能绑定key  key的作用：区分元素,现在一般都要加上key绑定
   
   尽量不要使用index做为key，index会在数据变化时产生一些问题，如果有唯一标示，尽量使用唯一标示。

#### 9. v-slot

Vue 2.6.0之后采用全新v-slot语法取代之前的slot、slot-scope



## 二、`vue`的配置项



### 1. 方法 `methods`

`New Vue()`会设置一些配置项，如data表示数据。这些配置项有一个就是methods,它是可选，可以写也可以不写。

它的作用就相当于你可以写一些的逻辑代码，像函数一样去调用。

​	调用方法一 ：在 `vue `实例 的外面，通过 `app.f1()`, `app.f2()` 去访问。

​	调用方法二 ：在 `vue` 实例内部，直接调用。	一般在{{}} 或者是v-bind中使用

​		**在一个方法中调用另一个方法，需要加上this**

```vue
<div id="app">
	{{f1()}}			//在 vue 实例内部，直接调用。
</div>



<script>
var app = new Vue({
	el: "#app",
	data: {
        
	},				//这里要用逗号隔开
	methods:{
        f1(){
            
        },			//各个函数之间也要用逗号隔开
        f2(){
            
        }
	}
})
</script>
```



### 2. 计算属性 `computed`

计算属性computed也是 vue 实例的配置项。声明式的描述一个值依赖了其它的值.

用法：一般是对初始数据（data中数据项）进行加工，处理，返回新的数据。

调用：计算属性本质上也是一个函数，但在调用时与普通数据项一样，不需要加 ()。

```vue
<script>
    Var vm = new Vue({
        el: ”#app”,
        data: {},
        methods: {},
        computed: {
            计算属性名: function () {
                return 值; //一定会有返回值
            }
        }
    })
</script>
```

​	**计算属性与方法之间的区别**：

1. 计算属性有缓存，函数没有缓存！

2. 函数比较敏感
3. 使用的区别 计算属性虽然本质也是方法，但它不加() ,方法要加()



### 3. 过滤器 `filters`

过滤器是用来格式化数据。与 `el` , `data `, `methods`, `coputed `一样，也是一个 `vue `实例的配置项。

格式：	**{{数据| 过滤器}}**

多个过滤器一起使用：

格式：**{{数据项 | 过滤器1 | 过滤器2 }}**

过滤器将数据在被指令处理并显示到视图之前进行转换，而**不必修改作用域中原有的数据**，这样能够允许同一份数据在应用的不同部分以不同形式得以展示。

```vue
<div id="app">
	<p>{{f|fixss(1)}}</p>
</div>
<!-- 引入vue.js文件 -->
<script src="../js/vue.js"></script>		
<script>
	var app = new Vue({
		el: "#app",
		data: {
			f: 3.14154,
		},
		methods: {},
		computed: {},
		filters: {
			fixss(d，n) {				
				return d.toFixed(n)
			}
		}
	})
</script>
```

在写过滤函数时，注意参数在传递的过程中的对应关系：

1. 第一个参数(必填)	是你一定要过滤的数据项。
2. 第二个参数(可选)	后面的参数就是你写在()中的实参

在调用时，如果有两个或两个以上的参数，第一个参数不需要填写，直填写第二个及以后的参数

 

过滤器的特殊之处在：

- 它的定义时，**至少要有一个参数**，而这个参数在调用时并不需要给实参：第一个参数默认的实参值就是你要过滤的数据项。
- 它**一定要有返回值 **, 这一点与计算属性是一致的。





## 三、事件	v-on



### 1.简介

1. **vue通过v-on指令处理事件**

可以用 `v-on` 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。

**格式**：	<**事件源**标签 v-on:**事件类型**=“**事件处理函数**”></事件源标签>

```vue
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>
<!-- 缩写 -->
<a @click="doSomething">...</a>
```

范例：

```vue
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>

<script>
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
</script>
```

然而许多事件处理逻辑会更为复杂，所以直接把 JavaScript 代码写在 `v-on` 指令中是不可行的。因此 `v-on` 还可以接收一个需要调用的方法名称。



### 2. 书写位置

1. 如果代码比较简单：直接写在行内

2. 如果代码比较复杂：在methods中新加一个方法，然后在v-on中调用



### 3. 事件类型	

- 鼠标事件，click，dblclick，mouseover，mouseout，mouseenter，mouseleave，mousedown，mousemove，mouseup
- 键盘事件，keydown，keypress，keyup，
- ui事件，scroll，resize，load，unload
- 焦点事件，focus，blur
- 表单事件，change，submit



在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

为了解决这个问题，Vue.js 为 `v-on` 提供了**事件修饰符**。

- `.stop`

- `.prevent`

- `.capture`

- `.self`

- `.once`

- `.passive`

  ```vue
  <!-- 阻止单击事件继续传播 -->
  <a v-on:click.stop="doThis"></a>
  
  <!-- 提交事件不再重载页面 -->
  <form v-on:submit.prevent="onSubmit"></form>
  
  <!-- 修饰符可以串联 -->
  <a v-on:click.stop.prevent="doThat"></a>
  
  <!-- 只有修饰符 -->
  <form v-on:submit.prevent></form>
  
  <!-- 添加事件监听器时使用事件捕获模式 -->
  <!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
  <div v-on:click.capture="doThis">...</div>
  
  <!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
  <!-- 即事件不是从内部元素触发的 -->
  <div v-on:click.self="doThat">...</div>
  ```

  使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止**所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。

**按键修饰符**

在监听键盘事件时，我们经常需要检查详细的按键。Vue 允许为 v-on 在监听键盘事件时添加按键修饰符：

```vue
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">

<!--你可以直接将 KeyboardEvent.key 暴露的任意有效按键名转换为 kebab-case 来作为修饰符。-->
<input v-on:keyup.page-down="onPageDown">
```

在上述示例中，处理函数只会在 $event.key 等于 PageDown 时被调用。

有如下按键修饰符：

.enter			.tab			.delete			.esc			.space			.up			.down			.left

.right			.ctrl  			.alt  				.shift  		.meta  



#### 1. `@input`

注意：在vue中，监听input输入事件，使用的是v-on指令。
v-on: input = “func” 或者简写为 @input = “func”

示例：

```vue

<input value="text" @input="onInput">
<script>
 //...
    
methods: {
    onInput(e) {
      console.log(e)
      //参数e是一个InputEvent对象，其中包含了各种关于input内容
      //这里的e.target指向DOM元素
      //这里的e.target.value就是输入框输入的值
      //this.$emit是为了在父元素能够调用onInput事件
      this.$emit("input", e.target.value);
 
    }
  }
</script>
```



示例中，给input输入框绑定一个input事件，在方法中设置 onInput ，其中，参数e就是









## 四、表单输入绑定



你可以用 `v-model` 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 `v-model` 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

**双向数据绑定：只有表单元素上，才会是双向的。**

**`v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 `data` 选项中声明初始值。**



`v-model` 在内部为不同的输入元素使用不同的属性并抛出不同的事件：

- text 和 textarea 元素使用 `value` 属性和 `input` 事件；
- checkbox 和 radio 使用 `checked` 属性和 `change` 事件；
- select 字段将 `value` 作为 prop 并将 `change` 作为事件。

### 1. 单选框

- 多个input:radio 的name要相同。这样才是单选
- 多个input:radio 的v-model要相同.
- 你设置的初值，必须要与input:radio中的value值相对应。

```vue
<!-- 性别选择并显示-->
<div id="app">
        <input type="radio" name="sex" v-model="add" value="男">男
        <input type="radio" name="sex" v-model="add" value="女">女
        <h1>{{add}}</h1>
    </div>
<!-- 引入vue.js文件 -->
<script src="../js/vue.js"></script>
    <script>
        var app = new Vue({
            el: "#app",
            data: {
                add:""
            },
            methods: {},
            computed: {},
            filters: {}
    })
</script>
```



### 2.复选框

一般有两种用法：

（1）只有一个复选框。用来表示 “同意”与否。表示单选，通常使用布尔值来表示。

```vue
<input type="checkbox" v-model="agree">
```

（2）多个复选框。表示在多个选择项中选择某一个，或者是几个。表示多选，通常使用数组来表示。例如，统计兴趣爱好，，可使用 **v-model** 使数据绑定



```vue
<!--是否同意按钮-->
<div id="app">
        <input type="checkbox" v-model="add" >同意
        <h1>{{f1()}}</h1>
</div>
<script src="../js/vue.js"></script>
<script>
	var app = new Vue({
		el: "#app",
		data: {
			add:"true"
		},
		methods: {
			f1(){return this.add?"同意":"不同意"}
		},
		computed: {},
		filters: {}
	})
</script>
```



```vue
<!--多选按钮-->

<div id="app">
	<template v-for="ass in allfav">
		<input v-model="info.favoriate" type="checkbox" :value="ass.value">{{ass.name}}
	</template>
	<hr/>
	<p>{{info}}</p>
</div>

<script src="../js/vue.js"></script>
<script>
	var app = new Vue({
	el: "#app",
	data: {
		allfav: [
			{name: "读书",value: "book"},
			{name: "唱歌",value: "song"},
			{name: "游戏",value: "game"},
			{name: "睡觉",value: "sleep"}
		],
		info: {favoriate: ["book"]}
	},
	methods: {},
	computed: {},
	filters: {}
	})
</script>
```



**不使用v-model双向绑定**

```html
<div id="app">
    <input type="text" :value="add" @input="abcd">

    <h2>输入内容：{{add}}</h2>
</div>
<!-- 引入vue.js文件 -->
<script src="./vue.js"></script>
<script>
    var vm = new Vue({
        data: {
            add: "aa",
            abb: ''
        },
        methods: {
            abcd(e) {
                this.add = e.target.value
            }
        }
    }).$mount('#app')
</script>
```



### 3. 下拉列表

使用`select`与`option`指令制作下拉列表

```vue
   <!--一个简单的下拉列表-->

	<div id="app">
        <select v-model="info.study" name="" id="">
            <option value="-请选择-">-请选择-</option>
            <option v-for="i in eduOption" v-model:value="i">{{i}}</option>
        </select>
        <hr/>
        <p>{{info}}</p>
    </div>
	<!-- 引入vue.js文件 -->
    <script src="../js/vue.js"></script>		

    <script>
        var app = new Vue({
            el: "#app",
            data: {
                info: {study:"-请选择-"},
                eduOption:["重点","一本","二本","三本"],
            },
            methods: {},
            computed: {},
            filters: {}
        })
    </script>
```



### 4、修饰符

#### `.lazy`

在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 (除了[上述](https://cn.vuejs.org/v2/guide/forms.html#vmodel-ime-tip)输入法组合文字时)。你可以添加 `lazy` 修饰符，从而转变为使用 `change` 事件进行同步：

```vue
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg" >
```

#### `.number`

如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符：

```vue
<input v-model.number="age" type="number">
```

这通常很有用，因为即使在 `type="number"` 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 `parseFloat()` 解析，则会返回原始的值。

#### `.trim`

如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符：

```vue
<input v-model.trim="msg">
```





## 五、 组件



在vue当中，一切都是组件。

组件就是我们自己扩展的 “HTML 元素”，封装可重用的代码。

在较高层面上，组件是自定义元素， Vue.js 的编译器为它添加特殊功能。

组件（Component）是 Vue.js 最强大的功能之一。

### 1.根组件

在` let vm = new Vue({}) `我们得到的vm是一个Vue的一个对象（或者叫实例），它也可以理解为一个组件，只不过它是一切其它的组件的根组件。

以示区别，我们后面都说是定义“子组件”

### 2. 子组件

有两种方法，使用组件：

- 全局组件
- 局部组件

#### 1. 全局组件

通过Vue的静态方法`component()`定义一个全局组件

**template:就是组件要显示出来的html标签的内容，它就会去替换你定义的html标签**

**全局组件：所有的vue实例都可以使用**

```vue
<div id="app">
    <!--这里使用注册名进行引用子组件-->
	<my-header></my-header>     
	<!-- 注意，如果组件名含有大写字母，需要在大写字母前加-并换为小写字母，大写的首字母只需改为小写即可 -->
</div>

<!-- 引入vue.js文件 -->
<script src="../js/vue.js"></script>
<script>
   <!--全局组件-->
Vue.component("myHeader",{      //myHeader为组件名，可随意更改
    template:"<div>这是全局组件<div>"   //固定格式，引号内部可改
})
    
let app=new Vue({
    el:"#app",
    data:{},
    methods:{},
    conputed:{},
    filters:{},
})
</script>
```



#### 2. 局部组件

局部组件只能在注册的那个实例当中使用，不能在其它的实例中用。

##### 1. 简单局部

```vue
    <div id="app">
        <!--这里使用注册名进行引用子组件-->
        <myhead></myhead>		
        <son></son>
    </div>

<!-- 引入vue.js文件 -->
<script src="../js/vue.js"></script>
<script>
    // 可以在这里定义组件
    let myhead={
        template:"<div>这是一个根组件</div>"
    }
    let app = new Vue({
        el: "#app",
        data: {},
        components: {
            myhead:myhead,  //这里是注册组件,如果注册名与定义名一致的话可以直接写为 myhead, 这种简写格式
            "son": {        //这里是定义与注册一起完成，省略了定义名
                template:"<div>这是另一个根组件</div>"
            }
        }
    })
  </script>  
```

##### 2. 稍复杂局部组件

```vue
<div id="app">
        <son></son>
    </div>
    <!-- 当子组件过于复杂时，可将子组件写在外部，然后通过id名引用  -->
    <template id="son">
        <div>
            <button></button>
        </div>
    </template>
</body>
<script src="../js/vue.js"></script>
<script>
    let son = {
        template: "#son",
        data() {            //子组件这里data是函数格式，且必须返回一个对象
            return {
                //子组件中的数据项写这里，固定写法
            }
        }
    }
    let app = new Vue({
        el: "#app",
        data: {},
        methods: {},
        components: {
            son     //注册子组件，这里因为注册名与定义名一致，所以简写了
        }
    })
</script>
```



#### 3. 一些细节

##### 1. 定义名

一般采用大驼峰命令法：如果有多个单词，则每个单词的首字母都要大小写。

例如：		MyHeader	EleUser

##### 2. 注册名

这里名字没有要求。一般与组件名保持一致：利用es6中的对象的属性名的简写规则，进行简写：

例如：		MyHeader : MyHeader,		可简写为	MyHead，

##### 3. 使用名

**在使用组件时，它的名字只能是全小写！！**

如果说你的组件的前两个名字：

1. 定义名字
2. 注册名字

都采用大驼峰命名法，则，使用组件你必须用kebab命名法：即大写字母改小写，在前面加中划线“-”



**总结**：建议写法

| 定义名       | 注册名                        | 使用名      |
| ------------ | ----------------------------- | ----------- |
| 大驼峰命名法 | 与定义名保持一致。用es6的简写 | kebab命名法 |



##### 4. 子组件格式

```vue
<script>
let son = {				//son为组件名，自命名
	template:"",	
	data:function(){		//这里可简写为	data(){return{	//数据	}}
		return {
		//你的数据项
		}
	}
}
</script>
```





## 六、组件交互



### 1. 父传子

1. 在父组件中，正常定义自己的数据

2. 在父组件的模板中通过属性绑定把数据绑定到子组件上。

3. 在子组件中定义props属性。用来接收父组件传递的数据。

4. 在props中定义的属性，是专门用来从父组件中去获取数据的，定义好了，就可以像定义在data中的数据项一样，去正常的使用。



**Props**详细的格式：每一个数据项，都可以加三个属性设置来修饰它。

Props:{

   数据项名字:{

​      **type**:类型。指明从父组件中传递过来的数据必须是什么类型。它的取值是:Object,Array,String,Number,Boolean 都是构造器。不要写成字符串。

​      **default**://默认值。当父组件没有传数据时，就用这个值。

​      **required** : true/false 。是否必须一定要传递过来。

}

}

```vue
<div id="app">
    <!-- 在父中通过属性绑定把数据绑定到子组件上 起一个桥梁的作用-->
    <son :sonvalue="num"></son>
</div>
<template id="son"> 
	<div>
		<!-- 在子元素中使用数据-->
		<h1>{{sonvalue}}</h1>
		<h1>{{sonnum}}</h1>       
	</div>
</template>

<script src="../js/vue.js"></script>
<script>
    let son = {
        template: "#son",
        data(){
            return{
               sonnum: this.sonvalue,       //将接收过来的数据转换为自己的
               sonnum:1                     //之后就可以任意修改了
            }
        },
        props:{
            sonvalue:{              //以下三个值可根据需要省略（但最少写一项）
                type:Number,        //用来指定数据类型
                dafault:25,         //设置默认值
                require:true        //设置必须要有传输数据
            }
        }
    }
    
    let app = new Vue({
        el: "#app",
        data: {
            num:10          //父组件中的数据
        },
        components: {
            son
        }
    })
</script>
```

**注意：不能在子组件中直接修改父组件传递的数据**

解决方法：在子组件中，接收父组件传递的数据之后，你可以用这个数据对自已子组件中定义的数据项去做初始化，然后，你就可以去自己自己在子组件中定义的数据。



### 2. 子传父

 

在父组件中的子组件上添加事件监听

格式： **<子组件 @子组件发回事件名="父组件要执行的方法"></子组件>**

​	注意：“子组件发回事件名”	要小写

并在父组件中定义  "父组件要执行的方法"

在子组件中,某一个时间通过this.$emit发出这个事件

```vue
<body>
    <div id="app">
        <h1>父组件</h1>
        <!-- 给子组件添加一个事件监听 -->
        <!-- submitmsg是一个事件 -->
        <!-- 子组件会发射一个事件，事件发生时会触发addmsg函数 -->
        <!-- addmsg是父组件的函数 -->
        <son @submitmsg="addmsg"></son>
        <h4>{{a}}</h4>
    </div>
    
    <template id="son">
        <div>
            <h3>子组件</h3>
            <button @click="fashe">发射</button>
        </div>
    </template>
</body>
<script src="./vue.js"></script>
<script>
    let Son = {         //子组件
        template: "#son",
        data() {return {}},
        methods: {
            fashe() {
                this.$emit("submitmsg",66666)
            }
        }
    }
    let vm = new Vue({      //父组件
        el: "#app",
        data: {
            a:0
        },
        components: {
            Son
        },
        methods: {
            addmsg(info) {	//info就是子组件传递的数据的载体
                // console.log(info)	info == 666
                this.a = info
            }
        }
    })
</script>
```



### 3.父传孙

**父传子传孙（父传子），只能一级一级的传，不能跨级传**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script src="./vue.js"></script>
</head>

<body>
    <div id="app">
    <!--绑定事件，传输数据，与父传子一样，
	其中，因为a,b是直接传输的字面量，所以不需要绑定data中的数据 -->
        <my-button :msg="content" a="1" b="2"> </my-button>
    </div>
</body>
<script>
    //父组件
    let vm = new Vue({
        el: "#app",
        data:{
            content:"点我"
        },
        //儿子组件 
        components: {
            "MyButton":{
                //mounted
                mounted(){
                    // this.$attrs 如果子接收的数据没有用到，把数据保存在$attrs中
                    console.log(this.$attrs)
                },
               //同样的，绑定事件，传输数据 
                template:`<div>子组件<my  v-bind="$attrs"></my></div>`,
                //   孙级组件
                components:{
                    'my':{
                        props: ['a','b'],
                        template:`<span>孙组件{{a}}----{{b}}</span>`
                    }
                }
            }
        }
    })
</script>
</html>
```

结果：

```txt
子组件孙组件1----2
```



### 4. 子触发父中的方法

**例如click点击事件change**

#### 1. 方式一

`@click.native` 

1，给 vue 组件绑定事件时候，必须加上native ，否则会认为监听的是来自Item组件自定义的事件

2，等同于在子组件中：  子组件内部处理click事件然后向外发送click事件：`$emit("click".fn)`

即是把 click 当成一个正儿八经的点击事件，让子组件来触发这个事件.   在调用子组件时将`@click="change"` 改为`@click.native="change" `

示例：`<my-button @click.native="change" a="1"></my-button>`

**此种方法有个缺点，就是在点击son组件那一行都会触发点击事件**

```html
<div id="app">
    <h1>这是父组件</h1>
    <son @click.native="change"> </son>
</div>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            content: "点点"
        },
        methods: {
            change() {
                alert(1)
            }
        },
        components: {
            son: {
                template: `<h2><button>这是子组件</button></h2>`,
            }
        }
    })
</script>
```



#### 2. 方式二

`@click="$listeners.click()"`

所有v-on绑定的事件都会放到`$listeners`中

可在定义子组件时，设置

```vue
template:`<div><button @click="$listeners.click()">点我</button></div>`
```

且因为所有v-on绑定的事件都会放到`$listeners`中，所以这样写的话：

```vue
//使用子组件
<my-button @click="change" @mouseup="change"></my-button>
//设置子组件
template:`<div><button v-on="$listeners">点我</button></div>`
//结果	触发两次change事件
```

示例：

```html
<div id="app">
    <h1>这是父组件</h1>
    <son @click="change"> </son>
</div>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            content: "点点"
        },
        methods: {
            change() {
                alert(1)
            }
        },
        components: {
            son: {
                template: `<h2><button @click="$listeners.click()">这是子组件</button></h2>`,
            }
        }
    })
</script>
```



#### 3. 方式三 

`@click="$emit('click')"`

即是使用`$emit`来触发父中的事件

示例：

```html
    <div id="app">
        <h1>这是父组件</h1>
        <son @click="change"> </son>
    </div>
    <script>
        let vm = new Vue({
            el: "#app",
            data: {
                content: "点点"
            },
            methods: {
                change() {
                    alert(1)
                }
            },
            components: {
                son: {
                    template: `<h2><button @click="$emit('click')">这是子组件</button></h2>`,
                }
            }
        })
    </script>
```

#### 4. 同时触发两次事件

所有使用`v-on`绑定的事件都会被放到`$listeners` 中

```html
    <div id="app">
        <h1>这是父组件</h1>
        <son @click="change" @mouseup="change"> </son>
    </div>
    <script>
        let vm = new Vue({
            el: "#app",
            data: {
                content: "点点"
            },
            methods: {
                change() {
                    alert(1)
                }
            },
            components: {
                son: {
                    template: `<h2>
                        <button @click="$listeners.click()">这是子组件1</button>	//触发一次
                        <button @click="$emit('click')">这是子组件2</button>	//触发一次
                        <button v-on="$listeners">这是子组件3</button>	//触发两次
                        </h2>`,
                }
            }
        })
    </script>
```





### 5.`provide`  `inject`

1.`provide`就相当于加强版父组件prop，`provide` 选项应该是一个对象或返回一个对象的函数。该对象包含可注入其子孙的属性。

2.`inject`就相当于加强版子组件的props 

以上两者可以在父组件与子组件、孙子组件、曾孙子...组件数据交互，也就是说不仅限于prop的父子组件数据交互，只要在上一层级的声明的provide，那么下一层级无论多深都能够通过inject来访问到provide的数据

其中，`provide`在父组件中声明时与`data`同级,格式为`provide:{键：值}`,

后代组件在接收时其`inject`也是与`data`同级，格式为`inject:[键]` 



`provide` 和 `inject` 主要为高阶插件/组件库提供用例。并不推荐直接用于应用程序代码中。

这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。如果你熟悉 React，这与 React 的上下文特性很相似。





## 七、路由守卫

**创建=>挂载=>更新=>销毁**

### 1. 参考图



![lifecycle](https://cn.vuejs.org/images/lifecycle.png)

### 2. 示例

```html
<!doctype html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>生命周期</title>
    <script src="./vue.js"></script>
</head>

<body>
    <div id="app">
        {{ flag }}
    </div>
</body>
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            flag: "生命周期"
        },
        //用于初始化生命周期，并且绑定自己的事件
        beforeCreate() {
            console.log("创建前：");	
            console.log(this.$el);	  	//undefined
            console.log(this.$data);	//undefined
        },
        //发起ajax，ajax是异步，不会中断，此时可以获取数据data中的数据
        created() {
            console.log("创建完成：");
            console.log(this.$el);		//undefined
            console.log(this.$data);	//{__ob__: Observer}
        },
        //第一次调用渲染函数之前 也可以发送ajax请求  一般不会在这里发
        beforeMount() {
            console.log("挂载前：");
            console.log(this.$el);		//<div id="app"> {{ flag }}</div>
            console.log(this.$data);	//{__ob__: Observer}
        },
        //用的比较多  发起ajax  当页面渲染完成后  获取真实dom
        mounted() {
            console.log("挂载完成：");
            console.log(this.$el);		//<div id="app">生命周期</div>
            console.log(this.$data);	//{__ob__: Observer}
        },
        //基本也不使用
        beforeUpdate() {
            console.log('更新渲染前');
            let flag = this.$refs.app.innerHTML;
            console.log('flag:' + flag);
        },
        //会使到，但不能用updata钩子操作数据  可能导致死循环
        updated() {
            console.log('更新完成');
            let flag = this.$refs.app.innerHTML;
            console.log('flag:' + flag);
        },
        //可在销毁前做一些清理工作
        beforeDestory(){
            console.log("销毁前：");
        },
        //销毁了
        destoryed(){
            console.log("销毁完成：");
        }
    });
</script>
</html>
```

### 3. 解释

`beforeCreate()`:	实例在内存中被创建出来，但是还没有初始化好data和methods属性。

`create()`:	实例已经在内存中创建，已经初始化好data和method，此时还没有开始编译模板。

`beforeMount()`:	已经完成了模板的编译，还没有挂载到页面中。

`mounted()`：	将编译好的模板挂载到页面指定的容器中显示。

`beforeUpdate()`:	状态更新之前执行函数，此时data中的状态值是最新的，但是界面上显示的数据还是旧的，因为还没有开始重新渲染DOM节点。

`updated()`:	此时data中的状态值和界面上显示的数据都已经完成了跟新，界面已经被重新渲染好了！

`beforeDestroy()`:	实例被销毁之前。

`destroyed()`:	实例销毁后调用，Vue实例指示的所有东西都会解绑，所有的事件监听器都会被移除,所有的子实例也都会被销毁。组件已经被完全销毁，此时组建中所有data、methods、以及过滤器，指令等，都已经不可用了。

### 4. 应用

这些都是官方说明，在实际开发项目中这些钩子函数如何使用呢？

`beforecreate` : 可以在这函数中初始化加载动画

`created` ：做一些数据初始化，实现函数自执行

`mounted`： 调用后台接口进行网络请求，拿回数据，配合路由钩子做一些事情

`destoryed` ：当前组件已被删除，清空相关内容





## 八、 导航守卫

`vue-router` 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。有多种机会植入路由导航过程中，他们分为三种：

- 全局路由守卫：在路由实例对象注册使用；
  - `router.beforeEach`
  - `router.beforeResolve`
  - `router.afterEach`
- 路由独享守卫：在路由配置项中项定义；
  - `beforeEnter`
- 组件内的路由守卫：在组件属性中定义；
  - `beforeRouteEnter`
  - `beforeRouteUpdate`
  - `beforeRouteLeave`

![图片加载失败!](https://www.github.com/github-fanjunyang/JueJin/raw/master/images/%E5%AF%BC%E8%88%AA%E5%AE%88%E5%8D%AB_1.png)

### 1. `beforeRouteLeave`

导航离开该组件的对应路由时调用，我们用它来禁止用户离开，比如还未保存草稿，或者在用户离开前，将`setInterval`销毁，防止离开之后，定时器还在调用。

### 2.  `beforeEach` 

`beforEach`其实是`vue-router`的钩子函数，可以理解为每个router跳转之前都会调用的一个方法

`beforEach`的三个参数：

1. to： router即将进入的路由对象。
2. from：当前导航正要离开的路由。
3. next：一个function，一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。

- `next()`: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed （确认的）。
- `next(false)`: 中断当前的导航。如果浏览器的 URL 改变了（可能是用户手动或者浏览器后退按钮），那么 URL 地址会重置到 from 路由对应的地址。
- **next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。** 你可以向 next 传递任意位置对象，且允许设置诸如 replace: true、name: ‘home’ 之类的选项以及任何用在 `router-link` 的 to prop 或 `router.push` 中的选项，注意，next可以通过query传递参数。
- `next(error)`: (2.4.0+) 如果传入 next 的参数是一个 Error 实例，则导航会被终止且该错误会被传递给 `router.onError() `注册过的回调。

```js
router.beforeEach((to, from, next) => {
    //根据字段判断是否路由过滤
    if (to.matched.some(record => record.meta.auth)) {
        if (getToken() !== null) {
            next()
        } else {
            //防止无限循环
            if (to.name === 'login') {
                next();
                return
            }
            next({
                path: '/login',
            });
        }
    } else {
        next()//若点击的是不需要验证的页面,则进行正常的路由跳转
    }
});
```

### 3. `beforeRouteUpdate`

使用场景：

　　组件复用；路由跳转；

```js
beforeRouteUpdate (to, from, next) {
    //在当前路由改变，但是该组件被复用时调用
    //对于一个带有动态参数的路径 例如/good/:id，在 /good/1 和 /good/2 之间跳转的时候，
    // 由于会渲染同样的good组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
}
```

### 4. `beforeEnter`

可以在路由配置上直接定义 `beforeEnter` 守卫：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```



### 5.  `beforeRouteEnter`

`beforeRouteEnter`路由钩子触发时机就是第一次进入`home.vue`页面的时候，但是在这个页面中点击其他菜单不会触发，我们也只需要在第一次进入的时候对菜单进行初始化。

我们可以利用`beforeRouteEnter`路由钩子生成权限菜单，并存储数据到vuex中

我们需要在`Home.vue`中利用`beforeRouteEnter`路由钩子生成权限菜单，并存储权限数据到vuex中，首先在`Login.vue`页面将登录返回的权限数据存储到本地`localStorage`或者`sessionStorage`，然后 **在Home.vue中引入路由数据**，



### 6. `beforeResolve`

全局解析守卫，和`beforeEach`类似，区别是在**导航被确认之前**，同时在所有**组件内守卫和异步路由组件被解析之后**，解析守卫就被调用
即在 `beforeEach` 和 组件内`beforeRouteEnter` 之后

参数和`beforeEach`一致，也需要调用next对导航确认



### 7. `afterEach`

全局后置钩子
在所有路由跳转结束的时候调用
这些钩子不会接受 next 函数也不会改变导航本身



### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用离开守卫`beforeRouteLeave`。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 用创建好的实例调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数。



每个守卫方法接收三个参数：

- **to: Route**: 即将要进入的目标 路由对象
- **from: Route**: 当前导航正要离开的路由
- **next: Function**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。
  - **next()**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** (确认的)。
  - **next(false)**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
  - **next('/') 或者 next({ path: '/' })**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在`router-link` 的 `to` prop或 `router.push` 中的选项。
  - **next(error)**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 `router.onError()`注册过的回调。

**确保要调用 next 方法，否则钩子就不会被 resolved。**

注意：

1. `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫。对于 `beforeRouteUpdate` 和 `beforeRouteLeave` 来说，`this` 已经可用了，所以**不支持**传递回调，因为没有必要了。

   






## 九、vue中的属性和方法

**`let vm = new Vue({})`**



### 1.  `vm.$data`

​	 Vue实例观察的数据对象，Vue实例代理了对其data对象属性的访问

### 2.  ` vm.$el`

 	Vue实例使用的根DOM元素

### 3.  `vm.$options`

​	用于当前Vue实例的初始化选项，需要在选项中包含自定义属性时会有处理

​	`console.log(vm.$options)`       打印出vm上面的所有的挂载项 

### 4. ` vm.$parent`

​	父实例，如果当前实例有的话。

### 5.  `vm.$root`

​	当前组件树的根Vue实例，如果当前实例没有父实例的话，此实例会是自己

### 6.  `vm.$children`

1. 当前实例的直接子组件
2. 注意是子组件，还是直接的
3. 不能保证顺序，也不是响应式的
4. 如果发现自己尝试使用$children 来进行数据绑定，考虑使用一个数组配合v-fork来生成子组件，并且使用Array作为真正的来源

### 7. ` vm.$slot`

1. 用来访问被slot分发的内容，每个具名slot都具有相应的属性
2.  default属性包括了所有没有被包含在slot中的节点

### 8. ` vm.$scopedSlots`

​	用来访问scoped slots

### 9. `vm.$refs`	和	`ref`

**$refs**

就是一个对象，表示持有注册过 `ref` 特性的所有 DOM 元素和组件实例。

**ref**

被用来给元素或子组件注册引用信息，引用信息将会注册在父组件的$refs对象上。如果在普通的DOM元素上使用，那么指向的就是普通的DOM元素。

`ref`不能给多个元素设置相同的ref，只能识别一个。

当 `v-for` 用于元素或组件的时候，引用信息将是包含 DOM 节点或组件实例的数组。

如果给组件设置ref，那么就可以通过`this.$refs`获取组件的实例，进而调用组件实例上的方法。

注意：关于 ref 注册时间的重要说明：因为 ref 本身是作为渲染结果被创建的，在初始渲染的时候你不能访问它们 - 它们还不存在！`$refs` 也不是响应式的，因此你不应该试图用它在模板中做数据绑定。

```vue
<body>
    <div id="app">
        <template v-for="i in 3">
            <div ref="my">我的dom元素</div>
        </template>
        <my-component ref="com"></my-component>
    </div>
</body>
<script>
    let vm = new Vue({
        el: "#app",
        mounted () {
          console.log(this.$refs.my);
          console.log(this.$refs.com.show());  
        },
        components: {
            "myComponent":{
                template:`<div>my-component</div>`
            }
        }
    })
</script>

结果：
我的dom元素
我的dom元素
我的dom元素
my-component
```



### 10. ` vm.$isServer`

​	当前实例是否运行于服务器

### 11.`  vm.$props`

1. 当前组件接收到的props对象

2. vue实例代理了对其props对象属性的访问

### 12. `vm.$attrs`

1. 包含了父作用域中不作为prop被识别的特性绑定，除了class和style

2. 当一个组件没有声明任何prop时，这里会包含所有父作用域的绑定，并且可以通过`v-bind="$attrs`传入内部组件--在创建高级别的组件时非常有用

   ```js
   <script>
       //父组件
       let vm = new Vue({
           el: "#app",
           data: {
               content: "点我"
           },
           //儿子组件 
           components: {
               "MyButton": {
                   // mounted(){
                   //     // this.$attrs 如果子接收的数据没有用到，把数据保存在$attrs中
                   //     console.log(this.$attrs)
                   // },
                   template: `<div>子组件 <my  v-bind="$attrs"> </my> </div>`,
                   //   孙级组件
                   components: {
                       'my': {
                           props: ['a', 'b'],
                           template: `<span>孙组件a{{a}}----b{{b}}</span>`
                       }
                   }
               }
           }
       })
   </script>
   
   结果为： 	子组件 孙组件a1----b2
   ```

   

### 13. `vm.$listeners`

​	包含了父作用域中的时间监听器，可以通过`v-on="$listeners"`传入内部组件--在创建更高层次的组件时非常·	有用



### 14. 添加实例属性

1. 在很多组件里用到的数据和实例，但是你并不想污染全局作用域，这个时候就可以通过在原型上定义他们使得在每个Vue实例中都是可用的 
2. `Vue.propotype.$appName='liba'`
3.  这样appName就可以在所有Vue实例中使用了，甚至在实例被创建之前就可以使用
4. 为实例属性设置作用域的重要性

### 15. `vm.$watch`

语法： 	**`vm.$watch( data,callback,[option] ):`**			用来监视数据的变化

当数据发生变化的时候，执行`callback`函数，这里的`callback`函数可以接受两个参数`newValue`和`oldValue`作为改变后（前）数据的值，还有`option`选项，其实就是一个对象`{deep:true(false)}`，用来决定是否深度监视（当监视数据对象的时候用到）。



### 16. `vm.$nextTick`

1. 在Vue生命周期的`created()`钩子函数进行的DOM操作一定要放在`Vue.nextTick()`的回调函数中

2. `$nextTick` 是在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后使用 `$nextTick`，则可以在回调中获取更新后的 DOM



### 17.`vm.$emit`

触发当前实例上的事件。附加参数都会传给监听器回调。

1、父组件可以使用 props 把数据传给子组件。
2、子组件可以使用 $emit 触发父组件的自定义事件。

`vm.$emit( event, arg ) `//触发当前实例上的事件

`vm.$on( event, fn );`//监听event事件后运行 fn； 



```vue
<div id="emit-example-simple">
  <welcome-button v-on:welcome="sayHi"></welcome-button>
</div>

<script>
Vue.component('welcome-button', {
  template: `
    <button v-on:click="$emit('welcome')">
      Click me to be welcomed
    </button>
  `
})
    
new Vue({
  el: '#emit-example-simple',
  methods: {
    sayHi: function () {
      alert('Hi!')
    }
  }
})
</script>
```





## 十、》》



### 1、侦听器 `watch`

watch的作用可以监控一个值的变换，并调用因为变化需要执行的方法。可以通过watch动态改变关联的状态。

```vue
<script>
    let vm = new Vue({
        el: "#app",
        data: {
            msg: "adadsad"
        },
        watch: {
            //此函数可以接收两个参数，分别为新值和旧值
            msg: function (newvalue, oldvalue) {
                console.log(newvalue, oldvalue)
            }
        }
    })
    vm.msg = "2222"
    //最后控制台输出 2222  adadsad
</script>
```

注意，**不应该使用箭头函数来定义 watcher 函数** (例如 `searchQuery: newValue => this.updateAutocomplete(newValue)`)。理由是箭头函数绑定了父级作用域的上下文，所以 `this` 将不会按照期望指向 Vue 实例，`this.updateAutocomplete` 将是 undefined。

**注意**：watch可以支持异步，其它情况和computed都可以互换，只是watch写起来更复杂一点，watch 可以实现一些简单的功能，能使用computed就不要使用watch。



### 2、拦截器

在请求或响应被 `then` 或 `catch` 处理前拦截它们。

```js
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



### 3、数据劫持

```js
let obj = {
    name:'z3',
    age:{
        age:55
    }
}
function observer(obj){
    if(typeof obj == "object"){
        for (let key in obj){
            defineReactive(obj,key,obj[key]);
        }
    }
}
function defineReactive(obj,key,value){
    Object.defineProperty(obj,key,{
        get(){
            return value
        },
        set(val){
            console.log("数据更新")
            value = val;
        }
    })
}
observer(obj);
obj.name = "wc"
console.log(obj.name)
```





## 十一、通过`vue-cli`创建的项目设置配置文件

。。。。





































