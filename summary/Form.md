# JavaScript 表单

JavaScript诞生之处的一个重要的责任就是为服务器分担表单处理的任务。随着Web的发展，在处理Web表单这一块并没有太大的变化。

本文并不会讲述方方面面的关于表单和JavaScript的细节问题，旨在对这一主题做个系统的认识。为更好的理解这一主题，先来看看表单中常用的一些关乎JavaScript的属性。

相信大家都知道表单是什么：

```html
<!-- 简而言之，就是长这个样子 -->
<form action="submit.php" method="post">
	<input type="submit" value="提交">
	<input type="reset" value="重置">
</form>
```

然而，表单还有很多属性和方法：

- `acceptCharset`：服务器能处理的字符集；相当于`accept-charset`
- `action`：接受请求的URL
- `elements`：表单中的控件
- `enctype`：请求的编码类型
- `length`：表单中的控件数量
- `method`：发送HTTP请求的类型
- `name`：表单名称
- `target`：发送请求和接受响应的窗口
- `submit()`：提交
- `reset()`：重置

## 表单提交

默认情况下给表单设置一个`type`为`submit`的`input`或者`button`就会默认触发提交行为。然而对于实际开发中，通常我们都需要在提交之前干些什么，而每个表单都有一个与提交对应的`submit`事件，因此我们可以监控这个事件在提交之前干些想干的事：

```javascript
formName.addEventListener('submit', function(event) {
	// 既然想干点什么，就需要阻止默认提交的行为
	event.preventDefault();
	// 干点啥
	// ...
	// 然后换个提交方式提交
}, false);
// 如果不想干点什么，也可以直接提交
form.submit(); // 直接调用表单的`submit()`方法提交，但是注意：这种方式不会触发`submit`事件
```
## 表单重置

与提交类似，可以使用监听表单的`reset`事件来重置，也可以直接使用`reset()`方法重置。

不同的是表单的`reset()`方法会触发`reset`事件。

## 表单字段

可以借助表单的`elements`属性访问表单字段，也可以使用其他方式访问表单字段。这里就不赘述。

但是值得了解每个表单字段都有一些属性用以描述字段的属性和状态等，分别有：

- `disabled` - 值为`true`或者`false`， 表示该字段是否被禁用，也可以用变成的方式来设置它
- `form` - 当前字段所属表单，这是一个只读属性
- `name` - 字段名称
- `readOnly` - 表示当前字段是否只读的布尔值
- `tabIndex` - 当前字段使用tab切换时的序号
- `type` - 当前字段类型，如`text`,`password`,`checkbox`等
- `value` - 当前字段值。[出于安全性的考虑，`file`字段的值是只读的]

这些字段属性，可以使用变成的方式修改，从而可以利用这一特性更好的处理表单。

除了上述字段属性外，表单字段还有一些方法：

- `focus()` - 设置浏览器的焦点给调用这个方法的当前字段
- `blur()` - 从焦点字段移除焦点

利用这一特性，对于HTML5中为表单字段新增的autofocus属性便很好模拟来兼容低版本：

```javascript
document.addEventListener('DOMContentLoaded', function() {
	fieldElement.focus();
}, false);
```

对于文本字段，我们还可以使用`select()`方法选中文本字段的文本，在需要让用户一次性删除字段值的时候这个方法提供更便利的操作。

实际上表单字段还有选择文本范围的方法，但是相对而言比较复杂，在这里不赘述。

## 表单字段事件

除了上述的字段属性和方法，对于表单的处理，最常见还有监听字段的事件。在不考虑一些特定事件的情况下，每个表单字段都有以下事件：

- blur 市区焦点时触发的事件
- change 表单字段值改变时触发
- focus 表单字段获取交单时触发

监听表单字段的这些事件，可以帮助我们更好的处理诸如数据验证，数据序列化等任务。

对于表单，还有更多的操作处理，这一期主要是简单的介绍一下表单，便没有深入。

