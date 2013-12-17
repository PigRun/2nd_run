# 杂项

## 基本类型（原始类型）一些值得注意的方法

### String.prototype.charCodeAt

### String.fromCharCode

### String.prototype.indexOf String.prototype.lastIndexOf

### String.prototype.match String.prototype.replace RegExp.prototype.test

### String.prototype.slice String.prototype.substr String.prototype.substring

### String.prototype.split Array.prototype.join

### String.prototype.toLowerCase String.prototype.toUpperCase

### Number.prototype.toFixed

### Number.prototype.ceil Number.prototype.floor Number.prototype.round

### Math.random

### parseInt

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

## 逗号、逻辑短路

### 逗号运算符

	var a = (1-1,console.log(123),3);
	
### 逻辑短路

	var a = 1-1 && console.log(123) && 3;

	var a = 1-1 || console.log(123) || 3;


	
