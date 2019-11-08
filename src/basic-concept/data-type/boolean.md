# Boolean类型

Boolean类型是ECMAScript中使用得最多的一种类型，该类型只有两个字面值：true和false。

## 数据转换

> 可以使用`Boolean(x)`（或使用!!）,将 x 转化为布尔值。

* JS中，只有`undefined`、`null`、`0`、`-0`、`""`(空字符串)、`NaN` ，6种数据会被转化为`false`，其余的变量都会转化为`true`。
