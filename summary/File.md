# File API #

JavaScript操作文件，有三个重要的API

* window.FileList
* window.File
* window.FileReader
 
```javascript
if(window.File && window.FileReader && window.FIleList && window.Blob){
    // Code
}
```

## 获取文件 ##

获取文件的方式有两种，一种是通过表单提交的方式，另一种是通过拖拽事件获得被拖拽的文件。
注意，所有获取文件的方式，都必须由用户主动触发，JavaScript 无法自动获取文件。

### 通过表单获取文件 ###

表单形式，就是使用```<input type="file">```标签，通过表单事件的```event.target.files```对象获取文件

#### event.target.files[] 详解 ####

event.target.files[]数组里面的每一个元素，就是一个window.File对象。

* 属性
  * File.name 当前File对象所引用文件的文件名
  * File.lastModifiedDate 当前File对象所引用文件最后修改时间
  * File.size 当前File对象所引用文件的文件大小，单位为字节
  * File.type 当前File对象所引用文件的文件类型(MIME类型)
* 方法
  * File.getAsBinary() 返回一个字符串，包含了文件内容的原始二进制形式
  * File.getAsDataURL() 返回一个字符串，包含了将所引用文件的文件内容进行编码后的data: URL
  * File.getAsText(string encoding) 返回一个字符串，其内容为根据参数encoding指定的编码读取文件内容形成的文本

#### 表单演示 ####

```html
<!-- 准备一个表单用来读取文件 -->
<input type="file" id="files" name="files[]" multiple />
<div id="list"></div>

<script>
  function handleFileSelect(evt) {
    var files = evt.target.files; // FileList object

    // files is a FileList of File objects. List some properties.
    var output = [];
    for (var i = 0, f; f = files[i]; i++) {
      var str = "'<li><strong>', escape(f.name), " +
                "'</strong> (', f.type || 'n/a', ') - '," +
                "f.size, ' bytes, last modified: '," +
                "f.lastModifiedDate.toLocaleDateString(), '</li>'";

      output.push(str);

    }
    document.getElementById('list').innerHTML = '<ul>' + output.join('') + '</ul>';
  }
    // 监听表单事件
  document.getElementById('files').addEventListener('change', handleFileSelect, false);
</script>
```

### 通过拖拽获取文件 ###

监听dom标签上面的拖拽事件，通过事件的```event.dataTransfer.files```对象获取文件

#### event.dataTransfer 相关方法 ####

#### 拖拽演示 ####

```html
<div id="drop_zone">Drop files here</div>
<div id="list"></div>

<script>
  function handleFileSelect(evt) {
    evt.stopPropagation();
    evt.preventDefault();

    var files = evt.dataTransfer.files; // FileList object.

    // files is a FileList of File objects. List some properties.
    var output = [];
    for (var i = 0, f; f = files[i]; i++) {
      var str = + "'<li><strong>', escape(f.name), "
                + "'</strong> (', f.type || 'n/a', ') - ',"
                + "f.size, ' bytes, last modified: ',"
                + "f.lastModifiedDate.toLocaleDateString(), '</li>'";

      output.push(str);
    }
    document.getElementById('list').innerHTML = '<ul>' + output.join('') + '</ul>';
  }

  function handleDragOver(evt) {
    evt.stopPropagation();
    evt.preventDefault();
    evt.dataTransfer.dropEffect = 'copy'; // Explicitly show this is a copy.
  }

  // Setup the dnd listeners.
  var dropZone = document.getElementById('drop_zone');
  // 监听拖拽事件
  dropZone.addEventListener('dragover', handleDragOver, false);
  dropZone.addEventListener('drop', handleFileSelect, false);
</script>
```

## 读取文件内容 ##

### 通过FileReader读取文件 ###

当通过上述表单或者拖拽方式，获取文件之后，通过实例化FileReader对象，使用```reader.readAsDataURL(file)```方法来读取文件内容。
当文件读取成功后，将触发读取程序的onload事件，而其result属性可用于访问文件数据。

#### FileReader 详解 ####

* 属性
  * FileReader.error 在读取文件时发生的错误
  * FileReader.readyState 表明FileReader对象的当前状态. 值为State constants中的一个
  * FileReader.result 读取到的文件内容。这个属性只在读取操作完成之后才有效，并且数据的格式取决于读取操作是由哪个方法发起的
* 方法
  * FileReader.abort() 中止该读取操作
  * FileReader.readAsArrayBuffer() 读取指定的Blob对象或File对象中的内容。result属性中将包含一个ArrayBuffer对象以表示所读取文件的内容
  * FileReader.readAsBinaryString() 读取指定的Blob对象或File对象中的内容。同时,result属性中将包含所读取文件的原始二进制数据
  * FileReader.readAsDataURL() 读取指定的Blob对象或File对象中的内容。result属性中将包含一个data: URL格式的字符串以表示所读取文件的内容.
  * FileReader.readAsText() 读取指定的Blob对象或File对象中的内容。result属性中将包含一个字符串以表示所读取的文件内容
* State constants
  * EMPTY 还没有加载任何数据
  * LOADING 数据正在被加载
  * DONE 已完成全部的读取请求
* Event handlers
  * onabort 当读取操作被终止时调用
  * onerror 当读取操作发生错误时调用
  * onload 当读取操作成功完成时调用
  * onloadend 当读取操作完成时调用，不管是成功还是失败。该处理程序在onload或者onerror之后调用
  * onloadstart 当读取操作将要开始之前调用
  * onprogress 在读取数据过程中周期性调用

#### FileReader演示####

```html
<input type="file" id="files" name="files[]" multiple />
<div id="list"></div>

<script>
  function handleFileSelect(evt) {
    var files = evt.target.files; // FileList object

    // Loop through the FileList and render image files as thumbnails.
    for (var i = 0, f; f = files[i]; i++) {

      // Only process image files.
      if (!f.type.match('image.*')) {
        continue;
      }

      var reader = new FileReader();

      // Closure to capture the file information.
      reader.onload = (function(theFile) {
        return function(e) {
          // Render thumbnail.
          var span = document.createElement('span');
          span.innerHTML = ['<img class="thumb" src="', e.target.result,
                            '" title="', escape(theFile.name), '"/>'].join('');
          document.getElementById('list').insertBefore(span, null);
        };
      })(f);

      // Read in the image file as a data URL.
      reader.readAsDataURL(f);
    }
  }

  document.getElementById('files').addEventListener('change', handleFileSelect, false);
</script>
```
