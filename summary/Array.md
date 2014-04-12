#JavaScript数组

数组是JavaScript最常用的类型之一，也是一种非常出色的数据结构，可以对它进行排序，求值。

JavaScript中的数组与其他语言中的数组一样都是一个有序排列的列表，不同的是在JavaScript中数组项，或者说数组元素可以存储任意类型的数据。

本质上，在JavaScript中数组是一个对象，但是它也有自己的属性和方法。和对一样它可以使用属性名来检索元素，此外它还可以使用整数作为索引来检索数组项。接下来，我们一起来学习一下数组：

## 创建数组

创建数组的方式有两种，分别是：

```javascript
var arr = new Array(); // 借用Array构造其创建实例
var arr = []; // 使用字面量的方式
```

不但如此，我们还可在创建数组时，为数组分配初始化元素：

```javascript
var arr = new Array('a', 'b', 'c');
var arr = [1,2,3,4,5];
```

值得注意的是，在使用`Array`构造器创建数组实例时，如果直接传递一个整数给构造器，最终结果并不会将给定的整数作为数组元素，而会创建指定数量的元素，其中每个元素的值都为`undefined`。

```javascript
// 反模式
var arr = new Array(3);
console.log(arr); // [undefined, undefined, undefined];
```

## 数组属性

数组有一个`length`属性，表示数组的长度，也就是数组中元素的个数。

```javascript
var arr = [1,2,3];
console.log(arr.length); // 3
```

这个`length`属性不能获取数组的长度，它还能操作数组。比如，清空，删除，新增：

```javascript
// 以下代码仅作示例
var arr = [1,2,3,4,5,6];
arr.length; // 6
arr.length = 5; // 将数组的长度改变为5，这会删除最后一个元素
// 此时检索最后一个元素结果便是5
arr[arr.length - 1]; // 5，注意默认情况下数组下表索引是从0开始，因此length - 1便是最后一个元素索引
// 通常访问第6个元素结果就是undefined
arr[6]; // undefined
// 还可使用length属性清空数组
arr.length = 0; // 将数组的长度变成0，直接清空所有元素
arr; // []
// 新增元素
arr[arr.length] = 7;
arr; // [1,2,3,4,5,6,7];
arr.length = 10; // 新增多个元素，新增的每一项都为undefined
arr; // [1,2,3,4,5,6,7,undefined,undefined,undefined];
```

## 删除数组元素

删除数组的元素的方式有多重，除了上面提到的`length`属性，还可以使用`delete`和`splice()`来删除数组元素。

```javascript
var arr = [1,2,3,4,5];
delete arr[1]; // 删除指定索引的元素后，这个位置的元素将变成undefined
arr.splice(3, 1); // 指定从索引为3的元素开始，删除1个元素，剩下的元素会往前移动
```

注意，如上所述`delete`删除后，指定位置的元素会变成`undefined`，因而建议在日常开发中尽量使用可靠的`splice()`方法。

除此之外，有两个方法可以分别从首位移除元素，并返回被移除的元素，分别是`pop()`和`shift()`。

```javascript
var arr = [1,2,3,4,5];
arr.pop(); // 5 获取最后一项，从数组中移除这一项，然后返回被移除的这一项
arr.shift(); // 1，与pop相反，移除第一项
```

## 为数组新增元素

除使用`length`属性外，为数组新增元素的方式也有多种。分别是使用`push()`方法从数组后面插入新元素，使用`unshift()`方法在开始绘制插入新元素，使用`splice()`方法插入元素。

```javascript
var arr = [1,2,3];
arr.push(4); // 从最后面插入一项，可以插入多项，也可以插入数组
arr.unshift(0); // 从第一位插入一项，也可以插入多项
arr.splice(2,0,'new1', 100); // 在索引为2的元素后面插入2项
```

不难发现，`splice()`方法很强大，实际上还可以使用它替换数组元素：

