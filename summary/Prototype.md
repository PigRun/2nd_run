# 原型

正如上一篇**对象**介绍中提到的，JavaScript中每个对象都*连接*到一个**原型对象** - `prototype`，而这个原型中包含着这个对象继承的属性和方法。

而JavaScript正是基于原型实现继承的。从严格意义上将，对象并没有原型，这里所说的原型实际上是对象构造器的原型。

```javascript
var o = new Object();
o.toString();
o.hasOwnProperty('toString'); // false
Object.prototype.hasOwnProperty('toString'); // true

function Person() {
	
}
Person.prototype.name = 'person';
var person = new Person();
person.name; // 'person'
person.hasOwnProperty('name'); // false
```

值得注意的是`prototype`是一个对象。

```javascript
tyepof Object.prototype; // 'object'
typeof Person.prototype; // 'object'
Array.prototype; // []
typeof Array.prototype; // 'object', 注意这里，数组也是对象
```

## 应用

基于对象实例继承原型的特性，便可以构造出不同对象的继承关系。比如，ECMAScript 5中的`Object.create()`方法接受一个作为新对象原型的参数，对于不兼容ECMAScript 5的低版本浏览器，借助这一特性也很容易实现这个方法：

```javascript
if(typeof Object.create !== 'function') { // Object.create()是一个方法函数，因而检测的时候直接检测它是不是函数类型即可 
	// 为不支持这个方法的函数实现这一方法
	Object.create = function(protoObject) { 
		// 在这里，我们要将传入的protoObject作为新实例的原型对象，因而需要借助一个构造函数
		// 使用一个“空”构造函数即可
		var F = function() {};
		// 将传入的对象作为这个临时构造函数的原型
		F.prototype = protoObject;
		// 返回临时构造函数实例
		return new F();
	}
}

// usage
var o = {
	name: 'xxx',
	age: 100
}
var instance = Object.create(o);
instance.name; // 'xxx'
```

虽然实例对象可以从它的构造器原型中继承属性，但是我们也可以给实例对象添加属性：

```javascript
// 沿用上例代码
instance.name = 'instance';
instance.name; // 'instance'
instance.__proto__.name; // 'xxx'
```

注意，这里我们修改了实例的`name`属性，而其原型中的`name`属性并没有改变。也就说，实例无法修改原型属性。

## 原型判断与提取

JavaScript中有两种方式来检查或者提取对象实例的原型，我们知道每个对象都有一个`isPrototypeOf()`方法用来检测原型。

```javascript
o.isPrototypeOf(instance); // true
// 给定的参数是调用这个方法的对象的原型时，这个方法才会返回true
```

在ECMAScript 5中还提供了另外一个方法提取对象的原型：

```javascript
Object.getPrototypeOf(instance); // Object {name: "xxx", age: 100}
Object.getPrototypeOf(instance) === o; // true
```

## 原型扩展

JavaScript中的原型是可扩展的，比较典型的是在早期的JavaScript库中，大多数为处理低版本浏览器的兼容性以及为了方便使用都使用原型扩展的方式提供了很多前卫的功能和实现。

扩展原型非常容易，只需简单的给原型对象添加属性和方法即可：

```javascript
Object.prototype.sayHi = function() {
	console.log('Hello, JavaScript');
};
var o = {};
o.sayHi(); // 'Hello, JavaScript'
```

在使用自定义构造函数来创建对象时，也可以将共享属性和方法直接定义在构造函数的原型中，以避免每次新建对象实例都需要重复的生成相同的属性和方法：

```javascript
function Person(name, age) {
	this.name = name;
	this.age = age;
}
Person.prototype.sayHi = function() {
	console.log('Hello, ' + this.name);
}
var person = new Person('xxx', 100);
person.sayHi(); // 'Hello, xxx'
```

在上面的示例代码中我们都是单独新增属性或者方法，事实上还可以使用对象字面量的方式一次性定义更多的属性和方法：

```javascript
Object.prototype = {
	prop: 'xxx'
	// ...
};
Person.prototype = {
	constructor: Person, // 注意这里
	sayHi: function() {}
}
```

在使用对象字面量扩展原型时需要注意，使用这种形式扩展内置构造器原型时很容易重写该构造器原型，从而丢失很多内置的方法，比如重写`Object`原型而丢失`toString()`等方法。

对于构造器原型，使用对象字面量定义构造器原型是千万要注意：**每个对象都有一个`constructor`属性指向这个对象的构造器，而默认情况下这个`constructor`属性继承自原型**。因而没了避免对象与构造器关系的丢失，千万要注意这个属性的正确指向问题，尤其是在处理继承的时候。

对于原型最典型的应用莫过于在使用jQuery编写插件，思考一下以下两种形式的区别：

```javascript
jQuery.pluginName = function(){};
jQuery.prototype.pluginName = function() {};
```

## 基于原型的继承

先来看一个例子：

```javascript
var Person = function() {
	this.name = 'name';
	// 注意这里的this指向使用这个构造器生成的对象实例
}
Person.prototype.name = 'myName';

var instance = new Person();
console.log(instance.name); // 'name'
console.log(instance.toString()); // '[object Object]'
```

上面说过，实例对象会继承构造器原型中的属性。然而为什么这个例子中没有继承原型中定义的`name`属性呢？这就需要了解对象查找属性的方式：

- 对象查找属性是首先会找自身有没有这个属性
- 如果自身没有要查找的属性它会从构造器原型中查找
- 如果在构造其原型中没有找到，则继续查找构造器所继承的原型中有没有
- 最后直到找到`Object.prototype`，有则返回，没有则返回`undefined`

其过程类似于：

```javascript
var person = new Person();
person.xxx -> Person.prototype -> Function.prototype -> Object.prototype
// 在这个过程中，只要在靠近左侧的对象中找到就返回这个属性，没有则继续，直到找到Object.prototype
```

因而，在上面的例子中`instance.name`从自身找到`name`属性，并不会查找构造器的原型。而访问`instance`的`toString()`方法时，由于自身没有便从原型中不断查找，而我们知道每个对象都有继承自`Object.prototype`的`toString()`方法。

## 关于原型的一些问题

理解原型这一概念，在处理对象继承时便更加轻触了。然而，这里还有一些问题需要解释。

前面提到，对于不同的对象实例共享的属性和方法定义在原型中。这是因为如果定义在构造函数中，在每一次创建对象实例时，都会生成同名方法，而这些同名方法并不相等，例如：

```javascript
function Person() {
	this.sayHello = function() {};
}
Person.prototype.sayHi = function() {};
var a = new Person();
var b = new Person();
a.sayHello === a.sayHello; // false
b.sayHi === a.sayHi; // true
```

因此在使用对于共享的属性和方法更加建议将它们定义到对象的原型中，从而避免重复的创建。