# Text 节点和 DocumentFragment 节点

## Text 节点的概念

文本节点（`Text`）代表元素节点（`Element`）和属性节点（`Attribute`）的文本内容。如果一个节点只包含一段文本，那么它就有一个文本子节点，代表该节点的文本内容。

通常我们使用父节点的`firstChild`、`nextSibling`等属性获取文本节点，或者使用`Document`节点的`createTextNode`方法创造一个文本节点。

```
// 获取文本节点
var textNode = document.querySelector('p').firstChild;

// 创造文本节点
var textNode = document.createTextNode('Hi');
document.querySelector('div').appendChild(textNode);
```

浏览器原生提供一个`Text`构造函数。它返回一个文本节点实例。它的参数就是该文本节点的文本内容。

```
// 空字符串
var text1 = new Text();

// 非空字符串
var text2 = new Text('This is a text node');
```

注意，由于空格也是一个字符，所以哪怕只有一个空格，也会形成文本节点。比如，`<p> </p>`包含一个空格，它的子节点就是一个文本节点。

文本节点除了继承`Node`接口，还继承了`CharacterData`接口。`Node`接口的属性和方法请参考《Node 接口》一章，这里不再重复介绍了，以下的属性和方法大部分来自`CharacterData`接口。

## Text 节点的属性

### data

`data`属性等同于`nodeValue`属性，用来设置或读取文本节点的内容。

```
// 读取文本内容
document.querySelector('p').firstChild.data
// 等同于
document.querySelector('p').firstChild.nodeValue

// 设置文本内容
document.querySelector('p').firstChild.data = 'Hello World';
```

### wholeText

`wholeText`属性将当前文本节点与毗邻的文本节点，作为一个整体返回。大多数情况下，`wholeText`属性的返回值，与`data`属性和`textContent`属性相同。但是，某些特殊情况会有差异。

举例来说，HTML 代码如下。

```
<p id="para">A <em>B</em> C</p>
```

这时，文本节点的`wholeText`属性和`data`属性，返回值相同。

```
var el = document.getElementById('para');
el.firstChild.wholeText // "A "
el.firstChild.data // "A "
```

但是，一旦移除`<em>`节点，`wholeText`属性与`data`属性就会有差异，因为这时其实`<p>`节点下面包含了两个毗邻的文本节点。

```
el.removeChild(para.childNodes[1]);
el.firstChild.wholeText // "A C"
el.firstChild.data // "A "
```

### length

`length`属性返回当前文本节点的文本长度。

```
(new Text('Hello')).length // 5
```

### nextElementSibling，previousElementSibling

`nextElementSibling`属性返回紧跟在当前文本节点后面的那个同级元素节点。如果取不到元素节点，则返回`null`。

```
// HTML 为
// <div>Hello <em>World</em></div>
var tn = document.querySelector('div').firstChild;
tn.nextElementSibling
// <em>World</em>
```

`previousElementSibling`属性返回当前文本节点前面最近的同级元素节点。如果取不到元素节点，则返回`null：`。

## Text 节点的方法

### appendData()，deleteData()，insertData()，replaceData()，subStringData()

以下5个方法都是编辑`Text`节点文本内容的方法。

- `appendData()`：在`Text`节点尾部追加字符串。
- `deleteData()`：删除`Text`节点内部的子字符串，第一个参数为子字符串开始位置，第二个参数为子字符串长度。
- `insertData()`：在`Text`节点插入字符串，第一个参数为插入位置，第二个参数为插入的子字符串。
- `replaceData()`：用于替换文本，第一个参数为替换开始位置，第二个参数为需要被替换掉的长度，第三个参数为新加入的字符串。
- `subStringData()`：用于获取子字符串，第一个参数为子字符串在`Text`节点中的开始位置，第二个参数为子字符串长度。

```
// HTML 代码为
// <p>Hello World</p>
var pElementText = document.querySelector('p').firstChild;

pElementText.appendData('!');
// 页面显示 Hello World!
pElementText.deleteData(7, 5);
// 页面显示 Hello W
pElementText.insertData(7, 'Hello ');
// 页面显示 Hello WHello
pElementText.replaceData(7, 5, 'World');
// 页面显示 Hello WWorld
pElementText.substringData(7, 10);
// 页面显示不变，返回"World "
```

### remove()

`remove`方法用于移除当前`Text`节点。

```
// HTML 代码为
// <p>Hello World</p>
document.querySelector('p').firstChild.remove()
// 现在 HTML 代码为
// <p></p>
```

### splitText()

`splitText`方法将`Text`节点一分为二，变成两个毗邻的`Text`节点。它的参数就是分割位置（从零开始），分割到该位置的字符前结束。如果分割位置不存在，将报错。

分割后，该方法返回分割位置后方的字符串，而原`Text`节点变成只包含分割位置前方的字符串。

```
// html 代码为 <p id="p">foobar</p>
var p = document.getElementById('p');
var textnode = p.firstChild;

var newText = textnode.splitText(3);
newText // "bar"
textnode // "foo"
```

父元素节点的`normalize`方法可以将毗邻的两个`Text`节点合并。

接上面的例子，文本节点的`splitText`方法将一个`Text`节点分割成两个，父元素的`normalize`方法可以实现逆操作，将它们合并。

```
p.childNodes.length // 2

// 将毗邻的两个 Text 节点合并
p.normalize();
p.childNodes.length // 1
```