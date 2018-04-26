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

**（3）Touch.radiusX，Touch.radiusY，Touch.rotationAngle**

`Touch.radiusX`属性和`Touch.radiusY`属性，分别返回触摸点周围受到影响的椭圆范围的 X 轴半径和 Y 轴半径，单位为像素。乘以 2 就可以得到触摸范围的宽度和高度。

`Touch.rotationAngle`属性表示触摸区域的椭圆的旋转角度，单位为度数，在`0`到`90`度之间。

上面这三个属性共同定义了用户与屏幕接触的区域，对于描述手指这一类非精确的触摸，很有帮助。指尖接触屏幕，触摸范围会形成一个椭圆，这三个属性就用来描述这个椭圆区域。

下面是一个示例。

```
div.addEventListener('touchstart', rotate);
div.addEventListener('touchmove', rotate);
div.addEventListener('touchend', rotate);

function rotate(e) {
  var touch = e.changedTouches.item(0);
  e.preventDefault();

  src.style.width = touch.radiusX * 2 + 'px';
  src.style.height = touch.radiusY * 2 + 'px';
  src.style.transform = 'rotate(' + touch.rotationAngle + 'deg)';
};
```

**（4）Touch.force**

`Touch.force`属性返回一个`0`到`1`之间的数值，表示触摸压力。`0`代表没有压力，`1`代表硬件所能识别的最大压力。

**（5）Touch.target**

`Touch.target`属性返回一个元素节点，代表触摸发生时所在的那个元素节点。即使触摸点已经离开了这个节点，该属性依然不变。

## TouchList 接口

`TouchList`接口表示一组触摸点的集合。它的实例是一个类似数组的对象，成员是`Touch`的实例对象，表示所有触摸点。用户用三根手指触摸，产生的`TouchList`实例就会包含三个成员，每根手指的触摸点对应一个`Touch`实例对象。

它的实例主要通过触摸事件的`TouchEvent.touches`、`TouchEvent.changedTouches`、`TouchEvent.targetTouches`这几个属性获取。

它的实例属性和实例方法只有两个。

- `TouchList.length`：数值，表示成员数量（即触摸点的数量）。
- `TouchList.item()`：返回指定位置的成员，它的参数是该成员的位置编号（从零开始）。