# 元素

`<option>`元素表示下拉框（`<select>`，`<optgroup>`或`<datalist>`）里面的一个选项。它是 HTMLOptionElement 接口的实例。

## 属性

除了继承 HTMLElement 接口的属性和方法，HTMLOptionElement 接口具有下面的属性。

- `disabled`：布尔值，表示该项是否可选择。
- `defaultSelected`：布尔值，表示该项是否默认选中。一旦设为`true`，该项的值就是`<select>`的默认值。
- `form`：返回`<option>`所在的表单元素。如果不属于任何表单，则返回`null`。该属性只读。
- `index`：整数，表示该选项在整个下拉列表里面的位置。该属性只读。
- `label`：字符串，表示对该选项的说明。如果该属性未设置，则返回该选项的文本内容。
- `selected`：布尔值，表示该选项是否选中。
- `text`：字符串，该选项的文本内容。
- `value`：字符串，该选项的值。表单提交时，上传的就是选中项的这个属性。