```javascript
var arr = [1,2,3,4];
arr.splice(2, 1, '1', '2');
// 原理
// 首先从索引为2的元素开始，删除1个元素
// 然后使用其余的参数替换这个被删除的元素
// 如此便实现了元素插入操作
```

值得注意的是，使用`splice()`方法操作数组时，数组的元素是*活动的*，也就是说，增删改会改变数组的长度，同时它还会自动*流动*补齐，并不会像`delete`那样留空。

## 基于数组的合并与新建操作

JavaScript中还可以基于现有的数组，合其他数组元素以及创建新的数组。

```javascript
// 使用concat()方法合并数组
var arr = [1,2,3];
var arr2 = ['a', 'b', 'c'];

var result = arr.concat(arr2);
result = [1,2,3,'a','b','c'];
// 这会将被合并数组的每一项与合并数组合并
// 合并操作返回新的数组，不会改变原数组

// 使用slice()方法新建数组
var arr = [1,2,3,4,5,6];
var new1 = arr.slice(1); // [2,3,4,5,6]
var new2 = arr.slice(1, 3); // [2,3]
// slice()方法接受两个参数
// 第一个参数表示起始项，第二个表示结束位置
// 如果没有传递结束位置，则返回起始项到末尾的元素
// 如过传递结束位置，则返回起始项至结束项之前的元素
```

## 遍历数组

由于JavaScript中的数组也是对象，因此可以使用`for in`循环遍历数组。

```javascript
var arr = ['a','b','c'];
for(var item in arr) {
	console.log(item);
}
```

但是需要注意的是，由于数组也是对象，而`for in`循环可能会返回原型中的属性和方法，因此不建议使用`for in`方法。

我们都知道`for`循环是用来遍历序列的。因而，可以也可以用`for`循环遍历数组。

```javascript
var arr = [1,2,3,4];
for(var i = 0, l = arr.length; i < l; i++) {
	// 注意，这里缓存了数组的长度
	console.log(arr[i]);
}
```
ECMAScript 5中还提供了新的迭代数组的方法，接下来一起看看。

## ECMAScript 5中的数组迭代方法

ECMAScript 5中定义了5个数组迭代方法。每个方法都接受两个参数，在每一项元素上执行的迭代函数，一个可选的运行迭代函数的作用域对象（这个作用域对象用于影响`this`值）。

其中每个迭代函数接受三个参数：数组项的值，该数组项的索引位置，数组自身。

这5个迭代方法分别是：

- `every()` 对数组的每一项运行给定的函数，如果每次运行都返回`true`，则返回`true`。
- `filter()` 对数组每一项运行给定函数，返回运行这个函数结果为`true`的数组元素。
- `forEach()` 对数组的每一项运行给定的函数，没有返回值。可用来遍历数组。
- `map()` 没数组每一项运行给定函数，返回每个数组项调用该函数之后的结果组成的新数组。
- `some()` 与`every()`方法类似，不同的时在对每一项运行给定函数时，只要任意一项运行给定函数返回`true`，这个方法都返回`true`。

示例：

```javascript
var arr = [1,2,3,4,5];
var everyTrue = arr.every(function(item, index, array) {
	return item > 0;
}); // true, 因为每一项都大于0
var someTrue = arr.some(function(item, index, array) {
	return item > 3;
}); // true, 因为有大于3的数组项
var filterItems = arr.filter(function(item, index, array) {
	retun item > 2;
}); // [3,4,5] 返回大于2的数组项组成的新数组
var mapItems = arr.map(function(item, index, array) {
	return item + 1;
}); // [2,3,4,5,6] 对每一项加1，然后返回加1后的新数组
var forEachItems = arr.forEach(function(item, index, array) {
	console.log(item); // 依次输出每一项，迭代数组，没有返回结果
});
```

## 数组排序

数组中有两个用于排序的方法，分别是`sort()`和`reverse()`。

顾命思议`reverse()`方法用于反转数组的元素顺序：

