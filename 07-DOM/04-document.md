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

**（6）document.styleSheets**

`document.styleSheets`属性返回文档内嵌或引入的样式表集合，详细介绍请看《CSS 对象模型》一章。

**（7）小结**

除了`document.styleSheets`，以上的集合属性返回的都是`HTMLCollection`实例。

```
document.links instanceof HTMLCollection // true
document.images instanceof HTMLCollection // true
document.forms instanceof HTMLCollection // true
document.embeds instanceof HTMLCollection // true
document.scripts instanceof HTMLCollection // true
```

`HTMLCollection`实例是类似数组的对象，所以这些属性都有`length`属性，都可以使用方括号运算符引用成员。如果成员有`id`或`name`属性，还可以用这两个属性的值，在`HTMLCollection`实例上引用到这个成员。

```
// HTML 代码如下
// <form name="myForm">
document.myForm === document.forms.myForm // true
```

### 文档静态信息属性

以下属性返回文档信息。

**（1）document.documentURI，document.URL**

`document.documentURI`属性和`document.URL`属性都返回一个字符串，表示当前文档的网址。不同之处是它们继承自不同的接口，`documentURI`继承自`Document`接口，可用于所有文档；`URL`继承自`HTMLDocument`接口，只能用于 HTML 文档。

```
document.URL
// http://www.example.com/about

document.documentURI === document.URL
// true
```

如果文档的锚点（`#anchor`）变化，这两个属性都会跟着变化。

**（2）document.domain**

`document.domain`属性返回当前文档的域名，不包含协议和端口。比如，网页的网址是`http://www.example.com:80/hello.html`，那么`document.domain`属性就等于`www.example.com`。如果无法获取域名，该属性返回`null`。

`document.domain`基本上是一个只读属性，只有一种情况除外。次级域名的网页，可以把`document.domain`设为对应的上级域名。比如，当前域名是`a.sub.example.com`，则`document.domain`属性可以设置为`sub.example.com`，也可以设为`example.com`。修改后，`document.domain`相同的两个网页，可以读取对方的资源，比如设置的 Cookie。

另外，设置`document.domain`会导致端口被改成`null`。因此，如果通过设置`document.domain`来进行通信，双方网页都必须设置这个值，才能保证端口相同。

**（3）document.location**

`Location`对象是浏览器提供的原生对象，提供 URL 相关的信息和操作方法。通过`window.location`和`document.location`属性，可以拿到这个对象。

关于这个对象的详细介绍，请看《浏览器模型》部分的《Location 对象》章节。

**（4）document.lastModified**

`document.lastModified`属性返回一个字符串，表示当前文档最后修改的时间。不同浏览器的返回值，日期格式是不一样的。

```
document.lastModified
// "03/07/2018 11:18:27"
```

注意，`document.lastModified`属性的值是字符串，所以不能直接用来比较。`Date.parse`方法将其转为`Date`实例，才能比较两个网页。

```
var lastVisitedDate = Date.parse('01/01/2018');
if (Date.parse(document.lastModified) > lastVisitedDate) {
  console.log('网页已经变更');
}
```

如果页面上有 JavaScript 生成的内容，`document.lastModified`属性返回的总是当前时间。

**（5）document.title**

`document.title`属性返回当前文档的标题。默认情况下，返回`<title>`节点的值。但是该属性是可写的，一旦被修改，就返回修改后的值。

```
document.title = '新标题';
document.title // "新标题"
```

**（6）document.characterSet**

`document.characterSet`属性返回当前文档的编码，比如`UTF-8`、`ISO-8859-1`等等。

**（7）document.referrer**

`document.referrer`属性返回一个字符串，表示当前文档的访问者来自哪里。

```
document.referrer
// "https://example.com/path"
```

如果无法获取来源，或者用户直接键入网址而不是从其他网页点击进入，`document.referrer`返回一个空字符串。

`document.referrer`的值，总是与 HTTP 头信息的`Referer`字段保持一致。但是，`document.referrer`的拼写有两个`r`，而头信息的`Referer`字段只有一个`r`。

**（8）document.dir**

`document.dir`返回一个字符串，表示文字方向。它只有两个可能的值：`rtl`表示文字从右到左，阿拉伯文是这种方式；`ltr`表示文字从左到右，包括英语和汉语在内的大多数文字采用这种方式。

**（9）document.compatMode**

`compatMode`属性返回浏览器处理文档的模式，可能的值为`BackCompat`（向后兼容模式）和`CSS1Compat`（严格模式）。

一般来说，如果网页代码的第一行设置了明确的`DOCTYPE`（比如`<!doctype html>`），`document.compatMode`的值都为`CSS1Compat`。

### 文档状态属性

**（1）document.hidden**

`document.hidden`属性返回一个布尔值，表示当前页面是否可见。如果窗口最小化、浏览器切换了 Tab，都会导致导致页面不可见，使得`document.hidden`返回`true`。

这个属性是 Page Visibility API 引入的，一般都是配合这个 API 使用。

**（2）document.visibilityState**

`document.visibilityState`返回文档的可见状态。

它的值有四种可能。

> - `visible`：页面可见。注意，页面可能是部分可见，即不是焦点窗口，前面被其他窗口部分挡住了。
> - `hidden`：页面不可见，有可能窗口最小化，或者浏览器切换到了另一个 Tab。
> - `prerender`：页面处于正在渲染状态，对于用户来说，该页面不可见。
> - `unloaded`：页面从内存里面卸载了。

