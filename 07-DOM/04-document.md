# Document 节点

## 概述

`document`节点对象代表整个文档，每张网页都有自己的`document`对象。`window.document`属性就指向这个对象。只要浏览器开始载入 HTML 文档，该对象就存在了，可以直接使用。

`document`对象有不同的办法可以获取。

- 正常的网页，直接使用`document`或`window.document`。
- `iframe`框架里面的网页，使用`iframe`节点的`contentDocument`属性。
- Ajax 操作返回的文档，使用`XMLHttpRequest`对象的`responseXML`属性。
- 内部节点的`ownerDocument`属性。

`document`对象继承了`EventTarget`接口和`Node`接口，并且混入（mixin）了`ParentNode`接口。这意味着，这些接口的方法都可以在`document`对象上调用。除此之外，`document`对象还有很多自己的属性和方法。

## 属性

### 快捷方式属性

以下属性是指向文档内部的某个节点的快捷方式。

**（1）document.defaultView**

`document.defaultView`属性返回`document`对象所属的`window`对象。如果当前文档不属于`window`对象，该属性返回`null`。

```
document.defaultView === window // true
```

**（2）document.doctype**

对于 HTML 文档来说，`document`对象一般有两个子节点。第一个子节点是`document.doctype`，指向`<DOCTYPE>`节点，即文档类型（Document Type Declaration，简写DTD）节点。HTML 的文档类型节点，一般写成`<!DOCTYPE html>`。如果网页没有声明 DTD，该属性返回`null`。

```
var doctype = document.doctype;
doctype // "<!DOCTYPE html>"
doctype.name // "html"
```

`document.firstChild`通常就返回这个节点。

**（3）document.documentElement**

`document.documentElement`属性返回当前文档的根元素节点（root）。它通常是`document`节点的第二个子节点，紧跟在`document.doctype`节点后面。HTML网页的该属性，一般是`<html>`节点。

**（4）document.body，document.head**

`document.body`属性指向`<body>`节点，`document.head`属性指向`<head>`节点。

这两个属性总是存在的，如果网页源码里面省略了`<head>`或`<body>`，浏览器会自动创建。另外，这两个属性是可写的，如果改写它们的值，相当于移除所有子节点。

**（5）document.scrollingElement**

`document.scrollingElement`属性返回文档的滚动元素。也就是说，当文档整体滚动时，到底是哪个元素在滚动。

标准模式下，这个属性返回的文档的根元素`document.documentElement`（即`<html>`）。兼容（quirk）模式下，返回的是`<body>`元素，如果该元素不存在，返回`null`。

```
// 页面滚动到浏览器顶部
document.scrollingElement.scrollTop = 0;
```

**（6）document.activeElement**

`document.activeElement`属性返回获得当前焦点（focus）的 DOM 元素。通常，这个属性返回的是`<input>`、`<textarea>`、`<select>`等表单元素，如果当前没有焦点元素，返回`<body>`元素或`null`。

**（7）document.fullscreenElement**

`document.fullscreenElement`属性返回当前以全屏状态展示的 DOM 元素。如果不是全屏状态，该属性返回`null`。

```
if (document.fullscreenElement.nodeName == 'VIDEO') {
  console.log('全屏播放视频');
}
```

上面代码中，通过`document.fullscreenElement`可以知道`<video>`元素有没有处在全屏状态，从而判断用户行为。

### 节点集合属性

以下属性返回一个`HTMLCollection`实例，表示文档内部特定元素的集合。这些集合都是动态的，原节点有任何变化，立刻会反映在集合中。

**（1）document.links**

`document.links`属性返回当前文档所有设定了`href`属性的`<a>`及`<area>`节点。

```
// 打印文档所有的链接
var links = document.links;
for(var i = 0; i < links.length; i++) {
  console.log(links[i]);
}
```

**（2）document.forms**

`document.forms`属性返回所有`<form>`表单节点。

```
var selectForm = document.forms[0];
```

上面代码获取文档第一个表单。

除了使用位置序号，`id`属性和`name`属性也可以用来引用表单。

```
/* HTML 代码如下
  <form name="foo" id="bar"></form>
*/
document.forms[0] === document.forms.foo // true
document.forms.bar === document.forms.foo // true
```

**（3）document.images**

`document.images`属性返回页面所有`<img>`图片节点。

```
var imglist = document.images;

for(var i = 0; i < imglist.length; i++) {
  if (imglist[i].src === 'banner.gif') {
    // ...
  }
}
```

上面代码在所有`img`标签中，寻找某张图片。

**（4）document.embeds，document.plugins**

`document.embeds`属性和`document.plugins`属性，都返回所有`<embed>`节点。

**（5）document.scripts**

`document.scripts`属性返回所有`<script>`节点。

```
var scripts = document.scripts;
if (scripts.length !== 0 ) {
  console.log('当前网页有脚本');
}
```