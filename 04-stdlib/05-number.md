# Number 对象

## 概述

`Number`对象是数值对应的包装对象，可以作为构造函数使用，也可以作为工具函数使用。

作为构造函数时，它用于生成值为数值的对象。

```
var n = new Number(1);
typeof n // "object"
```

上面代码中，`Number`对象作为构造函数使用，返回一个值为`1`的对象。

作为工具函数时，它可以将任何类型的值转为数值。

```
Number(true) // 1
```

上面代码将布尔值`true`转为数值`1`。`Number`作为工具函数的用法，详见《数据类型转换》一章。

## 静态属性

`Number`对象拥有以下一些静态属性（即直接定义在`Number`对象上的属性，而不是定义在实例上的属性）。

- `Number.POSITIVE_INFINITY`：正的无限，指向`Infinity`。
- `Number.NEGATIVE_INFINITY`：负的无限，指向`-Infinity`。
- `Number.NaN`：表示非数值，指向`NaN`。
- `Number.MIN_VALUE`：表示最小的正数（即最接近0的正数，在64位浮点数体系中为`5e-324`），相应的，最接近0的负数为`-Number.MIN_VALUE`。
- `Number.MAX_SAFE_INTEGER`：表示能够精确表示的最大整数，即`9007199254740991`。
- `Number.MIN_SAFE_INTEGER`：表示能够精确表示的最小整数，即`-9007199254740991`。

```
Number.POSITIVE_INFINITY // Infinity
Number.NEGATIVE_INFINITY // -Infinity
Number.NaN // NaN

Number.MAX_VALUE
// 1.7976931348623157e+308
Number.MAX_VALUE < Infinity
// true

Number.MIN_VALUE
// 5e-324
Number.MIN_VALUE > 0
// true

Number.MAX_SAFE_INTEGER // 9007199254740991
Number.MIN_SAFE_INTEGER // -9007199254740991
```