```javascript
var arr = [1,2,3];
arr.reverse();
arr; // [3,2,1]
```

但是，有时候可能希望按照指定的规则排序数组。因此便有了`sort()`方法。`sort()`方法默认按升序排列数组元素。

```javascript
var arr = [0,1,12,6,16];
arr.sort();
arr; // [0, 1, 12, 16, 6]
```

咦，不是说升序的么？为毛上面的结果这个坑爹。**咳咳，注意啦：`sort()`方法实现排序时，会对每个数组项执行`toString()`方法，然后比较的是字符串。即使数组元素都是数字。**。因此，为了准确的实现排序，可以传递一个指定的排序函数给`sort()`方法，然后按照这个指定的函数定义的规则排序。

传递给`sort()`的比较函数接受两个参数：如果第一个参数应该在第二个之前则返回负值，如果应该在后面则返回正值，如果相等则返回0。因此纠正上面的排序很简单：

```javascript
function compare(a, b) {
	if(a < b) {
		return -1;
	} else if(a > b) {
		return 1;
	} else {
		return 0;
	}
}
var arr = [0,1,12,12,6,16];
arr.sort(compare);
arr; // [0, 1, 6, 12, 16]
```

切记数组排序时，执行的是字符串比较。

## 应用

### 队列

排队吃饭，先到先得

```javascript
var people = ['a', 'b', 'c', 'd', 'e', 'f'];

var queue = [];
people.forEach(function(value, index){
	queue.push(value);
});
var i, length = queue.length;
for(i = 0; i < length; i++){
	console.log(queue.shift());
}
```

### 栈

排队进电梯，最早进去的，被堵在里面了，反而最后出来。最后进去的，第一个出来。

```javascript
var people = ['a', 'b', 'c', 'd', 'e', 'f'];

var stack = [];
people.forEach(function(value, index){
	stack.push(value);
});

var i, length = stack.length;
for(i = 0; i < length; i++){
	console.log(stack.pop());
}
```

### 简单去重

```javascript
Array.prototype.unique = function(){
	var results = this.sort();/*排序*/
	for ( var i = 1; i < results.length; i++ ) {
		if ( results[i] === results[ i - 1 ] ) {
			results.splice( i--, 1 );/*删除相邻相同元素*/
		}
	}
	return results;
};
```

## 微趋势实战

### 前端数据黑名单

```javascript
// 产品列表黑名单
filter: function (array){
	// 定义黑名单
	var blackList = [
		{
			ProductUniqueName: "天天看",
			ProductUniqueId: "76ece34e6a1e4b65b9130b0f2a06305b"
		}
	];
	var blackListIndex = [];// 准备一个空数组，存放黑名单中产品在实际数据中的索引值

	array.productList.forEach(function(value, index){
		
		blackList.forEach(function(item){
			// 当黑名单中存在 
			if(value.ProductUniqueId === item.ProductUniqueId){
				blackListIndex.push(index);
			}
		});

	});

	blackListIndex.reverse();
	blackListIndex.forEach(function(value){
		array.productList.splice(value, 1);
	});

	return array;
}
```

### 前端更改产品排名

> 背景：微趋势的产品列表页面，部分产品数据不准确，需要人为降级处理。

```javascript
var productList = [/* 这里是后台返回的数据 */]
var degradeList = []; 
var degradeListIndex = []; // 准备两个空数组，一个放数据，一个放数据在原数组中的索引值

productList.forEach(function(value, index){
	value.IsDegrade = '';
	if(value.TotalCount < 500 && value.IsFavStickie !== 1 ){
		value.IsDegrade = 'degraded';
		// 复制被降级产品
		degradeList.push(value);
		degradeListIndex.push(index);
	}
});

// 数组索引倒序，避免删除元素时候，受数组长度影响
degradeListIndex = degradeListIndex.reverse();
degradeListIndex.forEach(function(value){
	// 从原数组中删除被降级产品
	productList.splice(value, 1);
});

productList = productList.concat(degradeList);
```