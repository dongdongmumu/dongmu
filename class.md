# class

## 一，使用class创建对象

```js
class 类名{
	constructor(参数){
		this.属性= 参数;
	}
	method(){
        //对象中简写方法，省略了function。不要与箭头函数搞混了。
	}

}
```

**注意：**

-  class是关键字，后面紧跟类名，类名首字母大写，采取的是大驼峰命名法则。类名之后是{}。
-  在{}中，不能直接写语句，只能写方法，方法不需要使用关键字
-  方法和方法之间没有逗号。不是键值对。



## 二，使用extends实现继承

**格式：**

```js
class 子类 extends 父类{
	constructor(参数){
  		super(参数)
		this.属性 = 值
	}
}
```

注意：

-  使用extends关键字来实现继承
-  在子类中的构造器constructor中，必须要显式调用父类的super方法，如果不调用，则this不可用

## 三，类的静态方法 static

直接通过类名来访问的方法就是静态方法。类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”.

父类的静态方法，可以被子类继承。

如：Math.abs();这里的abs()是静态方法。

Array.isArray();isArray()是静态方法.

在方法名前加static 就可以了。

<script>
class Foo {
  static classMethod (){
    console.log('static fun')
  }
  //定义在原型上的方法
  commonMethod (){
    console.log('commonMethod')
  }
}
var foo = new Foo();
Foo.classMethod();  //类可以调用
Foo.commonMethod(); //类不可以调用
foo.commonMethod(); //实例可以调用
foo.classMethod();  //实例不可以调用
</script>

## 四，类的静态属性 static

直接通过类名来访问的属性就是静态属性。如：Math.PI;这里的PI是静态属性。

实现的方式在属性名前加static 就可以了。

## 五，类的修饰器

装饰器可以修饰类 类的属性 类的原型上的方法

修饰的时候 就是把这个类 属性… 传递给修饰的函数

如：@connect @withRoute