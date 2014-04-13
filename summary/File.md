# File API #

JavaScript操作文件，有三个重要的API

* window.File
* window.FileReader
* window.FileList
 
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

#### event.target.files 相关方法 ####

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

#### FileReader 相关方法 ####

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
