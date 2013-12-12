# JavaScript Array

数组是一段线性分配的内存，也就是说，它的数据在内存中，都是挨着在的。所以，数组是一种性能出色的数据结构（排序、求值等）。

But，JavaScript中的数组，却并不完全是这样。

JavaScript中的数组，本质上还是一个对象，只不过多了一些数组特有的属性，例如length。所以，它比真正的数组慢，但是使用起来更方便。
它的属性检索、更新方式和对象一模一样```array['name']```，只不过多了一个可以用整数作为属性名的特性```array[1]```。

## 创建一个数组

字面量方式创建：
```
var array = ['bobbli', 'jsonzhang', 'robyi', 'solodu', 'v_pfhe', 'v_ppingzhou'];
```
由```Array()```构造函数生成:
```
var array = new Array('bobbli', 'jsonzhang', 'robyi', 'solodu', 'v_pfhe', 'v_ppingzhou');
```

## 获取数组长度

每个数组都有一个length属性。JavaScript的length属性是没有上限，并且会自动增加。
```
var people = ['bobbli', 'jsonzhang', 'robyi', 'solodu', 'v_pfhe', 'v_ppingzhou'];
console.log(people.length);

people[10] = 'tealin';
console.log(people.length);
```

length属性的值是这个数组的最大整数属性名加上1，它不一定等于数里的属性个数。

length属性也能用来裁剪数组。
```
people.length = 3;
console.log(people);
```

## 删除数组

数组也是对象，所以可以用```delete```运算符来移除数组中的元素。
```
delete people[2];
console.log(people);
```
使用```delete```方式，会在数组中留下空洞，但是```splice```方法，就会自动将后面剩余的元素前移。
```
people.splice(2,1);
console.log(people);
```

## 遍历数组

### for in 遍历数组
```
var people = ['bobbli', 'jsonzhang', 'robyi', 'solodu', 'v_pfhe', 'v_ppingzhou'];

for (key in people) {
    console.log(person[key]);
}
```
数组也是对象，可以用```for in```来遍历。

**但是**，```for in```会从原型链中获得其他属性，不建议使用。

### for 遍历数组
```
var people = ['bobbli', 'jsonzhang', 'robyi', 'solodu', 'v_pfhe', 'v_ppingzhou'];
var i;
for (i = 0; i < people.length; i ++) {
    console.log(people[i]);
}
```
### ES5 中的 forEach 遍历数组
```
var people = ['bobbli', 'jsonzhang', 'robyi', 'solodu', 'v_pfhe', 'v_ppingzhou'];
people.forEach(function(value, index){
        console.log(value);
});
```

## 应用

### 队列

排队吃饭，先到先得
```
var people = ['bobbli', 'jsonzhang', 'robyi', 'solodu', 'v_pfhe', 'v_ppingzhou'];

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
```
var people = ['bobbli', 'jsonzhang', 'robyi', 'solodu', 'v_pfhe', 'v_ppingzhou'];

var stack = [];
people.forEach(function(value, index){
    stack.push(value);
});

var i, length = stack.length;
for(i = 0; i < length; i++){
    console.log(stack.pop());
}
```
### 排序
JavaScript自带的```array.sort(comparefn)```方法比较坑，它并不会对数字进行排序，而是全部以字符串的形式进行排序。
```
var arr = [4, 8, 15, 16, 23, 42];
console.log(arr.sort()); //[15, 16, 23, 4, 42, 8] 
```
想要对数字进行排序，还需自己传入一个排序函数。
```
var arr = [4, 8, 15, 16, 23, 42];
function sortNumber(a, b){
    return a - b;
}
console.log(arr.sort(sortNumber));
```

### 简单去重
```
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
```
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

```
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
