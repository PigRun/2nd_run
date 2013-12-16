# JavaScript 对象

*********************

- 对象
- 对象的创建
- 对象的特性
- 对象属性操作
- 对象判断
- 对象传递
- 对象遍历
- 对象复制
- 对象保护
- 对象相关的一些模式

## 对象

### 定义

ECMA-262定义：无序属性的集合，其属性可以包含基本值、对象或者函数。

通俗的讲，对象就是一组属性与属性值的集合，这个名值对集合是可变的。

**Example**

	// 创建一个名为 o 的对象
	var o = new Object;

	// 写入 o 的 name 属性
	o.name = 'userName';

	// 写入 o 的 age 属性
	o.age = 22;

	// o 的 sayHi 方法
	o.sayHi = function() {
		console.log('Hi!');
	}

	// 查看属性
	console.log(o.name); // 'userName'
	console.log(typeof o.name); // 'string'

	console.log(o.age); // age
	console.log(typeof o.age); // 'number'

	o.sayHi(); // 'Hi!'
	console.log(typeof o.sayHi); // 'function'

*********************

## 对象创建

JavaScript内置的对象有Object, Array, Date, RegExp, Functiion, 基本类型的包装类型, 其他内置对象，诸如Math对象等

### 对象字面量方式

对象字面量就是一组包含在花括号中的名值对.

	var _config = {
		name: 'userName'
	}
	
	var o = {
	    init: function(){
	        // do something
	        return newObj;
	    }
	}

### 构造函数模式

利用特定的构造函数创建指定类型的对象。Object和Array都属于原生的构造函数，也可以使用自定义的构造函数。在构造函数中可以初始化实例对象的一些属性和方法。

	function MyObject(name,age,job) {
		this.name = name;
		this.age = age;
		this.job = job;
		this.sayHi = function() {
			console.log('Hi!');
		}
	}

然后使用 `new` 操作符调用构造函数来创建对象实例。

	var o = new MyObject('name',100,'fe');
	// usage
	o.name; // 'name'
	o.age; // 100
	o.sayHi(); // 'Hi!'

实际上使用构造函数的方式会经历以下几个步骤：

- 创建一个新的对象
- 将构造函数作用域赋给新建的对象，也就是将新建的这个对象赋给`this` - `this`指向这个新建的对象
- 执行构造函数中的代码，比如为新建的对象添加属性和方法
- 最后返回新建的对象，也就是返回 `this`

那么在创建对象实例的时候，在构造函数内部的 `this` 实际上就是指向我们创建的对象实例的。

> 给构造函数传递参数时，缺省的形参值为 `undefined`.

### Object.create()

创建一个拥有指定的原型和可选的多个指定属性的对象实例。

	var o;
	o = Object.create(Object.prototype, {
		name: {
			value: 'name',
			writable: true
		}
		otherProperty: {
			get: function(){},
			set: function(){},
			configurable: true
		}
	});

- `Object.create()` 必须传递一个作为新创建对象原型的对象
- `Object.create(null)` 创建一个真正的空对象
> 实际上 `var o = {}` 并不是一个真正的空对象，因为它有继承自 `Object.prototype` 的属性和方法，比如 `toString()`。
- `Object.create()` 方法给对象示例设置属性时属性值必须是一个对象描述符

*********************

## 对象属性和方法

### 内置属性和方法

每个对象都有自身的内置属性和方法：

- `constructor` 创建当前对象的函数
- `hasOwnProperty(propertyName)` 检查当前属性在对象实例中是否存在
- `isPrototypeOf(obj)` 检查调用这个方法的对象是否传入参数对象的原型
- `propertyIsEnumerable(propertyName)` 检查给定的属性是否能够用`for-in`枚举
- `toLocaleString()` 返回对象字符串标识，与执行环境的地区对应
- `toString()` 返回对象字符串标识
- `valueOf()` 返回对象的字符串，数值或者布尔值表示。通常和`toString()`返回值相同

ES5中还定义了一些好用的方法:

	Object.getOwnPropertyNames()
	Object.getPrototypeOf();

### 对象特性

ECMAScript5 中为给对象定义了一些只有内部才用的特性，用来描述对象属性的特征。

> 这些特性通常是为了和JavaScript引擎实现相关的，在JavaScript中并不能直接使用这些特性。

ES5中两种类型的特性属性：

**数据属性**：

- [[Configurable]] 是否可配置，比如`delete` 属性后重新定义，修改特性类型，修改属性的其他特性，默认为`true`
- [[Enumerable]] 是否可用 `for-in` 枚举 默认为 `true`
- [[Writable]] 是否可修改属性值 默认为 `true`
- [[Value]] 对象属性的至 默认为 `undefined`

**访问器属性**:

内置的这些访问器类型的属性都不包含数据值。

- [[Configurable]] 是否可配置，与数据属性[[Configurable]]类似,默认为 `true`
- [[Enumerable]] 表示是否可没据属性，默认为 `true`
- [[Get]] 读取属性调用的函数，默认为 `undefined`
- [[Set]] 写入属性调用的函数，默认为 `undefined`

上面这些特性只能通过ES5提供的`Object.defineProperty()` 才能修改。

### 读取对象特性

ES5中定义了一个 `Object.getOwnPropertyDescriptor()` 方法用于读取给定对象属性的描述符

参数：指定对象，希望读取属性描述符的指定属性

返回：对象，根据对象属性的特性返回不同的值

	var des = Object.getOwnPropertyDescriptor(o, 'name');
	name.configurable;

****************************

## 对象的属性和方法

### 设置属性

定义对象的属性有三种方法：

#### 点表示法

	var o = {};
	o.name = 'value';

#### 方括号表示法

	var o = {},
		prop = "age";
	o['name'] = 'value';
	o[prop] = 22;

