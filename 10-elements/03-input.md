# 元素

`<input>`元素主要用于表单组件，它继承了 HTMLInputElement 接口。

## HTMLInputElement 的实例属性

### 特征属性

- `name`：字符串，表示`<input>`节点的名称。该属性可读写。
- `type`：字符串，表示`<input>`节点的类型。该属性可读写。
- `disabled`：布尔值，表示`<input>`节点是否禁止使用。一旦被禁止使用，表单提交时不会包含该`<input>`节点。该属性可读写。
- `autofocus`：布尔值，表示页面加载时，该元素是否会自动获得焦点。该属性可读写。
- `required`：布尔值，表示表单提交时，该`<input>`元素是否必填。该属性可读写。
- `value`：字符串，表示该`<input>`节点的值。该属性可读写。
- `validity`：返回一个`ValidityState`对象，表示`<input>`节点的校验状态。该属性只读。
- `validationMessage`：字符串，表示该`<input>`节点的校验失败时，用户看到的报错信息。如果该节点不需要校验，或者通过校验，该属性为空字符串。该属性只读。
- `willValidate`：布尔值，表示表单提交时，该`<input>`元素是否会被校验。该属性只读。