# window 对象

## 概述

浏览器里面，`window`对象（注意，`w`为小写）指当前的浏览器窗口。它也是当前页面的顶层对象，即最高一层的对象，所有其他对象都是它的下属。一个变量如果未声明，那么默认就是顶层对象的属性。

```
a = 1;
window.a // 1
```

上面代码中，`a`是一个没有声明就直接赋值的变量，它自动成为顶层对象的属性。

`window`有自己的实体含义，其实不适合当作最高一层的顶层对象，这是一个语言的设计失误。最早，设计这门语言的时候，原始设想是语言内置的对象越少越好，这样可以提高浏览器的性能。因此，语言设计者 Brendan Eich 就把`window`对象当作顶层对象，所有未声明就赋值的变量都自动变成`window`对象的属性。这种设计使得编译阶段无法检测出未声明变量，但到了今天已经没有办法纠正了。

## window 对象的属性

### window.name

`window.name`属性是一个字符串，表示当前浏览器窗口的名字。窗口不一定需要名字，这个属性主要配合超链接和表单的`target`属性使用。

```
window.name = 'Hello World!';
console.log(window.name)
// "Hello World!"
```

该属性只能保存字符串，如果写入的值不是字符串，会自动转成字符串。各个浏览器对这个值的储存容量有所不同，但是一般来说，可以高达几MB。

只要浏览器窗口不关闭，这个属性是不会消失的。举例来说，访问`a.com`时，该页面的脚本设置了`window.name`，接下来在同一个窗口里面载入了`b.com`，新页面的脚本可以读到上一个网页设置的`window.name`。页面刷新也是这种情况。一旦浏览器窗口关闭后，该属性保存的值就会消失，因为这时窗口已经不存在了。

### window.closed，window.opener

`window.closed`属性返回一个布尔值，表示窗口是否关闭。

```
window.closed // false
```

上面代码检查当前窗口是否关闭。这种检查意义不大，因为只要能运行代码，当前窗口肯定没有关闭。这个属性一般用来检查，使用脚本打开的新窗口是否关闭。

```
var popup = window.open();

if ((popup !== null) && !popup.closed) {
  // 窗口仍然打开着
}
```

`window.opener`属性表示打开当前窗口的父窗口。如果当前窗口没有父窗口（即直接在地址栏输入打开），则返回`null`。

```
window.open().opener === window // true
```

上面表达式会打开一个新窗口，然后返回`true`。

如果两个窗口之间不需要通信，建议将子窗口的`opener`属性显式设为`null`，这样可以减少一些安全隐患。

```
var newWin = window.open('example.html', 'newWindow', 'height=400,width=400');
newWin.opener = null;
```

上面代码中，子窗口的`opener`属性设为`null`，两个窗口之间就没办法再联系了。

通过`opener`属性，可以获得父窗口的全局属性和方法，但只限于两个窗口同源的情况（参见《同源限制》一章），且其中一个窗口由另一个打开。`<a>`元素添加`rel="noopener"`属性，可以防止新打开的窗口获取父窗口，减轻被恶意网站修改父窗口 URL 的风险。

```
<a href="https://an.evil.site" target="_blank" rel="noopener">
恶意网站
</a>
```

### window.self，window.window

`window.self`和`window.window`属性都指向窗口本身。这两个属性只读。

```
window.self === window // true
window.window === window // true
```

### window.frames，window.length

`window.frames`属性返回一个类似数组的对象，成员为页面内所有框架窗口，包括`frame`元素和`iframe`元素。`window.frames[0]`表示页面中第一个框架窗口。

如果`iframe`元素设置了`id`或`name`属性，那么就可以用属性值，引用这个`iframe`窗口。比如`<iframe name="myIFrame">`可以用`frames['myIFrame']`或者`frames.myIFrame`来引用。

`frames`属性实际上是`window`对象的别名。

```
frames === window // true
```

因此，`frames[0]`也可以用`window[0]`表示。但是，从语义上看，`frames`更清晰，而且考虑到`window`还是全局对象，因此推荐表示多窗口时，总是使用`frames[0]`的写法。更多介绍请看下文的《多窗口操作》部分。

`window.length`属性返回当前网页包含的框架总数。如果当前网页不包含`frame`和`iframe`元素，那么`window.length`就返回`0`。

```
window.frames.length === window.length // true
```

上面代码表示，`window.frames.length`与`window.length`应该是相等的。