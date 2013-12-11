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



isPrototypeOf() 方法可以判断对象是否原型:

	obj1.isPrototypeOf(obj2);



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


在严格意义上来讲,原型继承的属性,其实并不属于对象真正的属性,其只是在对象内部创建一些指针,指向这个原型,所以原型的修改伴随着所有实例该属性的修改.



而在一个object中访问一个属性,比如上面的b的bb属性,其过程是:

	遍历该对象自有的属性
	遍历该对象prototype的属性
	遍历该对象prototype的constructor的prototype的属性
	遍历该对象prototype的constructor的prototpye的constructor的prototype的属性
	...

比如下面这段程序:
再看一个例子:

```
	var a = function(){}
	var b = new a();
	console.log(b.toString);

```
b为什么会有toString方法?

```
	var Person = function(){
		
	}
	Person.prototype.age = 15;
	var person1 = new Person();
	var person2 = new Person();

	person2.age = 16;
	console.log(person2.hasOwnProperty("age"));
	delete person2.age;

	console.log(person2.age);
	console.log(person2.hasOwnProperty("age"));
	Person.prototype.constructor === person1.constructor //true
```

原型链继承的图就是这样子的:

![e1.png](https://github.com/PigRun/2nd_run/blob/master/json/img/e1.png?raw=true)


```
	Object.prototype.constructor.prototype === Object.prototype ;//true;
```

Object.prototype是任何js原型链的终点,这个在原型链里寻找属性的过程就叫做委托.而如果直到终点都没有找到这个属性则返回undefined


这样上面的bb这个例子我们可以看作是:
	我们在构造函数上的原型添加了bb属性,然后又用this在实例本身上面添加了bb属性.这样的话访问这个属性,程序是会从实例本身开始遍历,找不到才开始往上寻找实例的原型中的属性,但是如果实例本身已经有这个bb属性,那么原型里面的同名属性就会被覆盖.






图片中的[[prototype]]是object的一个内置的属性,但是这个属性是不可读的.我们只需要知道有这么一个属性是指向创造自己的原型的.
而在ES5中,getPrototypeOf() 方法则可以获取到对象的原型.

前天晚上说到的hasOwnProperty就可以用来检测这个属性是来自于实例本身,还是来自于原型:



###使用纯原型链继承的问题

```
	var sup = function(){};
	sup.prototype.colors = ["red","green","blue"];
	var sub = new sup();
	var sub2 = new sup();


	sub.colors.push("black");

	console.log(sub2);

```
	

如果需要共享的属性,则用原型创造,如果需要单独的属性,则用构造函数的this来创建.




###其他继承方法

构造函数(略)
原型继承(略)

混合式继承(略 常用)

####原型式继承
	
	function object(o){
		function F(){}
		F.prototype = o;
		return new F();
	}


	var person = {
		name: "json",
		age : 22
	}

	var anoterPerson = object(person);

传入原型,进行浅复制,返回一个全新的对象. 新的浏览器已经完全支持这种方法了 : Object.create();

####寄生式继承
####寄生组合式继承
