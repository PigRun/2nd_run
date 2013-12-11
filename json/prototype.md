#原型

在javascript中,对象都有一个原型(prototype)属性,每个实例继承的都是构造函数中的原型属性中的属性.

```
	var a = function(){console.log("func a is running;");}
	a.prototype.newProperty = "hi,wtf";
	a.prototype.newProperty2 = function(){
		console.log(this.newProperty);
	};

	var b = new a();
	b.newProperty2();
	console.log(b);

```


prototype是一个object对象,javascript中一个函数所衍生的实例全部属性都继承自该函数的prototype.



###应用:

如果我们想要给Object的所有实例都加上一个create属性,来兼容ie6,那么我们可以加上下面这段代码:
```
	if(typeof Object.create !== 'function'){	//判断Object中没有这个函数
		Object.create = function(o){			//传入原型函数,返回新的实例.
			var F = function(){};
			F.prototype = o;
			return new F();
		}
	}


	var obj1 = {
		a : "value1",
		b : "value2"
	}

	var obj2 = Object.create(obj1);
	console.log(obj2);


	obj1.c = "50";

	console.log(obj2);
```

需要提醒的是,原型连接在更新时是不起作用的,一个对象在更改其自己的属性的时候,不会影响到生成他的原型.

比如上述例子,obj2是以obj1为原型生成的,但是修改obj2的属性,obj2的原型,即obj1是不会改变的.

但是一旦obj1被修改,那么obj2也会随之被修改.


###原型继承以及原型链

注意!

原型拓展中,建议尽量用点语法来拓展而不是传入对象:

```
	//正确写法
	Object.prototype.a = function(){};

	//错误写法
	Object.prototype = {
		a : function(){}
		//...
	};

```
错误的书写方法不会导致语法错误,但是这样会覆盖掉Object原有原型中的所有方法,包括toString等原有的方法和属性会被一概删除掉.



再看一个例子:

```
	var a = function(){}
	var b = new a();
	console.log(b.toString);

```
b为什么会有toString方法?

题目:

	jQuery中, 
	$.plugin = function(){};
	$.prototype.plugin =  function(){};

	有什么不同?
	

##什么是原型继承



请问:
```
	var a = function(){
		this.bb = "6";
	}
	a.prototype = {
		bb : "5"
	}

	var b = new a();

	console.log(b.bb);

```

b.bb的值?

第一种使用this来赋值给实例的方法有一个公用的名词,叫做工厂模式,可以批量复制出一大批一模一样的对象.
而使用原型来继承实现赋值给实例的方法则叫做原型继承.


在理论上来讲,原型继承的属性,其实并不属于对象真正的属性,其只是在对象内部创建一些指针,指向这个原型,所以原型的修改伴随着所有实例该属性的修改.

而在一个object中访问一个属性,比如上面的b的bb属性,其过程是:

	遍历该对象自有的属性
	遍历该对象prototype的属性
	遍历该对象prototype的constructor的prototype的属性
	遍历该对象prototype的constructor的prototpye的constructor的prototype的属性
	...

比如下面这段程序:

```
	var Person = function(){
		this.age = 15;
	}
	var person1 = new Person();
	var person2 = new Person();
```

如果说原型的图是这样的:

![e1.png](https://github.com/PigRun/2nd_run/blob/master/json/img/e1.png?raw=true)

那么原型链继承的图就是这样子的:

![e2.png](https://github.com/PigRun/2nd_run/blob/master/json/img/e2.png?raw=true)

```
	Object.prototype.constructor.prototype === Object.prototype ;//true;
```

Object.prototype是任何js原型链的终点,这个在原型链里寻找属性的过程就叫做委托.而如果直到终点都没有找到这个属性则返回undefined

如果我们需要的是所有实例都一样的属性,且构造函数是我们可以控制的,则使用this工厂模式方法
而如果我们创建的是不同实例需要不同,或者构造函数不可控(Array的range方法),则应该用prototype来生成.