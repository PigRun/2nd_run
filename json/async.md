#1. 异步简介

 ##1.1 什么是线程

  ![线程](https://github.com/PigRun/2nd_run/blob/master/json/img/bg2012122102.png?raw=true)

 ##1.2 异步用来解决什么问题?

 ##1.3 异步带来的新的难点

 ##1.4 如何解决?


#2. 执行异步的方法

 ##2.1 回调函数

 假设我们有一个f1函数,一个f2函数,f2函数等待f1函数执行的结果.

 ```
     function f1(callback){
 　　　　setTimeout(function () {
 　　　　　　// f1的任务代码
 　　　　　　callback();
 　　　　}, 1000);
 　　}
  f1(f2);
 ```

 回调函数
 
 优点是简单、容易理解和部署
    
 缺点是不利于代码的阅读和维护，各个部分之间高度耦合（Coupling），流程会很混乱且每个任务只能指定一个回调函数。
 而且不利于调试.callback抛出的错误并不能被当场trycatch到

 最重要的是,他很难看.

 ##2.2 事件监听

 采用事件驱动模式。任务的执行不取决于代码的顺序，而取决于某个事件是否发生。(采用jQuery写法)
```
 //给f1绑定一个done标记
 f1.on('done', f2);

 function f1(){
　　　setTimeout(function () {
　　　　　// f1的任务代码
　　　　　f1.trigger('done');
　　　}, 1000);
　　}
```
 在f1执行后会触发done标记,done标记会让f2开始运行.

 优点是每个函数能绑定多个回调,一个标记对一个回调.
 
 缺点是整个程序都要变成事件驱动型，运行流程会变得很不清晰。
```
$('#tabby, #socks').on('meow', function() { 
console.log(this.id + ' meowed'); 
}); 
$('#tabby').trigger('meow'); // "tabby meowed" 
$('#socks').trigger('meow'); // "socks meowed" 
```
 ##2.3 发布/订阅

 "发布/订阅模式" 又叫 "观察者模式";
 这里在事件监听的基础上多了一个"信号中心"的概念
```
 jQuery.subscribe("done", f2);
 function f1(){
 　　　　setTimeout(function () {
 　　　　　　// f1的任务代码
 　　　　　　jQuery.publish("done");
 　　　　}, 1000);
 　　}

 //在f2执行完毕后
 jQuery.unsubscribe("done", f2);
```

 这里的publish不是publish到本身的一个标记,而是publish到"信息中心"的一个标记,然后信息中心去指挥订阅这个事件的函数开始执行

 改良的点在于 因为我们可以通过查看"消息中心"，了解存在多少信号、每个信号有多少订阅者，从而监控程序的运行。

 ##2.4 Promises对象

 [Promises对象](http://wiki.commonjs.org/wiki/Promises/A)是CommonJS工作组提出的一种规范，目的是为异步编程提供统一接口。

 思想是每一个需要异步的函数,返回的是一个Promise对象，该对象有一个then方法，允许指定回调函数。

 这里还是借用jQuery中已经实现了的方法
```
 f1().then(f2);
```
```
 function f1(){
　　　　var dfd = $.Deferred();
　　　　setTimeout(function () {
　　　　　　// f1的任务代码
　　　　　　dfd.resolve();
　　　　}, 500);
 //我们需要返回一个promise对象
　　　　return dfd.promise;
　　}
```
 先说好处: 我们可以和jQuery一样链式调用, 这样流程就很清楚:
```
f1().then(f2).then(f3);
```

 如果我们需要指定一个失败的函数,我们可以这样写:
```
f1().then(f2).fail(f3);
```

 而且，它还有一个前面三种方法都没有的好处：如果一个任务已经完成，再添加回调函数，该回调函数会立即执行。所以，你不用担心是否错过了某个事件或信号。
 这种方法的缺点就是编写和理解，都相对比较难。

 #3.具体一点介绍promise?

 ##3.1 首先看怎么实现一个promise
```
 var promptDeferred = new $.Deferred(); 
 promptDeferred.always(function(){ console.log('A choice was made:'); }); 
 promptDeferred.done(function(){ console.log('Starting game...'); }); 
 promptDeferred.fail(function(){ console.log('No game today.'); });
```
```
 var prompt = new $.Deferred();
 $('#playGame').focus().on('keypress', function(e) {
   var Y = 121, N = 110;
   if (e.keyCode === Y) {
     prompt.resolve(); 
   } else if (e.keyCode === N) {
     prompt.reject();
   } else {
     return false;  // they must choose!
   }
 });
 prompt.done(function(){ console.log('Starting game...'); });
 prompt.fail(function(){ console.log('No game today.'); });
```
 ##3.2 jQuery中的Promise
 
 1.5版本以前的jQuery用的回调函数功能很弱,由此有了deerred对象

 等等等等...不是在说Promise吗? 怎么扯到deferred对象了? 看下面的图

 ![deferred promise](https://github.com/PigRun/2nd_run/blob/master/json/img/dp.png?raw=true)

 这就是deferred和promise的关系

 下面是我们传统的jQuery ajax的写法
```
 $.ajax({
　　　　url: "test.html",
　　　　success: function(){
　　　　　　alert("哈哈，成功了！");
　　　　},
　　　　error:function(){
　　　　　　alert("出错啦！");
　　　　}
　　});
```
现在新的写法是这样子的:
```
 $.ajax("test.html")
　　.done(function(){ alert("哈哈，成功了！"); })
　　.fail(function(){ alert("出错啦！"); });
```
好处:

 1. 变帅了
 2. 变帅了
 3. 允许你自由添加多个回调函数。
```
 /*多个回调*/
 $.ajax("test.html")
　　.done(function(){ alert("哈哈，成功了！"); })
　　.fail(function(){ alert("出错啦！"); });
 .done(function(){ alert("哈哈，成功了！"); })
```
 $.when函数的用法:
```
 $.when($.ajax("test1.html"), $.ajax("test2.html"))
　　.done(function(){ alert("哈哈，成功了！"); })
　　.fail(function(){ alert("出错啦！"); });
```

 ##3.3 如何把一个函数改造成promise/deffered函数
```
   var getLocation = function( callback ){

       navigator.geolocation.getCurrentPosition( callback || function( position ){

           // Stuff with geolocation

       });

   };
```
改造成promise/deffered之后:
```
 var getLocation = function() {
       var deferred = new $.Deferred();

       navigator.geolocation.getCurrentPosition(function( position ){
           // geolocation 要做的事情!!

           //把deferred的状态设置为完成
           deferred.resolve(position);
       });

       // 返回promise对象,这样这个函数的reject和fail回调就不会执行了
       return deferred.promise();
   };
 getLocation().then(drawMarkerOnMap);
```

 ##3.4 promise/A规范
 deferred对象遵从promise/A的规范:

 三种执行状态----未完成(等待 -> progress)，已完成(resolved -> done)和已失败(fail)

 jQuery的deferred对象就是把回调函数接口拓展到所有操作的对象,不仅ajax,也不管是异步函数还是同步函数,都可以使用这个对象的方法来指定回调函数
```
 /*假设有一个很耗时的操作*/
 var wait = function(){
  var tasks = function(){
   alert("执行完毕!");
  }
  setTimeout(tasks,5000);
 }
```

 我们希望给他指定回调函数,那么就应该是这样写:
```
 $.when(wait())
　　.done(function(){ alert("哈哈，成功了！"); })
　　.fail(function(){ alert("出错啦！"); });
```
 但是这段代码是不对的,因为wait返回的不是deferred对象,所以jQuery的done方法会立即执行,所以要改写wait:
```
 var dtd = $.Deferred(); // 新建一个deferred对象
　　var wait = function(dtd){
　　　　var tasks = function(){
　　　　　　alert("执行完毕！");
　　　　　　dtd.resolve(); // 改变deferred对象的执行状态
　　　　};
　　　　setTimeout(tasks,5000);
　　　　return dtd;
　　};

 $.when(wait(dtd))
　　.done(function(){ alert("哈哈，成功了！"); })
　　.fail(function(){ alert("出错啦！"); });
```

 但是像上面这样子写, 只要我在代码最后加上一行 `dtd.resolve()` 整个状态就改变了,不会再等wait函数执行完 deferred函数就执行了.

 所以jQuery提供了deferred.promise() 如果我们在函数里面用这个方法返回了另一个deferred对象,这个对象只开放与执行状态无关的比如done和fail方法,而像resolve方法和reject方法这种会改变运行状态的,就没有开放.
```
 var wait = function(dtd){
 var dtd = $.Deferred();// 新建一个Deferred对象
  var tasks = function(){
   alert("执行完毕！");
   dtd.resolve(); // 改变Deferred对象的执行状态
  };

  setTimeout(tasks,5000);
  return dtd.promise(); // 返回promise对象
 };
  $.when(wait())
  .done(function(){ alert("哈哈，成功了！"); })
  .fail(function(){ alert("出错啦！"); });
  d.resolve(); // 此时，这个语句是无效的
```
 jQuery会默认把新的deferred对象最为参数传入when的参数中,而此时when的参数应该是一个函数名,所以上面的程序还可以这样写:
```
　　$.when(wait)
　　.done(function(){ alert("哈哈，成功了！"); })
　　.fail(function(){ alert("出错啦！"); });
　　d.resolve(); // 此时，这个语句是无效的
```
`then = fail + done;`
```
 $.when(wait)
　　.then(function(){ alert("哈哈，成功了！"); },function(){ alert("出错啦！"); });
　　.then(function(){ alert("哈哈，成功了！"); });
```
`always = fail || done;`
 
 ##3.5 补充

 ###3.5.1 jQuery与promise/A

 promise中规定成功为fulfill,而jQuery实现出来是resolve.

 在jQuery1.8- , then方法只是done,fail,progress三种回调的缩写,而Promise/A中的then行为是把后续回调存储如队列.

 还有其他一些实现上的细微差别.

 因此,如果我们在开发时,使用了jQuery来生成我们的promise对象,那么最好从头到尾都只用jQuery.
 如果使用了其他遵循Promise/A规范的类库来得到CommonJS Promise对象,那么也最好从一而终.
 两种方式不能混用.
 除非你用Q.js(Q.js中有处理好Promise/A规范生成的Promise对象与jQuery生成的Promise对象不同的兼容)

 ###3.5.2 jQuery中多次调用同一个对象返回的promise其实是同一个
```
 var promise1 = promptDeferred.promise();
 var promise2 = promptDeferred.promise();
 console.log(promise1 === promise2); //true
```

 ###3.5.3 异步中还有其他诸如wind.js的非主流方法,待研究




