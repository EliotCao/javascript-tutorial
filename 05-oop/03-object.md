# Object 对象的相关方法

JavaScript 在`Object`对象上面，提供了很多相关方法，处理面向对象编程的相关操作。本章介绍这些方法。

Object.getPrototypeOf()

`Object.getPrototypeOf`方法返回参数对象的原型。这是获取原型对象的标准方法。

```
var F = function () {};
var f = new F();
Object.getPrototypeOf(f) === F.prototype // true
```

上面代码中，实例对象`f`的原型是`F.prototype`。