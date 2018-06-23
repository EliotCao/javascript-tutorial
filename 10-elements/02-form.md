# 元素

`<form>`元素代表了表单，继承了 HTMLFormElement 接口。

## HTMLFormElement 的实例属性

- `elements`：返回一个类似数组的对象，成员是属于该表单的所有控件元素。该属性只读。
- `length`：返回一个整数，表示属于该表单的控件数量。该属性只读。
- `name`：字符串，表示该表单的名称。
- `method`：字符串，表示提交给服务器时所使用的 HTTP 方法。
- `target`：字符串，表示表单提交后，服务器返回的数据的展示位置。
- `action`：字符串，表示表单提交数据的 URL。
- `enctype`（或`encoding`）：字符串，表示表单提交数据的编码方法，可能的值有`application/x-www-form-urlencoded`、`multipart/form-data`和`text/plain`。
- `acceptCharset`：字符串，表示服务器所能接受的字符编码，多个编码格式之间使用逗号或空格分隔。
- `autocomplete`：字符串`on`或`off`，表示浏览器是否要对`<input>`控件提供自动补全。
- `noValidate`：布尔值，表示是否关闭表单的自动校验。