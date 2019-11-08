# 数据类型

ECMAScript中有5种简单数据类型（也称为基本数据类型）：Undefined、Null、Boolean、Number和String。还有1种复杂数据类型——Object，Object本质上是由一组无序的名值对组成的。ECMAScript不支持任何创建自定义类型的机制，而所有值最终都将是上述6种数据类型之一。乍一看，好像只有6种数据类型不足以表示所有数据；但是，由于ECMAScript数据类型具有动态性，因此的确没有再定义其他数据类型的必要了

## 数据类型检测

### typeof操作符（一元操作符）

`typeof` 可以判断数据的类型。它会返回7种数据类型，并且返回值都是字符串类型

类型 | 结果
-- | --
Undefined | "undefined"
Null | "object"
Boolean | "boolean"´
Number | "number"
String | "string"
Symbol （ECMAScript 6 新增）| "symbol"
函数对象（[[Call]] 在ECMA-262条款中实现了）| "function"

> 使用时，可以用小括号括起来，这样可以更加代码可读性。示例：

```javascript
var a = true;
var b = true;
console.log(typeof a === b);
console.log((typeof a) === b);
```

[typeof-MDN参考链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/typeof)

### instanceof

`instanceof` 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。

```javascript
let A = function(){}
let B = function(){}
let a = new A();

a instanceof A; // Object.getPrototypeOf(a) === A.prototype
a instanceof B; // Object.getPrototypeOf(b) === B.prototype
```

因此我们可以通过判断某个对象的原型链时候是否存在Array、Object、Function 的 prototype 属性，判断数据类型。但是有以下几个缺点：

1. 基础类型不属于对象，因此无法使用 instanceof 判断基础类型，并且总是返回 false。
2. 页面中包含多个window对象时，数据类型判断有误。例如：`[] instanceof window.frames[0].Array` 返回 false

```javascript
let a = 1;
let b = {};
let c = [];
let d = function () {}

a instanceof Number;

c instanceof Array;

d instanceof Function;
```

### Object 的 toString 方法

不同数据类型的变量的 toStrong 方法，返回的字符串是一个固定的值。

```javascript
Object.prototype.toString.call(1) === "[object Number]";
Object.prototype.toString.call('') === "[object String]";
Object.prototype.toString.call(true) === "[object Boolean]";
Object.prototype.toString.call(null) === "[object Null]";
Object.prototype.toString.call(undefined) === "[object Undefined]";
Object.prototype.toString.call({}) === "[object Object]";
Object.prototype.toString.call(function(){}) === "[object Function]";
Object.prototype.toString.call([]) === "[object Array]";
```

## 注意

* 只有对象有属性和方法，基本数据类型是没有方法的。例如：

```javascript
var a = 'abcd';
var b = a.toUpperCase();
a.length = 10;
console.log(a, a.length, b);
```

* 基础数据类型是不可变的，复杂数据类型是可变的
* 任何两个独立的对象都不相等

```javascript
var a = {age:1};
var b = {age:1};
console.log(a === b);
```