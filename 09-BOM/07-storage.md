# Storage 接口

## 概述

Storage 接口用于脚本在浏览器保存数据。两个对象部署了这个接口：`window.sessionStorage`和`window.localStorage`。

`sessionStorage`保存的数据用于浏览器的一次会话（session），当会话结束（通常是窗口关闭），数据被清空；`localStorage`保存的数据长期存在，下一次访问该网站的时候，网页可以直接读取以前保存的数据。除了保存期限的长短不同，这两个对象的其他方面都一致。

保存的数据都以“键值对”的形式存在。也就是说，每一项数据都有一个键名和对应的值。所有的数据都是以文本格式保存。

这个接口很像 Cookie 的强化版，能够使用大得多的存储空间。目前，每个域名的存储上限视浏览器而定，Chrome 是 2.5MB，Firefox 和 Opera 是 5MB，IE 是 10MB。其中，Firefox 的存储空间由一级域名决定，而其他浏览器没有这个限制。也就是说，Firefox 中，`a.example.com`和`b.example.com`共享 5MB 的存储空间。另外，与 Cookie 一样，它们也受同域限制。某个网页存入的数据，只有同域下的网页才能读取，如果跨域操作会报错。

## 属性和方法

Storage 接口只有一个属性。

- `Storage.length`：返回保存的数据项个数。

```
window.localStorage.setItem('foo', 'a');
window.localStorage.setItem('bar', 'b');
window.localStorage.setItem('baz', 'c');

window.localStorage.length // 3
```

该接口提供5个方法。

### Storage.setItem()

`Storage.setItem()`方法用于存入数据。它接受两个参数，第一个是键名，第二个是保存的数据。如果键名已经存在，该方法会更新已有的键值。该方法没有返回值。

```
window.sessionStorage.setItem('key', 'value');
window.localStorage.setItem('key', 'value');
```

注意，`Storage.setItem()`两个参数都是字符串。如果不是字符串，会自动转成字符串，再存入浏览器。

```
window.sessionStorage.setItem(3, { foo: 1 });
window.sessionStorage.getItem('3') // "[object Object]"
```

上面代码中，`setItem`方法的两个参数都不是字符串，但是存入的值都是字符串。

如果储存空间已满，该方法会抛错。

### Storage.getItem()

`Storage.getItem()`方法用于读取数据。它只有一个参数，就是键名。如果键名不存在，该方法返回`null`。

```
window.sessionStorage.getItem('key')
window.localStorage.getItem('key')
```

键名应该是一个字符串，否则会被自动转为字符串。

### Storage.removeItem()

`Storage.removeItem()`方法用于清除某个键名对应的键值。它接受键名作为参数，如果键名不存在，该方法不会做任何事情。

```
sessionStorage.removeItem('key');
localStorage.removeItem('key');
```