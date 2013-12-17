
# File API

File API的三个重要方法 *window.File*, *window.FileReader*, *window.FileList*
```
if(window.File && window.FileReader && window.FIleList && window.Blob){
    // Code
}
```
## 获取文件
### 通过表单，获取本地文件

要加载文件，最直接的方法就是使用标准```<input type="file">```元素。JavaScript 会返回选定的 File 对象的列表作为 FileList。
```
<!-- 准备一个表单用来读取文件 -->
<input type="file" id="files" name="files[]" multiple />
<output id="list"></output>

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

**例子：**使用表单进行读入文件。

<input type="file" id="files1" name="files[]" multiple />
<output id="list"></output>

<script>
  function handleFileSelect(evt) {
    var files = evt.target.files; // FileList object

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
    // 监听表单事件
  document.getElementById('files1').addEventListener('change', handleFileSelect, false);
</script>

### 使用拖拽方式获取文件

拖拽事件

* ondrag 当拖动元素时运行脚本
* ondragend 当拖动操作结束时运行脚本
* ondragenter 当元素被拖动至有效的拖放目标时运行脚本
* ondragleave 当元素离开有效拖放目标时运行脚本
* ondragover 当元素被拖动至有效拖放目标上方时运行脚本
* ondragstart 当拖动操作开始时运行脚本
* ondrop 当被拖动元素正在被拖放时运行脚本

拖拽事件的dataTransfer接口

* dataTransfer.dropEffect [ = value ]：返回已选择的拖放效果，如果该操作效果与起初设置的effectAllowed效果不符，则拖拽操作失败。可以设置修改，包含这几个值：“none”, “copy”, “link” 和 “move”。
* dataTransfer.effectAllowed [ = value ]：返回允许执行的拖拽操作效果，可以设置修改，包含这些值：“none”, “copy”, “copyLink”, “copyMove”, “link”, “linkMove”, “move”, “all” 和 “uninitialized”
* dataTransfer.types：返回在dragstart事件出发时为元素存储数据的格式，如果是外部文件的拖拽，则返回”files”
* dataTransfer.clearData ( [ format ] )：删除指定格式的数据，如果未指定格式，则删除当前元素的所有携带数据
* dataTransfer.setData(format, data)：为元素添加指定数据
* dataTransfer.getData(format)：返回指定数据，如果数据不存在，则返回空字符串
* dataTransfer.files：如果是拖拽文件，则返回正在拖拽的文件列表FileList
* dataTransfer.setDragImage(element, x, y)：制定拖拽元素时跟随鼠标移动的图片，x、y分别是相对于鼠标的坐标(据测试，Chrome暂不支持)
* dataTransfer.addElement(element)：添加一起跟随拖拽的元素，如果你想让某个元素跟随被拖拽元素一同被拖拽，则使用此方法(据测试，Chrome暂不支持)

```
<div id="drop_zone">Drop files here</div>
<output id="list"></output>

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

<div id="drop_zone" style="border: 5px dotted #CCC;padding: 5px;">Drop files here</div>
<output id="list"></output>

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

# 读取文件

当您获取了 File 引用后，实例化 FileReader 对象，以便将其内容读取到内存中。加载结束后，将触发读取程序的 onload 事件，而其 result 属性可用于访问文件数据。

FileReader 包括四个异步读取文件的选项：

* FileReader.readAsBinaryString(Blob|File) - result 属性将包含二进制字符串形式的 file/blob 数据。每个字节均由一个 [0..255] 范围内的整数表示。
* FileReader.readAsText(Blob|File, opt_encoding) - result 属性将包含文本字符串形式的 file/blob 数据。该字符串在默认情况下采用“UTF-8”编码。使用可选编码参数可指定其他格式。
* FileReader.readAsDataURL(Blob|File) - result 属性将包含编码为数据网址的 file/blob 数据。
* FileReader.readAsArrayBuffer(Blob|File) - result 属性将包含 ArrayBuffer 对象形式的 file/blob 数据。
对您的 FileReader 对象调用其中某一种读取方法后，可使用 onloadstart、onprogress、onload、onabort、onerror 和 onloadend 跟踪其进度。

## 读取本地图片，生成缩略图
```
<style>
  .thumb {
    height: 75px;
    border: 1px solid #000;
    margin: 10px 5px 0 0;
  }
</style>

<input type="file" id="files" name="files[]" multiple />
<output id="list"></output>

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

<style>
  .thumb {
    height: 75px;
    border: 1px solid #000;
    margin: 10px 5px 0 0;
  }
</style>

<input type="file" id="files2" name="files[]" multiple />
<output id="list"></output>

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

  document.getElementById('files2').addEventListener('change', handleFileSelect, false);
</script>