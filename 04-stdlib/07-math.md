# Math 对象

`Math`是 JavaScript 的原生对象，提供各种数学功能。该对象不是构造函数，不能生成实例，所有的属性和方法都必须在`Math`对象上调用。

## 静态属性

`Math`对象的静态属性，提供以下一些数学常数。

- `Math.E`：常数`e`。
- `Math.LN2`：2 的自然对数。
- `Math.LN10`：10 的自然对数。
- `Math.LOG2E`：以 2 为底的`e`的对数。
- `Math.LOG10E`：以 10 为底的`e`的对数。
- `Math.PI`：常数`π`。
- `Math.SQRT1_2`：0.5 的平方根。
- `Math.SQRT2`：2 的平方根。

```
Math.E // 2.718281828459045
Math.LN2 // 0.6931471805599453
Math.LN10 // 2.302585092994046
Math.LOG2E // 1.4426950408889634
Math.LOG10E // 0.4342944819032518
Math.PI // 3.141592653589793
Math.SQRT1_2 // 0.7071067811865476
Math.SQRT2 // 1.4142135623730951
```

这些属性都是只读的，不能修改。

## 静态方法

`Math`对象提供以下一些静态方法。

- `Math.abs()`：绝对值
- `Math.ceil()`：向上取整
- `Math.floor()`：向下取整
- `Math.max()`：最大值
- `Math.min()`：最小值
- `Math.pow()`：幂运算
- `Math.sqrt()`：平方根
- `Math.log()`：自然对数
- `Math.exp()`：`e`的指数
- `Math.round()`：四舍五入
- `Math.random()`：随机数

### Math.abs()

`Math.abs`方法返回参数值的绝对值。

```
Math.abs(1) // 1
Math.abs(-1) // 1
```