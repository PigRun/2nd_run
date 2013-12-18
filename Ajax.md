# AJAX

Ajax 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。

## 什么是 Ajax ？

Ajax = 异步 JavaScript 和 XML。

Ajax 是一种用于创建快速动态网页的技术。

为了帮助您理解 AJAX 的工作原理，我们创建了一个小型的 AJAX 应用程序。

```
<html>
<body>

<div id="myDiv"><h3>Let AJAX change this text</h3></div>
<button type="button" onclick="loadXMLDoc()">Change Content</button>

</body>
</html>

<script type="text/javascript">
function loadXMLDoc()
{
.... AJAX script goes here ...
}
</script>
</head>
```
## XMLHttpRequest 对象

创建 XMLHttpRequest 对象的语法：
```
variable=new XMLHttpRequest();
```
XMLHttpRequest 对象用于和服务器交换数据。

向服务器发送请求

如需将请求发送到服务器，我们使用 XMLHttpRequest 对象的 open() 和 send() 方法：
```
xmlhttp.open("GET","test1.txt",true);
xmlhttp.send();
```
### GET 还是 POST？

与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。

然而，在以下情况中，请使用 POST 请求：

1. 无法使用缓存文件（更新服务器上的文件或数据库）
2. 向服务器发送大量数据（POST 没有数据量限制）
3. 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

### GET 请求

一个简单的 GET 请求：
```
xmlhttp.open("GET","demo_get.asp",true);
xmlhttp.send();
```
### POST 请求

一个简单的 POST 请求：
```
xmlhttp.open("POST","demo_post.asp",true);
xmlhttp.send();
```
### url - 服务器上的文件

open() 方法的 url 参数是服务器上文件的地址：
```
xmlhttp.open("GET","ajax_test.asp",true);
```
该文件可以是任何类型的文件，比如 .txt 和 .xml，或者服务器脚本文件，比如 .asp 和 .php （在传回响应之前，能够在服务器上执行任务）。

### 异步 - True 或 False？

AJAX 指的是异步 JavaScript 和 XML（Asynchronous JavaScript and XML）。
XMLHttpRequest 对象如果要用于 AJAX 的话，其 open() 方法的 async 参数必须设置为 true：
```
xmlhttp.open("GET","ajax_test.asp",true);
```
通过 AJAX，JavaScript 无需等待服务器的响应，而是：

1. 在等待服务器响应时执行其他脚本
2. 当响应就绪后对响应进行处理

### Async = true

当使用 async=true 时，请规定在响应处于 onreadystatechange 事件中的就绪状态时执行的函数：
```
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
  }
xmlhttp.open("GET","test1.txt",true);
xmlhttp.send();
```
### Async = false

如需使用 async=false，请将 open() 方法中的第三个参数改为 false：
```
xmlhttp.open("GET","test1.txt",false);
```
不推荐使用 async=false，但是对于一些小型的请求，也是可以的。

## AJAX - 服务器响应

1. responseText 获得字符串形式的响应数据。
2. responseXML  获得 XML 形式的响应数据。

### responseText 属性
```
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```
### responseXML 属性
```
xmlDoc=xmlhttp.responseXML;
txt="";
x=xmlDoc.getElementsByTagName("ARTIST");
for (i=0;i<x.length;i++)
  {
  txt=txt + x[i].childNodes[0].nodeValue + "<br />";
  }
document.getElementById("myDiv").innerHTML=txt;
```
## AJAX - onreadystatechange 事件

当 readyState 等于 4 且状态为 200 时，表示响应已就绪：
```
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
  }
```
onreadystatechange 事件被触发 5 次（0 - 4），对应着 readyState 的每个变化。

### 使用 Callback 函数

callback 函数是一种以参数形式传递给另一个函数的函数。

## AJAX 数据库实例

AJAX 可用来与数据库进行动态通信。

http://echo.113.im/?data={%22test%22:1}

http://echo.113.im/?data={%22test%22:1}&timeout=3000
