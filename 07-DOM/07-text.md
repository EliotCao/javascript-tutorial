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