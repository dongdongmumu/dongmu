**"use strict":**

　　1、它是 ES5 引入的一条**指令**，指令不是语句，但非常接近于语句

　　2、不包含任何语言的关键字，指令仅仅是一个包含一个特殊**字符串直接量**的表达式。对于那些没有实现 ES5 的 JavaScript 解释器来说，它只是一条没有副作用的表达式语句

　　3、只能出现在脚本代码的开始或函数体的开始、任何实体语句之前。但它不必一定出现在脚本的首行或函数体的首行

　　4、使用该指令的目的是说明后续代码将解析为**严格代码**，根据指令所处作用域来确定严格代码的范围

　　5、ES5 中的严格模式是该语言的一个**受限制的子集**，它修正了语言的重要缺陷，并提供**健壮的查错功能**和**增强安全机制**

 

**严格模式与非严格模式的区别：**

　　1、禁止使用 with 语句

```js
// 下面代码将 obj 添加到作用域链的头部，然后执行 statement
with (obj)
    statement;
```

 

　　2、严格模式下，所有变量都要先声明，如果给一个未声明的变量、函数、函数参数、catch 从句参数或全局对象赋值，将会抛出一个引用错误异常



```js
 "use strict";

tempOne = 3;   // 在严格模式下 tempOne 未声明，不能直接赋值，会报错
tempTwo = 3;    // 下面语句已声明，可正常通过编译
var tempTwo;
tempThree= 3；    // 报错，注意 let 声明变量的作用域
let tempThree;
```



 

　　3、调用的函数（不是方法）中的一个 this 值是 undefined，非严格模式下该 this 值总是全局对象



```js
// 可通过以下代码判断 js 是否支持严格模式
let hasStrictMode = (function () {
   "use strict";    // 若去掉，则会返回 false
   return this === undefined;
}());

console.log(hasStrictMode);    // 输出 true
```



*注：函数是一段代码，需要通过名字来调用，而方法是特殊的函数，需要通过对象来调用*

 

　　4、当通过 call() 或 apply() 来调用函数时，其中的 this 值就是通过 call() 或 apply() 传入的第一个参数；在非严格模式中，null 和 undefined 值被全局对象和转换为对象的非对象值所代替



```js
function add(a, b){    return a+b;}
function sub(a, b){    return a-b;}
// B.call(A, args1, args2); 即A对象调用B对象的方法
let a1 = add.call(sub, 4, 2);
// 注意 2 者的参数，arguments 是一个数组
// B.apply(A, arguments); 即A对象应用B对象的方法
let a2 = sub.apply(add, [4, 2]);

console.log(a1);  // 输出6
console.log(a2);  // 输出2
```



 

　　5、给只读属性赋值和给不可扩展的对象创建新成员都将抛出一个类型错误异常（非严格模式下不会报错）

```js
"use strict";

// 类型错误
Object.prototype = "Hello";
```

 

　　6、传入 eval() 的代码不能在调用程序所在的上下文中声明变量或定义函数，变量和函数的定义在 eval() 创建的新作用域中，这个作用域在 eval() 返回时就弃用了



```js
"use strict";

let num = 2;
console.log(eval("var num = 5; num")); // 输出 5
console.log(num); // 输出 2
```



　　7、函数里的 arguments 对象拥有传入函数值的**静态副本**



```js
"use strict";

function one(num) {
    num = 2;
    // 严格模式下 arguments 保存静态副本，非严格模式会动态改变
    console.log(num, arguments[0]);
}

one(1);    // 输出 2 1；
```



 

　　8、当 delete 运算符后面跟随非法标识符时，会抛出一个语法错误异常；非严格模式下仅仅返回 false

```js
"use strict";

// 具有非法标识符，抛出语法错误
delete p-temp;
```

 

　　9、试图删除一个不可配置的属性将抛出一个类型错误异常；非严格模式下仅仅返回 false

```js
"use strict";

// 删除一个不可配置属性，报错
delete Object.prototype;
```

 

　　10、在一个对象直接量中定义两个或多个同名属性将产生一个语法错误

```js
"use strict";

// 语法错误
let obj = {"test": 1, "test": 2};
```

 

　　11、函数声明中存在两个或多个同名参数将会产生一个语法错误

```js
"use strict";

// 语法错误
function f(a, a, b) {
    console.log(a);
}
```



 

　　12、不允许使用八进制整数直接量（以 0 为前缀而不是 0x）

```js
"use strict";

let oct = 0100; // 语法错误
```

 

　　13、标识符 eval 和 arguments 当做**关键字**，它们的值是**不能更改**的

```js
"use strict";

// 语法错误
let eval = 5;
```

 

　　14、限制了对调用栈的检测能力；在严格模式的函数中，**arguments.caller** 和 **arguments.callee** 都会抛出一个类型错误异常；严格模式的函数同样具有 **caller** 和 **arguments** 属性，当访问这两个属性时将抛出类型错误异常



```js
"use strict";

// 报错
var f = function() { return arguments.callee; };
f();
```