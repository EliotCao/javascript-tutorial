# 触摸事件

## 触摸操作概述

浏览器的触摸 API 由三个部分组成。

- Touch：一个触摸点
- TouchList：多个触摸点的集合
- TouchEvent：触摸引发的事件实例

`Touch`接口的实例对象用来表示触摸点（一根手指或者一根触摸笔），包括位置、大小、形状、压力、目标元素等属性。有时，触摸动作由多个触摸点（多根手指）组成，多个触摸点的集合由`TouchList`接口的实例对象表示。`TouchEvent`接口的实例对象代表由触摸引发的事件，只有触摸屏才会引发这一类事件。

很多时候，触摸事件和鼠标事件同时触发，即使这个时候并没有用到鼠标。这是为了让那些只定义鼠标事件、没有定义触摸事件的代码，在触摸屏的情况下仍然能用。如果想避免这种情况，可以用`event.preventDefault`方法阻止发出鼠标事件。

## Touch 接口

### Touch 接口概述

Touch 接口代表单个触摸点。触摸点可能是一根手指，也可能是一根触摸笔。

浏览器原生提供`Touch`构造函数，用来生成`Touch`实例。

```
var touch = new Touch(touchOptions);
```

`Touch`构造函数接受一个配置对象作为参数，它有以下属性。

- `identifier`：必需，类型为整数，表示触摸点的唯一 ID。
- `target`：必需，类型为元素节点，表示触摸点开始时所在的网页元素。
- `clientX`：可选，类型为数值，表示触摸点相对于浏览器窗口左上角的水平距离，默认为0。
- `clientY`：可选，类型为数值，表示触摸点相对于浏览器窗口左上角的垂直距离，默认为0。
- `screenX`：可选，类型为数值，表示触摸点相对于屏幕左上角的水平距离，默认为0。
- `screenY`：可选，类型为数值，表示触摸点相对于屏幕左上角的垂直距离，默认为0。
- `pageX`：可选，类型为数值，表示触摸点相对于网页左上角的水平位置（即包括页面的滚动距离），默认为0。
- `pageY`：可选，类型为数值，表示触摸点相对于网页左上角的垂直位置（即包括页面的滚动距离），默认为0。
- `radiusX`：可选，类型为数值，表示触摸点周围受到影响的椭圆范围的 X 轴半径，默认为0。
- `radiusY`：可选：类型为数值，表示触摸点周围受到影响的椭圆范围的 Y 轴半径，默认为0。
- `rotationAngle`：可选，类型为数值，表示触摸区域的椭圆的旋转角度，单位为度数，在0到90度之间，默认值为0。
- `force`：可选，类型为数值，范围在`0`到`1`之间，表示触摸压力。`0`代表没有压力，`1`代表硬件所能识别的最大压力，默认为`0`。

### Touch 接口的实例属性

**（1）Touch.identifier**

`Touch.identifier`属性返回一个整数，表示触摸点的唯一 ID。这个值在整个触摸过程保持不变，直到触摸事件结束。

```
someElement.addEventListener('touchmove', function (e) {
  for (var i = 0; i < e.changedTouches.length; i++) {
    console.log(e.changedTouches[i].identifier);
  }
}, false);
```

**（2）Touch.screenX，Touch.screenY，Touch.clientX，Touch.clientY，pageX，pageY**

`Touch.screenX`属性和`Touch.screenY`属性，分别表示触摸点相对于屏幕左上角的横坐标和纵坐标，与页面是否滚动无关。

`Touch.clientX`属性和`Touch.clientY`属性，分别表示触摸点相对于浏览器视口左上角的横坐标和纵坐标，与页面是否滚动无关。

`Touch.pageX`属性和`Touch.pageY`属性，分别表示触摸点相对于当前页面左上角的横坐标和纵坐标，包含了页面滚动带来的位移。