#### Object.defineProperty() && Object.defineProperties()

这两个方法是在ES5中定义的

	var o = {};
	Object.defineProperty(o, 'name', {
		value: 'userName',
		writable: false
	});

> 参数：对象，属性名，属性描述符
> 
> 描述符: `configurable`, `enumberable`, `writable`, `value` 中的一个或者多个

	Object.defineProperty(o, 'age', {
		get: function(){
			return this._prop;
		}
		set: function(prop) {
			this._prop = prop;
		}
	});

定义多个属性

	Object.defineProperties(o, {
		a: {
			value: 'valA'
		},
		b: {
			value: 'valB'
		}
	});

> 属性都必须使用属性描述符定义

### 读取属性

#### 点表示法

	o.name;

#### 方括号表示法

	var o = {
		'a-1': 123
	};
	o['a-1'];

> 定义已存在的属性会覆盖原有属性的值， 否则新增属性

> 读取不存在的属性返回undefined


### 删除属性

使用 `delete`运算符可以删除对象的属性

	delete o.name;

> `delete` 不会删除原型中的属性

> ES5 中 `configurable` 设置为 `false` 的属性不能使用 `delete` 删除并且会导致错误

******************

## 对象判断

### 类型判断

**tyepof 运算符**

返回一个字符串，标识给定的运算数是什么类型


	typeof o; //'object'

**instanceof 运算符**

	{} instanceof Object;

如果左侧是右侧的实例返回 `true`

**constructor 属性**

每个对象都有，引用初始化对象实例的构造函数

	o.constructor === Object;

### 存在性判断

方法多种多样

	var o;
	if(!o) {
		o = {};
	}

	if(!obj) {
		var obj = {};
	}

	if(o === undefined) {
		o = {};
	}

### 相等性判断

******************************

## 对象传递

对象是按引用传递的。

	var o = { name: 123 }
	var o2 = o;
	o2.name === 123;
	o2.age = 100;
	o2.age === o.age;

	var a = {},
		b = {};

	a !== b;

	var a, b;
	a = b = { name: 123 }
	a === b;
	a.name === b.name;

************************
## 对象遍历

for-in语句可用来编辑对象中的所有属性，可能会包括原型中的属性

	for(var key in o) {
		console.log(key);
	}

如果需要过滤来自原型的属性，可以使用`hasOwnProperty()`方法来进行过滤

ES5中定义了一个 `Object.keys()` 方法来枚举对象自身的属性，返回一个属性名组成的数组。

	Object.keys(o);

> 对象属性顺序是不确定的，如果依赖顺序就需要进行额外的操作。比如将属性存储在数组中使用`for`循环遍历。

***********************************

## 对象复制

### 浅复制
	
	/*
	 * 浅复制，子类实例引用类型属性的修改会影响父类对应的属性
	 */
	function clone(parent, child) {
	    var i;
	        
	    child = child || {};
	        
	    for(i in parent){
	        if(parent.hasOwnProperty(i)){
	            child[i] = parent[i];
	        }
	    }
	    
	    return child;
	}

### 深复制

	/*
	 * 深复制
	 */
	function deepClone(parent, child) {
	    var i,
	        toStr = Object.prototype.toString,
	        arrStr = ['object Array'];
	        
	    child = child || {};
	    
	    for(i in parent) {
	        if(parent.hasOwnProperty(i)) {
	            if(typeof parent[i] === 'object') {
	                child[i] = (toStr.call(parent[i]) === arrStr) ? [] : {};
	                deepClone(parent[i], child[i]);
	            } else {
	                child[i] = parent[i];
	            }
	        }
	    }
	    
	    return child;
	};

	// or
	var str = JSON.stringify(o);
	var newObj = JSON.parse(str);

***********************************

## 对象保护

ES5中定义了一些方法可以让对象不可扩展，密封对象，冻结对象。这些方法旨在保护对象。分别是 `Object.preventExtensions(myObject)` , `Object.seal(myObject)`, `Object.freeze(myObject)`

### 禁用扩展

使用 `Object.preventExtensions()` 方法可以让给定的对象不再可以扩展, 也就是不能够再添加新的属性，包括不能在原型上添加属性。

使用这个方法禁用扩展后的对象在严格模式中可能会抛出TypeError错误。

可以扩展对象可以变得不可扩展，但是反过来不行。

可以使用 `Object.isExtensible()` 方法检测对象是否可扩展。 

### 密封对象

使用 `Object.seal()` 方法可以密封一个对象并返回密封后的对象。密封后的对象不能添加属性，撒谎年初属性，修改属性的可枚举，可写性，可配置性。 但可能可以修改已有属性的值。

可以使用 `Object.isSealed()` 方法检测对象是否可扩展

### 冻结对象

使用 `Object.freeze()` 方法冻结对象并返回冻结后的对象。冻结后的对象永远不可变。

`Object.isFrozen(obj)` 判断一个对象是否被冻结。

*************************************

## 对象相关的一些模式

### 命名空间模式

可以使用对象字面量的来维护代码，这样可以减少全局变量的数量。

	if(typeof module === 'undefined') {
	    var module = {};
	}

创建一个全局唯一的变量，将这个变量作为一系列代码的容器。然后在容器中添加不同的功能：

	module.getId = function(id) {
		return document.getElementById(id);
	};

	module.NAME = 'MyApp';
	module.assets = {
		js: {
			jQuery: '/static/jquery/jquery-2.0.3.min.js'
		},
		css: {
			common: '/static/css/common.css'
		}
	};

将不同的代码组织在对象中可以有效的减少命名冲突。但是过渡的使用也容易引起引用问题，比如

	moduleA.modA.modB.modC.methodA();


