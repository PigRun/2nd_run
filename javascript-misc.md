# 杂项

## 可乐题

	console.log(3.0);		//结果是什么？
	
	var a = 1/3;
	console.log(a);
	console.log(a*3);	//结果是什么？
	
	console.log(0.1 + 0.2);	//结果是什么？

## 基本类型（原始类型）一些值得注意的方法

### String.prototype.charCodeAt

获取字符串指定位置的字符的ASCII码。

### String.fromCharCode

将ASCII解码为字符串

### String.prototype.indexOf String.prototype.lastIndexOf

获取字符串中某个子串所在的位置

### String.prototype.match String.prototype.replace RegExp.prototype.test

字符串匹配、替换、测试

### String.prototype.slice String.prototype.substr String.prototype.substring String.prototype.trim

字符串截取

### String.prototype.split Array.prototype.join

字符串拆分

### String.prototype.toLowerCase String.prototype.toUpperCase

转换大小写

### Number.prototype.toFixed

将数字转换为固定位数小数的字符串

### Number.prototype.ceil Number.prototype.floor Number.prototype.round

数字向上取整、向下取整、四舍五入取整

### Math.random

产生0到1的随机数 [0,1)

### parseInt

将字符串解析为数字
	
	parseInt('13');
	parseInt('1e4');
	parseInt('a0');
	parseInt('80',16);

## 数据转换

### 比较和运算时自动转换

- 字符串 -> 数字
		`'1'>0 //true`
		`'1'-0 //1`

- 布尔 -> 数字
		`true>0 //true`
		`true-0 //1`

### 手动转换

-  -> 数字： `'1' - 0;  //1` `true - 0;  //1`
-  -> 字符串: `3 + '';  //'3'` `true + '';  //'true'`
-  -> 布尔： `!!3; //true` `!!'0'; //true`

### 为falsy的值

- false
- null
- undefined
- ''
- 0
- NaN

### 相等传递性


## NaN

	typeof NaN
	NaN == NaN 
	isNaN(NaN)

## 逗号、逻辑短路

### 逗号运算符

	var a = (1-1,console.log(123),3);
	
### 逻辑短路

	var a = 1-1 && console.log(123) && 3;

	var a = 1-1 || console.log(123) || 3;


	
