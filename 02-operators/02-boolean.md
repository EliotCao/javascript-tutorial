# 布尔运算符

## 概述

布尔运算符用于将表达式转为布尔值，一共包含四个运算符。

- 取反运算符：`!`
- 且运算符：`&&`
- 或运算符：`||`
- 三元运算符：`?:`

## 取反运算符（!）

取反运算符是一个感叹号，用于将布尔值变为相反值，即`true`变成`false`，`false`变成`true`。

```
!true // false
!false // true
```

对于非布尔值，取反运算符会将其转为布尔值。可以这样记忆，以下六个值取反后为`true`，其他值都为`false`。

- `undefined`
- `null`
- `false`
- `0`
- `NaN`
- 空字符串（`''`）

```
!undefined // true
!null // true
!0 // true
!NaN // true
!"" // true

!54 // false
!'hello' // false
![] // false
!{} // false
```

上面代码中，不管什么类型的值，经过取反运算后，都变成了布尔值。