这个属性可以用在页面加载时，防止加载某些资源；或者页面不可见时，停掉一些页面功能。

**（3）document.readyState**

`document.readyState`属性返回当前文档的状态，共有三种可能的值。

- `loading`：加载 HTML 代码阶段（尚未完成解析）
- `interactive`：加载外部资源阶段
- `complete`：加载完成

这个属性变化的过程如下。

1. 浏览器开始解析 HTML 文档，`document.readyState`属性等于`loading`。
2. 浏览器遇到 HTML 文档中的`<script>`元素，并且没有`async`或`defer`属性，就暂停解析，开始执行脚本，这时`document.readyState`属性还是等于`loading`。
3. HTML 文档解析完成，`document.readyState`属性变成`interactive`。
4. 浏览器等待图片、样式表、字体文件等外部资源加载完成，一旦全部加载完成，`document.readyState`属性变成`complete`。

下面的代码用来检查网页是否加载成功。

```
// 基本检查
if (document.readyState === 'complete') {
  // ...
}

// 轮询检查
var interval = setInterval(function() {
  if (document.readyState === 'complete') {
    clearInterval(interval);
    // ...
  }
}, 100);
```

另外，每次状态变化都会触发一个`readystatechange`事件。

### document.cookie

`document.cookie`属性用来操作浏览器 Cookie，详见《浏览器模型》部分的《Cookie》章节。

### document.designMode

`document.designMode`属性控制当前文档是否可编辑。该属性只有两个值`on`和`off`，默认值为`off`。一旦设为`on`，用户就可以编辑整个文档的内容。

下面代码打开`iframe`元素内部文档的`designMode`属性，就能将其变为一个所见即所得的编辑器。

```
// HTML 代码如下
// <iframe id="editor" src="about:blank"></iframe>
var editor = document.getElementById('editor');
editor.contentDocument.designMode = 'on';
```

### document.currentScript

`document.currentScript`属性只用在`<script>`元素的内嵌脚本或加载的外部脚本之中，返回当前脚本所在的那个 DOM 节点，即`<script>`元素的 DOM 节点。

```
<script id="foo">
  console.log(
    document.currentScript === document.getElementById('foo')
  ); // true
</script>
```

上面代码中，`document.currentScript`就是`<script>`元素节点。

### document.implementation

`document.implementation`属性返回一个`DOMImplementation`对象。该对象有三个方法，主要用于创建独立于当前文档的新的 Document 对象。

- `DOMImplementation.createDocument()`：创建一个 XML 文档。
- `DOMImplementation.createHTMLDocument()`：创建一个 HTML 文档。
- `DOMImplementation.createDocumentType()`：创建一个 DocumentType 对象。

下面是创建 HTML 文档的例子。

```
var doc = document.implementation.createHTMLDocument('Title');
var p = doc.createElement('p');
p.innerHTML = 'hello world';
doc.body.appendChild(p);

document.replaceChild(
  doc.documentElement,
  document.documentElement
);
```

上面代码中，第一步生成一个新的 HTML 文档`doc`，然后用它的根元素`document.documentElement`替换掉`document.documentElement`。这会使得当前文档的内容全部消失，变成`hello world`。

## 方法

### document.open()，document.close()

`document.open`方法清除当前文档所有内容，使得文档处于可写状态，供`document.write`方法写入内容。

`document.close`方法用来关闭`document.open()`打开的文档。

```
document.open();
document.write('hello world');
document.close();
```

### document.write()，document.writeln()

`document.write`方法用于向当前文档写入内容。

在网页的首次渲染阶段，只要页面没有关闭写入（即没有执行`document.close()`），`document.write`写入的内容就会追加在已有内容的后面。

```
// 页面显示“helloworld”
document.open();
document.write('hello');
document.write('world');
document.close();
```

注意，`document.write`会当作 HTML 代码解析，不会转义。

```
document.write('<p>hello world</p>');
```

上面代码中，`document.write`会将`<p>`当作 HTML 标签解释。

如果页面已经解析完成（`DOMContentLoaded`事件发生之后），再调用`write`方法，它会先调用`open`方法，擦除当前文档所有内容，然后再写入。

```
document.addEventListener('DOMContentLoaded', function (event) {
  document.write('<p>Hello World!</p>');
});

// 等同于
document.addEventListener('DOMContentLoaded', function (event) {
  document.open();
  document.write('<p>Hello World!</p>');
  document.close();
});
```

如果在页面渲染过程中调用`write`方法，并不会自动调用`open`方法。（可以理解成，`open`方法已调用，但`close`方法还未调用。）

```
<html>
<body>
hello
<script type="text/javascript">
  document.write("world")
</script>
</body>
</html>
```

在浏览器打开上面网页，将会显示`hello world`。

`document.write`是 JavaScript 语言标准化之前就存在的方法，现在完全有更符合标准的方法向文档写入内容（比如对`innerHTML`属性赋值）。所以，除了某些特殊情况，应该尽量避免使用`document.write`这个方法。

`document.writeln`方法与`write`方法完全一致，除了会在输出内容的尾部添加换行符。

```
document.write(1);
document.write(2);
// 12

document.writeln(1);
document.writeln(2);
// 1
// 2
//
```

注意，`writeln`方法添加的是 ASCII 码的换行符，渲染成 HTML 网页时不起作用，即在网页上显示不出换行。网页上的换行，必须显式写入`<br>`。