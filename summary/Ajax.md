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
