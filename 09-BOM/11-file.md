# File 对象，FileList 对象，FileReader 对象

## File 对象

File 对象代表一个文件，用来读写文件信息。它继承了 Blob 对象，或者说是一种特殊的 Blob 对象，所有可以使用 Blob 对象的场合都可以使用它。

最常见的使用场合是表单的文件上传控件（`<input type="file">`），用户选中文件以后，浏览器就会生成一个数组，里面是每一个用户选中的文件，它们都是 File 实例对象。

```
// HTML 代码如下
// <input id="fileItem" type="file">
var file = document.getElementById('fileItem').files[0];
file instanceof File // true
```

上面代码中，`file`是用户选中的第一个文件，它是 File 的实例。

### 构造函数

浏览器原生提供一个`File()`构造函数，用来生成 File 实例对象。

```
new File(array, name [, options])
```

`File()`构造函数接受三个参数。

- array：一个数组，成员可以是二进制对象或字符串，表示文件的内容。
- name：字符串，表示文件名或文件路径。
- options：配置对象，设置实例的属性。该参数可选。

第三个参数配置对象，可以设置两个属性。

- type：字符串，表示实例对象的 MIME 类型，默认值为空字符串。
- lastModified：时间戳，表示上次修改的时间，默认为`Date.now()`。

下面是一个例子。

```
var file = new File(
  ['foo'],
  'foo.txt',
  {
    type: 'text/plain',
  }
);
```