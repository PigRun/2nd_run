# JavaScript对象

JavaScript中将的最多的东西莫过于对象了。对象是一个无需属性的集合，这些属性可以是基本类型的值，可以是函数，对象。通常用来封装数据，管理数据。

## 对象的创建

有两种常见的创建对象的方式，分别是对象字面量和借用构造函数，使用`new`关键字创建对象。

```javascript
// 使用对象字面量创建对象
var o = {
	name: 'userName',
	age: 100,
	sayHi: function() {
		console.log('Hello World!')
	}
}

// 借用构造函数创建对象
var obj = new Object();
var str = new String('JavaScript');
// ... more
```

虽然在这门语言中包含一系列内置的对象，和全局对象。我们也可以使用自定义构造函数的方式在生成对象：

```javascript
function Person(name, age) {
	this.name = name;
	this.age = age;
	this.fn = function() {
		console.log('我是实例方法');
	}	
}
// 使用`new`关键字生成对象
var person = new Person('userName', 100);
person.name;
person.age;
person.fn();
```

在ECMAScript 5中新增了一个创建对象的方法，， `Object.create()`。

```javascript
var o = Object.create(PrototypeObject);
```

这个方法接受一个作为新建对象原型的对象作为参数。使用`Object.create()`创建对象时，还可以给他传递初试属性：

```javascript
var o = Object.create(Object.prototype, {
	name: {
		value: 'userName',
		writable: true
	},
	otherProperty: {
		value: 'xxxx',
		get: function() {},
		set: function() {}
	}
});
```

## 对象属性操作

对象的属性与属性值是可变的。因为可以很轻松的为对象分配新的属性，获取对象的属性。

```javascript
// 检索对象属性
var o = {
	name: 'userName',
	prop: 'xxx'
}
o.name; // 使用点表示法读取属性
o['name']; // 使用中括号表示法读取属性

// 也可以检索对象属性时修改属性值
o.name = 'basecss';

// 为对象添加属性或
o.age = 100;
o['age'] = 100;

// 删除对象属性
delete o.prop;
delete o['prop'];
```

通过对对象的增删改操作，很容易实现对象的更新。

## 对象枚举

和数组类似，也可以通过枚举的手段列出对象的属性：

```javascript
var o = {
	name: 'userName',
	age: 100
}

// 使用`for in`枚举对象属性
for(var key in o) {
	console.log(o);
}
```

ECMAScript 5中提供了一个`Object.keys()`方法，这个方法会列出对象的所有**自有属性**：

```javascript
Object.keys(o); // ['name', 'age'];
```

## 对象检测

有两个运算符可以用来检测对象，分别是`typeof`和`instanceof`：

```javascript
var o = {};
typeof o; // 'object' 返回指定对象所属类型字符串
o instanceof Object; // true/false  这个运算符可以检测指定对象是否属于某个特定类型的实例
```

也可以通过每个对象都有的`constructor`属性来判断对象是否某个构造器的实例对象：

```javascript
function Person() {}
var person = new Person();
person.constructor === Person;
var arr = [1,2,3];
arr.constructor === Array;
```

更多关于类型检测的信息可以参考：[浅析JavaScript之类型检测](https://github.com/basecss/basecss.github.io/issues/3)

## 对象特征

以下是每个对象都有的一些内部属性和方法：

- `constructor` 创建当前对象的函数
- `hasOwnProperty(propertyName)` 检查当前属性在对象实例中是否存在
- `isPrototypeOf(obj)` 检查调用这个方法的对象是否传入参数对象的原型
- `propertyIsEnumerable(propertyName)` 检查给定的属性是否能够用`for-in`枚举
- `toLocaleString()` 返回对象字符串标识，与执行环境的地区对应
- `toString()` 返回对象字符串标识
- `valueOf()` 返回对象的字符串，数值或者布尔值表示。通常和`toString()`返回值相同

在前面`for in`枚举中，可能会获取到原型中的属性。因此，可以借助这里的`hasOwnProperty()`方法来过滤原型属性：

```javascript
var o = {
	name: 'xxx',
	age: 123,
	otehr: 'ooooo'
}
var keys = [];
for(var key in o) {
	if(o.hasOwnProperty(key)) {
		keys.push(key);
	}
}
```

## 对象原型

每个对象都会*连接*到一个原型，它能够从原型中集成属性和方法。在创建对象对象时，也可以使用某个对象作为实例对象的原型。

```javascript
function Person() {}
Person.prototype.name = 'xxx';
var person = new Person();
person.name; // 实例本身没有这个属性，但会查找原型获取能访问到的属性
// 获取对象的原型
Object.getPrototypeOf(person);
// 使用Object.create()设置对象原型
var o = Object.create(Object.prototype);
// 访问继承属性和方法
o.toString(); // '[object Object]'
o.constructor; // function Object() { /*[native code]*/ }

## 使用对象

通常，如果不希望将大量的变量，方法暴露在全局作用域中以避免污染全局命名空间，可以创建唯一的对象将代码封装在这个唯一的对象里面：

```javascript
if(typeof app === 'undefined') {
	var app = {};
}
app.name = 'xxx';
app.method = function() {
	// ...
}
// 使用
app.name();
app.method();
app.method(app.otherProperty);
```

这种方式虽然可以有效的减少全局命名空间污染的问题，但是过渡的使用也会带来问题，比如：

```javascript
app.moduleA.moduleB.moduleC.moduleD.method();
```

深层的嵌套，导致访问每个属性是都需要进行查询，这回带来管理维护的问题，从性能的角度而言也会有一些问题。在实际使用中，需要进行衡量；或者可以借助于模块化管理工具，合理进行代码模块管理。