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

**（2）`Object`的实例方法**

所谓实例方法就是定义在`Object`原型对象`Object.prototype`上的方法。它可以被`Object`实例直接使用。

```
Object.prototype.print = function () {
  console.log(this);
};

var obj = new Object();
obj.print() // Object
```

上面代码中，`Object.prototype`定义了一个`print`方法，然后生成一个`Object`的实例`obj`。`obj`直接继承了`Object.prototype`的属性和方法，可以直接使用`obj.print`调用`print`方法。也就是说，`obj`对象的`print`方法实质上就是调用`Object.prototype.print`方法。

关于原型对象`object.prototype`的详细解释，参见《面向对象编程》章节。这里只要知道，凡是定义在`Object.prototype`对象上面的属性和方法，将被所有实例对象共享就可以了。

以下先介绍`Object`作为函数的用法，然后再介绍`Object`对象的原生方法，分成对象自身的方法（又称为“静态方法”）和实例方法两部分。

## Object()

`Object`本身是一个函数，可以当作工具方法使用，将任意值转为对象。这个方法常用于保证某个值一定是对象。

如果参数为空（或者为`undefined`和`null`），`Object()`返回一个空对象。

```
var obj = Object();
// 等同于
var obj = Object(undefined);
var obj = Object(null);

obj instanceof Object // true
```

上面代码的含义，是将`undefined`和`null`转为对象，结果得到了一个空对象`obj`。

`instanceof`运算符用来验证，一个对象是否为指定的构造函数的实例。`obj instanceof Object`返回`true`，就表示`obj`对象是`Object`的实例。

如果参数是原始类型的值，`Object`方法将其转为对应的包装对象的实例（参见《原始类型的包装对象》一章）。

```
var obj = Object(1);
obj instanceof Object // true
obj instanceof Number // true

var obj = Object('foo');
obj instanceof Object // true
obj instanceof String // true

var obj = Object(true);
obj instanceof Object // true
obj instanceof Boolean // true
```

上面代码中，`Object`函数的参数是各种原始类型的值，转换成对象就是原始类型值对应的包装对象。

如果`Object`方法的参数是一个对象，它总是返回该对象，即不用转换。

```
var arr = [];
var obj = Object(arr); // 返回原数组
obj === arr // true

var value = {};
var obj = Object(value) // 返回原对象
obj === value // true

var fn = function () {};
var obj = Object(fn); // 返回原函数
obj === fn // true
```

利用这一点，可以写一个判断变量是否为对象的函数。

```
function isObject(value) {
  return value === Object(value);
}

isObject([]) // true
isObject(true) // false
```

## Object 构造函数

`Object`不仅可以当作工具函数使用，还可以当作构造函数使用，即前面可以使用`new`命令。

`Object`构造函数的首要用途，是直接通过它来生成新对象。

```
var obj = new Object();
```

> 注意，通过`var obj = new Object()`的写法生成新对象，与字面量的写法`var obj = {}`是等价的。或者说，后者只是前者的一种简便写法。

`Object`构造函数的用法与工具方法很相似，几乎一模一样。使用时，可以接受一个参数，如果该参数是一个对象，则直接返回这个对象；如果是一个原始类型的值，则返回该值对应的包装对象（详见《包装对象》一章）。

```
var o1 = {a: 1};
var o2 = new Object(o1);
o1 === o2 // true

var obj = new Object(123);
obj instanceof Number // true
```

虽然用法相似，但是`Object(value)`与`new Object(value)`两者的语义是不同的，`Object(value)`表示将`value`转成一个对象，`new Object(value)`则表示新生成一个对象，它的值是`value`。

## Object 的静态方法

所谓“静态方法”，是指部署在`Object`对象自身的方法。

### Object.keys()，Object.getOwnPropertyNames()

`Object.keys`方法和`Object.getOwnPropertyNames`方法都用来遍历对象的属性。

`Object.keys`方法的参数是一个对象，返回一个数组。该数组的成员都是该对象自身的（而不是继承的）所有属性名。

```
var obj = {
  p1: 123,
  p2: 456
};

Object.keys(obj) // ["p1", "p2"]
```

`Object.getOwnPropertyNames`方法与`Object.keys`类似，也是接受一个对象作为参数，返回一个数组，包含了该对象自身的所有属性名。

```
var obj = {
  p1: 123,
  p2: 456
};

Object.getOwnPropertyNames(obj) // ["p1", "p2"]
```

对于一般的对象来说，`Object.keys()`和`Object.getOwnPropertyNames()`返回的结果是一样的。只有涉及不可枚举属性时，才会有不一样的结果。`Object.keys`方法只返回可枚举的属性（详见《对象属性的描述对象》一章），`Object.getOwnPropertyNames`方法还返回不可枚举的属性名。

```
var a = ['Hello', 'World'];

Object.keys(a) // ["0", "1"]
Object.getOwnPropertyNames(a) // ["0", "1", "length"]
```

上面代码中，数组的`length`属性是不可枚举的属性，所以只出现在`Object.getOwnPropertyNames`方法的返回结果中。

由于 JavaScript 没有提供计算对象属性个数的方法，所以可以用这两个方法代替。

```
var obj = {
  p1: 123,
  p2: 456
};

Object.keys(obj).length // 2
Object.getOwnPropertyNames(obj).length // 2
```

一般情况下，几乎总是使用`Object.keys`方法，遍历对象的属性。