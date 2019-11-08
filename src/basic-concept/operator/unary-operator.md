# 一元操作符

只能操作一个数（表达式）的操作符叫做一元操作符。一元操作符是ECMAScript中最简单的操作符。
<!-- toc -->

## 递增和递减操作符

++ 即为递增，-- 即为递减。这两种操作符，分别有两种使用方式，一个是前置，一个为后置。（首先操作的语句值转化为数字类型，等同于先执行 [Number()](http://localhost:4000/src/basic-concept/data-type/number.html#%E6%95%B0%E5%80%BC%E8%BD%AC%E6%8D%A2)）

> 前置的特点是：先进行递增或递减的操作，再进行其它的运算：

```javascript
var num = 1;
console.log(++num); // num递增，再输出

num = 1;
console.log(--num); // numnum递减，再输出

num = 1;
console.log(--num + 1); // num先递减，再加一

num = 1;
console.log(++num + 1); // num先递增，再加一
```

> 后置的特点是：先运算，在进行递增或递减

```javascript
var num = 1;
console.log(num--); // 先输出，num再递减

num = 1;
console.log(num++); // 先输出，num再递增

num = 1;
console.log(num-- + 1); // 先加一，num再递减

num = 1;
console.log(num++ + 1); // 先减一，num再递增
```

> **如果对小数进行递减/递增的操作，需要把它先转化为整数（[原因可参考](http://localhost:4000/src/basic-concept/data-type/number.html#%E6%B5%AE%E7%82%B9%E6%95%B0%E5%80%BC)）,然后在转化为对应小数**

## 一元加 + 表达式

> 后面为一个 JS语句。它会把紧跟的第一条语句的值转化为数字类型，等同于 [Number()](http://localhost:4000/src/basic-concept/data-type/number.html#%E6%95%B0%E5%80%BC%E8%BD%AC%E6%8D%A2)

```javascript
console.log(+-1);
```

## 一元减 - 表达式

> 后面为一个 JS语句。第一步执行的过程和使用一元加相同，然后对结果进行取反

```javascript
console.log(-'1');
```

## 按位非（NOT）

按位非操作符由一个波浪线（~）表示，执行按位非的结果就是返回数值的反码。按位非是ECMAScript操作符中少数几个与二进制计算有关的操作符之一。
由于按位非是在数值表示的最底层执行操作，因此速度很快。

### 特点

1. 只能处理成整数
2. 数值的范围在带符号 32 位整型之内（因为在规范里称为 ToInt32）

### 按位操作符的效果

> **对任一数值 x 进行按位非操作的结果为 -(x + 1)****对任一数值 x 进行按位非操作的结果为 -(x + 1)**。对非数字类型和NaN，会被转会为0

```javascript
console.log(~-1);
console.log(~-1);
console.log(~[]);
console.log(~{});
console.log(~NaN);
console.log(~'string');
```

### 按位非使用场景

> JS单个 ~ 使用场景，判断数组中是否存在每一项

```javascript
// 方法一：
[1,2,3].indexOf(1) > -1 && console.log('存在');
// 方法二：
~[1,2,3].indexOf(1) &&  console.log('存在');
```

> JS两个 ~~ 使用场景，将数字转化为整数型。并且由于按位运算符表示的最底层执行操作，相对于 parseInt 方法更加快速。

```javascript
// 方法一：
console.log(parseInt(10.123));
console.log(parseInt(-10.123));
// 方法二：
console.log(~~10.123);  // 相当于执行了两遍 -(x + 1)
console.log(~~-10.123);
```

* [按位非-MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators#Bitwise_NOT)
* [更多按位操作符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators)

## 逻辑非运算符

逻辑非操作符由一个叹号（!）表示，可以应用于ECMAScript中的任何值。无论这个值是什么数据类型，这个操作符都会返回一个布尔值。逻辑非操作符首先会将它的操作数转换为一个布尔值，等同于 [Boolean()](http://localhost:4000/src/basic-concept/data-type/boolean.html)，然后再对其求反。

## typeof 运算符

> [参考链接](http://localhost:4000/src/basic-concept/data-type/#typeof%E6%93%8D%E4%BD%9C%E7%AC%A6%EF%BC%88%E4%B8%80%E5%85%83%E6%93%8D%E4%BD%9C%E7%AC%A6%EF%BC%89)

## void 运算符

void 运算符 对给定的表达式进行求值（void expression），然后返回 undefined;

### typeof使用场景

> 可以利用void，将JS引擎，将function关键字识别成函数表达式而不是函数声明（也可用`()`包裹起来），做到函数的立即执行。

```javascriipt
void function(){console.log('立即执行')}();
```

> 阻止 a 链接跳转。

```html
// 在点击跳转的链接以 `javascript: expression`开头的时候，它会执行 expression。如果 expression 的值是一个字符串，它会以 expression 的值替换当前页面的内容。
<a href="javascript: void 0;">不可跳转</a>
```

[参考链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/void)

## delete 运算符

delete 运算符可以删除对象的属性。如果对象的属性存在并且拥有 DontDelete (对象属性的一个内部属性，拥有该内部属性表明该属性不能被删除)，时返回 false(在严格模式下将抛出异常），否则返回 true。

一般不可删除的属性：

* 通关关键字 var function 声明的变量或函数。直接赋值的变量是不包含 DontDelete 属性的

```javascript
var a = 1;
b = 2;
console.log(delete a);
console.log(delete b);
```

* 函数的 arguments 和 参数

* 原型链的属性或方法

### 参考链接

* [delete-MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/delete)
* [深入理解 JavaScript 中的 delete 操作符](http://bubkoo.com/2014/01/23/deep-in-delete/)

## 注意事项

* **常见的 typeof delete void 后面都会跟着一个括号。但是实际当中这些一元运算符是不需要跟小括号的，它们不是方法，加括号是为了方便阅读。**