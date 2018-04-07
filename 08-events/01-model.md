# 事件模型

## 监听函数

浏览器的事件模型，就是通过监听函数（listener）对事件做出反应。事件发生后，浏览器监听到了这个事件，就会执行对应的监听函数。这是事件驱动编程模式（event-driven）的主要编程方式。

JavaScript 有三种方法，可以为事件绑定监听函数。

### HTML 的 on- 属性

HTML 语言允许在元素的属性中，直接定义某些事件的监听代码。

```
<body onload="doSomething()">
<div onclick="console.log('触发事件')">
```

上面代码为`body`节点的`load`事件、`div`节点的`click`事件，指定了监听代码。一旦事件发生，就会执行这段代码。

元素的事件监听属性，都是`on`加上事件名，比如`onload`就是`on + load`，表示`load`事件的监听代码。

注意，这些属性的值是将会执行的代码，而不是一个函数。

```
<!-- 正确 -->
<body onload="doSomething()">

<!-- 错误 -->
<body onload="doSomething">
```

一旦指定的事件发生，`on-`属性的值是原样传入 JavaScript 引擎执行。因此如果要执行函数，不要忘记加上一对圆括号。

使用这个方法指定的监听代码，只会在冒泡阶段触发。

```
<div onclick="console.log(2)">
  <button onclick="console.log(1)">点击</button>
</div>
```

上面代码中，`<button>`是`<div>`的子元素。`<button>`的`click`事件，也会触发`<div>`的`click`事件。由于`on-`属性的监听代码，只在冒泡阶段触发，所以点击结果是先输出`1`，再输出`2`，即事件从子元素开始冒泡到父元素。