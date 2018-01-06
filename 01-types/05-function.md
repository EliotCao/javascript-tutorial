# 函数

函数是一段可以反复调用的代码块。函数还能接受输入的参数，不同的参数会返回不同的值。

## 概述

### 函数的声明

JavaScript 有三种声明函数的方法。

**（1）function 命令**

`function`命令声明的代码区块，就是一个函数。`function`命令后面是函数名，函数名后面是一对圆括号，里面是传入函数的参数。函数体放在大括号里面。

```
function print(s) {
  console.log(s);
}
```

上面的代码命名了一个`print`函数，以后使用`print()`这种形式，就可以调用相应的代码。这叫做函数的声明（Function Declaration）。