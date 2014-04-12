# 函数

“JavaScript设计得最出色的就是它的函数的实现。它几乎接近于完美。”

## 函数也是对象

函数实际上也是对象，区别在于它是可调用的，这调用表示“使用给定的代码块执行指定的动作”。任何代表函数的表示后跟随一堆括号 - `()`都表示函数调用。

每个函数都是Function类型的实例，因而它也有属性和方法。比如：`arguments`属性，`call()`方法等等。

## 创建函数

创建函数的方式有多种，最常见的莫过于函数声明和函数表达式。

```javascript
// 函数声明
fnName(); // 函数声明会提前
function fnName(arg1, arg2) {
	// 函数体
}
// 函数表达式
function(){}; // 也叫“匿名函数表达式”
var fn = function() {};
var fn = someFunction;

// 借用new关键字
// 反模式
var fn = new Function('a', 'b', 'return a + b');
```

## 使用函数

函数可以作为方法，也可以作为参数：

```javascript
var user = {};
user.sayHi = function(){};
// 调用
user.sayHi();

// 函数作为参数
setTimeout(someFn, 100);

// 作为返回结果
function A() {
	return function() {
		// ...
	}
}
```

## 构造函数

函数可以用来生成对象，这种函数叫做构造函数，任何函数都可以作为构造函数。构造函数用于定义示例对象的初始化信息。

```javascript
function Person(name, age) {
	this.name = name;
	this.age = age;
}
// 使用new关键字创建实例
var basecss = new Person('basecss', 23);
basecss.name; // 'basecss'
basecss.age; // 23
```

## 函数原型

每个函数都有原型，原型中定义了函数实例共享的属性和方法，可以在原型中添加，重写属性和方法。

```javascript
function myFunction() {};
myFunction.call(/*xxxx*/)
Function.prototype.hiGirl = '妹子，你好。';
var myFn = function() {};
my.hiGirl;
```

## 变形金刚

```javascript
// 立即调用函数表达式
(function(){})();
	
(function(){}());

(function(){
	return function(){};
})()();
// 对象方法
myObj['myFunc']();
```
参考：[[译]立即调用函数表达式](http://www.basecss.net/article/immediately-invoked-function-expression.html)

## 函数的返回值。

每个函数都有返回值，也可以指定函数的返回值。使用`return`语句可以指定函数的返回值，在没有指定函数返回值的情况下默认返回`undefined`。

```javascript
function plus(a, b) {
	return a + b; // 显示的指定返回值
}

function log(msg) {
	console.log(msg); // 没有return语句的函数返回undefined
}
```

## 函数的调用方式

有4中调用函数的方式。

普通的函数，在函数名后跟一对括号即可调用函数：

```javascript
function myFn() {
	console.log('普通调用');
}
myFn(); // 普通函数调用
```

当函数作为对象的属性时，被称作对象的方法。作为方法调用时，必须带指定的对象作为前缀。

```javascript
var basecss = {
	name: 'basecss',
	age: 23,
	sayHi: function() {
		console.log('姑娘，你好！');
	}
};
basecss.sayHi(); // 点号形式
basecss['sayHi'](); // 中括号形式
```

还可以使用`call()`和`apply()`方法在特定的作用域中调用函数。这两个方法并非继承的，是Function类型特有的方法。使用这两个方法是可以设定调用函数的作用域，也就是被调用函数体内的`this`对象。

使用`call()`和`apply()`调用函数的形式类似，不同的是给被调用的函数传递参数的方式不同。这两个的第二个参数用于给被调用函数传递参数，`apply()`在传递参数时必须使用数组，而`call()`则是将除第一个参数之外的参数直接传递给被调用的函数。

```javascript
function plus(a, b) {
	return a + b;
}
function useCall(a, b) {
	return plus.call(this, a, b);
}
useCall(1, 2); // 3
function useApply(a, b) {
	return plus.apply(this, [a, b]);
}
useApply(1, 1); // 2
```

当函数作为构造函数时，被用来实例化对象。通常使用`new`关键字调用构造函数。

```javascript
// 自定义构造函数
// 通常首字母大写
function Person() {}
var userA = new Person();

// 实际上使用new关键字创建对象或者数组是也是调用构造函数
// 只是这些构造函数是JavaScript内置的
var o = new Object();
var arr = new Array('1', '2');
```

## 函数的参数

每个函数都可以有参数。定义函数时的参数叫做“形参”，调用函数时给定的参数叫做“实参”。

“形参”的个数可以和“实参”的个数不匹配。当实参个数多余形参个数时，只会用等于形参个数数量的参数，多余的自动忽略；当形参个数多余实参个数时，不够的实参都为`undefined`（通常用于防止`undefined`被覆盖）。

```javascript
function fn(a, b, c) {
	console.log(a);
	console.log(b);
	console.log(c);
}
// 实参不够，多的形参为undefined
fn(1,2);
// 实参多于形参，多的实参被忽略
fn(1,2,3,4);
```

## 函数作用域

作用域用于控制变量的可见性和生命周期。JavaScript中的作用域叫“函数作用域”。

```javascript
var a = 1;
function fn() {
	var a = 2;
	console.log(a);
}
fn(); // 2

function fn2() {
	console.log(a);
	var a = 3;
}
fn(a); // undefined
// 注意，变量声明也会提升，因此第二个函数打印undefined
// 相当于
function fn2() {
	var a; // undefined
	console.log(a);
	a = 3;
}
```