# GlobalEventHandlers 接口

指定事件的回调函数，推荐使用的方法是元素的`addEventListener`方法。

```
div.addEventListener('click', clickHandler, false);
```

除了之外，还有一种方法可以直接指定事件的回调函数。

```
div.onclick = clickHandler;
```

这个接口是由`GlobalEventHandlers`接口提供的。它的优点是使用比较方便，缺点是只能为每个事件指定一个回调函数，并且无法指定事件触发的阶段（捕获阶段还是冒泡阶段）。

`HTMLElement`、`Document`和`Window`都继承了这个接口，也就是说，各种 HTML 元素、`document`对象、`window`对象上面都可以使用`GlobalEventHandlers`接口提供的属性。下面就列出这个接口提供的主要的事件属性。

## GlobalEventHandlers.onabort

某个对象的`abort`事件（停止加载）发生时，就会调用`onabort`属性指定的回调函数。

各种元素的停止加载事件，到底如何触发，目前并没有统一的规定。因此实际上，这个属性现在一般只用在`<img>`元素上面。

```
// HTML 代码如下
// <img src="example.jpg" id="img">
var img = document.getElementById('img');
img.onabort = function () {
  console.log('image load aborted.');
}
```

## GlobalEventHandlers.onerror

`error`事件发生时，就会调用`onerror`属性指定的回调函数。

`error`事件分成两种。

一种是 JavaScript 的运行时错误，这会传到`window`对象，导致`window.onerror()`。

```
window.onerror = function (message, source, lineno, colno, error) {
  // ...
}
```

`window.onerror`的处理函数共接受五个参数，含义如下。

- message：错误信息字符串
- source：报错脚本的 URL
- lineno：报错的行号，是一个整数
- colno：报错的列号，是一个整数
- error： 错误对象

另一种是资源加载错误，比如`<img>`或`<script>`加载的资源出现加载错误。这时，Error 对象会传到对应的元素，导致该元素的`onerror`属性开始执行。

```
element.onerror = function (event) {
  // ...
}
```

注意，一般来说，资源的加载错误不会触发`window.onerror`。

## GlobalEventHandlers.onload、GlobalEventHandlers.onloadstart

元素完成加载时，会触发`load`事件，执行`onload()`。它的典型使用场景是`window`对象和`<img>`元素。对于`window`对象来说，只有页面的所有资源加载完成（包括图片、脚本、样式表、字体等所有外部资源），才会触发`load`事件。

对于`<img>`和`<video>`等元素，加载开始时还会触发`loadstart`事件，导致执行`onloadstart`。