# ParentNode 接口，ChildNode 接口

节点对象除了继承 Node 接口以外，还拥有其他接口。`ParentNode`接口表示当前节点是一个父节点，提供一些处理子节点的方法。`ChildNode`接口表示当前节点是一个子节点，提供一些相关方法。

## ParentNode 接口

如果当前节点是父节点，就会混入了（mixin）`ParentNode`接口。由于只有元素节点（element）、文档节点（document）和文档片段节点（documentFragment）拥有子节点，因此只有这三类节点会拥有`ParentNode`接口。

### ParentNode.children

`children`属性返回一个`HTMLCollection`实例，成员是当前节点的所有元素子节点。该属性只读。

下面是遍历某个节点的所有元素子节点的示例。

```
for (var i = 0; i < el.children.length; i++) {
  // ...
}
```

注意，`children`属性只包括元素子节点，不包括其他类型的子节点（比如文本子节点）。如果没有元素类型的子节点，返回值`HTMLCollection`实例的`length`属性为`0`。

另外，`HTMLCollection`是动态集合，会实时反映 DOM 的任何变化。

### ParentNode.firstElementChild

`firstElementChild`属性返回当前节点的第一个元素子节点。如果没有任何元素子节点，则返回`null`。

```
document.firstElementChild.nodeName
// "HTML"
```

上面代码中，`document`节点的第一个元素子节点是`<HTML>`。

### ParentNode.lastElementChild

`lastElementChild`属性返回当前节点的最后一个元素子节点，如果不存在任何元素子节点，则返回`null`。

```
document.lastElementChild.nodeName
// "HTML"
```

上面代码中，`document`节点的最后一个元素子节点是`<HTML>`（因为`document`只包含这一个元素子节点）。

### ParentNode.childElementCount

`childElementCount`属性返回一个整数，表示当前节点的所有元素子节点的数目。如果不包含任何元素子节点，则返回`0`。

```
document.body.childElementCount // 13
```