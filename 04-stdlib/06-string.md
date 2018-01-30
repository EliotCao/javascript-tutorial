# String 对象

## 概述

`String`对象是 JavaScript 原生提供的三个包装对象之一，用来生成字符串对象。

```
var s1 = 'abc';
var s2 = new String('abc');

typeof s1 // "string"
typeof s2 // "object"

s2.valueOf() // "abc"
```

上面代码中，变量`s1`是字符串，`s2`是对象。由于`s2`是字符串对象，`s2.valueOf`方法返回的就是它所对应的原始字符串。

字符串对象是一个类似数组的对象（很像数组，但不是数组）。

```
new String('abc')
// String {0: "a", 1: "b", 2: "c", length: 3}

(new String('abc'))[1] // "b"
```

上面代码中，字符串`abc`对应的字符串对象，有数值键（`0`、`1`、`2`）和`length`属性，所以可以像数组那样取值。

除了用作构造函数，`String`对象还可以当作工具方法使用，将任意类型的值转为字符串。

```
String(true) // "true"
String(5) // "5"
```

上面代码将布尔值`true`和数值`5`，分别转换为字符串。