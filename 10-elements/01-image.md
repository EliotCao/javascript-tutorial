# 元素

## 概述

<img>元素用于插入图片，主要继承了 HTMLImageElement 接口。

浏览器提供一个原生构造函数`Image`，用于生成`HTMLImageElement`实例。

```
var img = new Image();
img instanceof Image // true
img instanceof HTMLImageElement // true
```

`Image`构造函数可以接受两个整数作为参数，分别表示`<img>`元素的宽度和高度。

```
// 语法
Image(width, height)

// 用法
var myImage = new Image(100, 200);
```

<img>实例的src属性可以定义图像的网址。

```
var img = new Image();
img.src = 'picture.jpg';
```

新生成的`<img>`实例并不属于文档的一部分。如果想让它显示在文档中，必须手动插入文档。

```
var img = new Image();
img.src = 'image1.png';
document.body.appendChild(img);
```

除了使用`Image`构造，下面的方法也可以得到`HTMLImageElement`实例。

- `document.images`的成员
- 节点选取方法（比如`document.getElementById`）得到的`<img>`节点
- `document.createElement('img')`生成的`<img>`节点

```
document.images[0] instanceof HTMLImageElement
// true

var img = document.getElementById('myImg');
img instanceof HTMLImageElement
// true

var img = document.createElement('img');
img instanceof HTMLImageElement
// true
```

`HTMLImageElement`实例除了具有 Node、Element、HTMLElement 接口以外，还拥有一些独有的属性。这个接口没有定义自己的方法。

## 特性相关的属性

**（1）HTMLImageElement.src**

`HTMLImageElement.src`属性返回图像的完整网址。

```
// HTML 代码如下
// <img width="300" height="400" id="myImg" src="http://example.com/pic.jpg">
var img = document.getElementById('img');
img.src // http://example.com/pic.jpg
```

**（2）HTMLImageElement.currentSrc**

`HTMLImageElement.currentSrc`属性返回当前正在展示的图像的网址。JavaScript 和 CSS 的 mediaQuery 都可能改变正在展示的图像。

**（3）HTMLImageElement.alt**

`HTMLImageElement.alt`属性可以读写`<img>`的 HTML 属性`alt`，表示对图片的文字说明。

**（4）HTMLImageElement.isMap，HTMLImageElement.useMap**

`HTMLImageElement.isMap`属性对应`<img>`元素的 HTML 属性`ismap`，返回一个布尔值，表示图像是否为服务器端的图像映射的一部分。

`HTMLImageElement.useMap`属性对应`<img>`元素的 HTML 属性`usemap`，表示当前图像对应的`<map>`元素。