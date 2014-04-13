# Ajax

一种向服务器发送请求而无需重载页面的技术，关于这个主题的历史以及它有多火就不赘述了。

Ajax全称Asynchronous JavaScript + XML，但这里的XML只是一种通信的数据格式，实际上并不一定是XML数据。

## XHR介绍

Ajax技术的核心在于XMLHttpRequest对象（简称XHR），使用XHR可以以异步的方式获取数据，然后更新到页面中而无需刷新页面。

在标准的浏览器中有一个XMLHttpRequest对象，但是由于早期这一技术概念是有微软引入的，而在微软的IE浏览器中是使用ActiveX对象实现的，而且还有多重不同的版本，常见的有MSXML.XMLHttp, MSXML2.XMLHttp.3.0和MSXML2.XMLHttp.6.0。

对于基于XHR的Ajax技术，首先我们需要创建一个兼容性的代码来正确的获取到核心的XHR对象以处理不同浏览器的兼容性：

```javascript
function getXHR() {
	if(typeof XMLHttpRequest !== 'undefined') {
		getXHR = function() {
			return new XMLHttpRequest();
		}
	} else if(typeof ActiveXObject !== 'undefined') {
		getXHR = function() {
			return new ActiveXObject('MSXML2.XMLHttp');
		}
	} else {
		throw new Error('浏览器太low，换个吧');
	}
	return getXHR();
}
// 创建xhr
var xhr = getXHR();
```

## 使用XHR

首先要用XHR向服务器发送请求，我们得使用XHR对象的`open()`方法启动请求：

```javascript
xhr.open('get', 'userinfo.php', false);
```

`open()`方法接受三个参数： 

- 请求类型，可选的有`get`,`post`等标准的请求类型
- 请求的url
- 表示是否异步的一个布尔值，`true`表示异步，`false`表示同步

此时，并不会发起真正的请求，要发起真正的请求还需要借助xhr对象的`send()`方法：

```javascript
xhr.send(null);
```

`send()`方法接受一个参数，即发送给服务器的数据。对于无需发送数据，使用`null`作为参数即可。调用`send()`方法之后，这个请求才会真正的发送到服务器。

## 处理XHR请求响应

默认情况下，服务器在接受到请求之后会返回响应数据，而发起请求的XHR对象也有响应的属性来接受这些数据，以及反馈响应状态。以下是相关属性：

- responseText 返回的文本数据
- responseXML 返回的XML文档，这取决于响应的内容类型
- status 响应的HTTP状态
- statusText HTTP状态说明

为了正确的验证数据的可靠性，通常我们需要先检查status属性的值已确定响应是否成功返回。通常响应代码为200表示成功，因此可以基于这一特性来正确处理响应数据：

```javascript
if(xhr.status >= 200 && xhr.status < 300 || xhrs.status == 304) {
	console.log(xhr.responseText); // 成功
} else {
	console.log('error: ' + xhr.status); // 失败
}
```

然而对于同步请求，上面的代码没什么问题。通常情况下我们都是使用Ajax处理异步请求，这样不必阻塞页面的渲染。而为了进一步验证异步请求的可靠性，我们还可以检测XHR对象的`readyState`属性来检查异步请求的状态。

这个`readyState`有几个值，分别表示不同的状态：

- 0 还没调用`open()`方法
- 1 启动请求了，还没调用`send()`方法
- 2 发送请求了，还没收到响应
- 3 接受到响应数据，但还没全部收到
- 4 响应数据全部收到，并且可用了

而这个`readyState`属性的变化会触发`readystatechange`事件，最后我们可以利用这个事件来检查`readyState`的值。为了异步请求垮浏览器的可靠性，通常都在启动请求之间就开始检测：

```javascript
xhr.onreadystatechange = function() {
	if(xhr.readyState == 4) { // 通常都只处理ok的情况
		if(xhr.status >= 200 && xhr.status < 300 || xhr.status == 304) {
			// 数据ok
		} else {
			// 响应失败
		}
	}
}
// 发请求
xhr.open('get', 'getInfo.php', true);
xhr.send(null);
```

掌握这些技巧便可以简单的处理日常的Ajax请求。

然而默认情况下，发起Ajax请求时，浏览器默认发送一些信息给服务器，这里不赘述。但是，有时候我们可能希望设置自定义的请求头信息。

对于这种情况，可以使用XHR对象提供的`setRequestHeader()`方法来设置自定义的请求头信息。

这个方法接受两个参数：

- 请求头字段
- 请求头字段的值

为了正确的发送请求头信息，还必须在`open()`之后`send()`之前调用这个方法：

```javascript
xhr.open('get', 'info.php', true);
xhr.setRequestHeader('userName', 'basecss');
xhr.send(null);
```

能这是请求头，当然也可以提取请求头，有两个方法可以提取这些信息：

- `getResponseHeader()`，接受一个参数，获取参数指定的请求头
- `getAllResponseHeaders()，获取所有请求头信息

至此Ajax相关的介绍就完了。相信通过对Ajax的了解，在日常开发中处理这类问题时会更加轻松啦。

