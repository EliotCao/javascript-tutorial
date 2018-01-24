# Object 对象

## 概述

JavaScript 原生提供`Object`对象（注意起首的`O`是大写），本章介绍该对象原生的各种方法。

JavaScript 的所有其他对象都继承自`Object`对象，即那些对象都是`Object`的实例。

`Object`对象的原生方法分成两类：`Object`本身的方法与`Object`的实例方法。

**（1）`Object`对象本身的方法**

所谓“本身的方法”就是直接定义在`Object`对象的方法。

```
Object.print = function (o) { console.log(o) };
```

上面代码中，`print`方法就是直接定义在`Object`对象上。