# 函数
“JavaScript设计得最出色的就是它的函数的实现。它几乎接近于完美。”

## 函数也是对象

函数也是对象，区别在于它可以被调用。任何代表函数的表达式后加上括号`()`表示调用函数。

### 七十二变满地跑

#### 函数表达式

	function(){};
	
也叫“匿名函数表达式”、“匿名函数”。

#### 赋值

	var myFunc = function(){};
	var myFunc2 = myFunc;
	
也可以作为方法：

	var myObj = {};
	myObj.myFunc = myFunc;

	
#### 作为参数

	setTimeout(myFunc,0);
	
#### 作为返回值

	return myFunc;
	
#### 作为构造函数

	new myFunc();
	
#### 函数声明

	function myFunc(){}
	
函数声明会提前。
	
#### 变型金刚

	(function(){})();
	
	(function(){}());
	
	myObj['myFunc']();
	
	(function(){
		return function(){};
	})()();

### 有原型

	Function.prototype.myProp = 123;
	var a=function(){};
	a.myProp; //123

## 返回值

使用return返回值并结束函数执行。如果没有return语句，则返回值为`undefined`。
	
## 4种调用模式

1. 普通调用
2. 方法调用
3. apply/call调用
4. 构造函数调用

## 参数

调用的参数称为“形参”，函数接受的参数称为“实参”。

形参、实参个数可以不匹配，多余的参数会被忽略，少的参数为undefined（用于防止undefined被覆盖）。

## 作用域

作用域用于控制变量的可见性和生命周期。JavaScript中的作用域叫“函数作用域”。

	var a=1;
	function func1(){
		var a=2;
		console.log(a);
	}




