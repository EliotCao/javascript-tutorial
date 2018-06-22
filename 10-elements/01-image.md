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