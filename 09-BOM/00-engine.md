# 浏览器环境概述

JavaScript 是浏览器的内置脚本语言。也就是说，浏览器内置了 JavaScript 引擎，并且提供各种接口，让 JavaScript 脚本可以控制浏览器的各种功能。一旦网页内嵌了 JavaScript 脚本，浏览器加载网页，就会去执行脚本，从而达到操作浏览器的目的，实现网页的各种动态效果。

本章开始介绍浏览器提供的各种 JavaScript 接口。首先，介绍 JavaScript 代码嵌入网页的方法。

## 代码嵌入网页的方法

网页中嵌入 JavaScript 代码，主要有四种方法。

- `<script>`元素直接嵌入代码。
- `<script>`标签加载外部脚本
- 事件属性
- URL 协议

### script 元素嵌入代码

<script>元素内部可以直接写入 JavaScript 代码。

```
<script>
  var x = 1 + 5;
  console.log(x);
</script>
```

<script>标签有一个type属性，用来指定脚本类型。对 JavaScript 脚本来说，type属性可以设为两种值。

- `text/javascript`：这是默认值，也是历史上一贯设定的值。如果你省略`type`属性，默认就是这个值。对于老式浏览器，设为这个值比较好。
- `application/javascript`：对于较新的浏览器，建议设为这个值。

```
<script type="application/javascript">
  console.log('Hello World');
</script>
```

由于`<script>`标签默认就是 JavaScript 代码。所以，嵌入 JavaScript 脚本时，`type`属性可以省略。

如果`type`属性的值，浏览器不认识，那么它不会执行其中的代码。利用这一点，可以在`<script>`标签之中嵌入任意的文本内容，只要加上一个浏览器不认识的`type`属性即可。

```
<script id="mydata" type="x-custom-data">
  console.log('Hello World');
</script>
```

上面的代码，浏览器不会执行，也不会显示它的内容，因为不认识它的`type`属性。但是，这个`<script>`节点依然存在于 DOM 之中，可以使用`<script>`节点的`text`属性读出它的内容。

```
document.getElementById('mydata').text
//   console.log('Hello World');
```

### script 元素加载外部脚本

<script>标签也可以指定加载外部的脚本文件。

```
<script src="https://www.example.com/script.js"></script>
```

如果脚本文件使用了非英语字符，还应该注明字符的编码。

```
<script charset="utf-8" src="https://www.example.com/script.js"></script>
```

所加载的脚本必须是纯的 JavaScript 代码，不能有`HTML`代码和`<script>`标签。

加载外部脚本和直接添加代码块，这两种方法不能混用。下面代码的`console.log`语句直接被忽略。

```
<script charset="utf-8" src="example.js">
  console.log('Hello World!');
</script>
```

为了防止攻击者篡改外部脚本，`script`标签允许设置一个`integrity`属性，写入该外部脚本的 Hash 签名，用来验证脚本的一致性。

```
<script src="/assets/application.js"
  integrity="sha256-TvVUHzSfftWg1rcfL6TIJ0XKEGrgLyEq6lEpcmrG9qs=">
</script>
```

上面代码中，`script`标签有一个`integrity`属性，指定了外部脚本`/assets/application.js`的 SHA256 签名。一旦有人改了这个脚本，导致 SHA256 签名不匹配，浏览器就会拒绝加载。