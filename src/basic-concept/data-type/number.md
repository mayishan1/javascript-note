# Number

Number类型应该是ECMAScript中最令人关注的数据类型了，这种类型使用[IEEE754格式](https://zh.wikipedia.org/zh-hans/IEEE_754)来表示整数和浮点数值（浮点数值在某些语言中也被称为双精度数值）。
不同的数值字面量格式:

* 最基本的数值字面量格式是十进制整数，十进制整数可以像下面这样直接在代码中输入：

```javascript
var number = 10;
```

* 八进制字面量在严格模式下是无效的，会导致支持的JavaScript引擎抛出错误。应避免使用
* 十六进制字面值的前两位必须是0x，后跟任何十六进制数字（0~9及A~F）。其中，字母A~F可以大写，也可以小写。如下面的例子所示：

```javascript
var number = 0xA; // 十进制的10
```

**在进行算术计算时，所有以八进制和十六进制表示的数值最终都将被转换成十进制数值，平时开发尽量使用十进制**
科学计数法

* 对于那些极大或极小的数值，可以用e表示法（即科学计数法）表示的浮点数值表示。用e表示法表示的数值等于e前面的数值乘以10的指数次幂。

```javascript
var number1 = 3.125e7; //等于31250000
var number2 = 3e-17; // 等于0.00000000000000003
```

## 浮点数值

* 所谓浮点数值，就是该数值中必须包含一个小数点，并且小数点后面必须至少有一位数字。
* 浮点数值需要的内存空间是保存整数值的两倍，因此ECMAScript会不失时机地将浮点数值转换为整数值。例如：

```javascript
var a = 10.00000;
console.log(a);
```

* **关于浮点数值计算会产生舍入误差的问题，有一点需要明确：这是使用基于IEEE754数值的浮点计算的通病，ECMAScript并非独此一家；其他使用相同数值格式的语言也存在这个问题。（计算机数据都是以二进制表示，某些浮点数转换二进制有误差，因此`0.1 + 0.2 !== 0.3`，因此计算时可以将浮点数转换为整数型）**

```javascript
var num1 = 17.45 * 0.9 * 3;
var num2 = 17.45 * 3 * 0.9;
console.log(num1 !== num2);
```

## 数值范围

由于内存的限制，ECMAScript并不能保存世界上所有的数值。

`Number.MAX_VALUE`和`Number.MIN_VALUE`,分别表示浏览器支持的`Infinity`（最大数值）和`-Infinity`（最小数值）。

`isFinite()`可以判断数字，是否位于最大值和最小值之间，是则返回true，否则返回false

**任何非0数字除以0，返回Infinity或-Infinity，0/0返回`NaN`**

## NaN

NaN，即非数值（Not a Number）是一个特殊的数值，这个数值用于表示一个本来要返回数值的操作数未返回数值的情况（这样就不会抛出错误了）。

* NaN和任何数值参与计算，都会返回NaN

* JS中，只有NaN和自身判断是否全等，返回`false`。

针对NaN的这两个特点，ECMAScript定义了isNaN()函数。这个函数会尝试将一个参数，转化为一个数字，如果结果是一个数字，则返回false，如果不是则返回true。因此这个函数的真正目的是判断某个变量是否可以转换为数字，而不是判断某个数值是否为NaN。（可以使用`xxx !=== xxx`，来判断数字是否为NaN）

> **在基于对象调用isNaN()函数时，会首先调用对象的valueOf()方法，然后确定该方法返回的值是否可以转换为数值。如果不能，则基于这个返回值再调用toString()方法，再测试返回值。（这是一个将对象转换为原始值的过程）**

## 数值转换

有3个函数可以把非数值转换为数值：Number()、parseInt()和parseFloat()。第一个函数，即转型函数Number()可以用于任何数据类型，而另两个函数则专门用于把字符串转换成数值。

### Number(x)

> 将 x 转化为数字类型:

* 如果 Boolean 值，true 和 false 将分别被转换为 1 和 0。
* 如果是数字值，只是简单的传人和返回。
* 如果是 null 值，返回 0，undefined，返回 NaN
* 如果字符串中只包含数字（包括前面带加号或头号的情况），则将其转换为十进制数值，即 “1” 会变成 1，“123” 会变成 123，而 “011” 会变成 11（注意：前导的零被忽略了）；
* 如果字符串是空的（不包含任何字符），则将其转换为 0；
* 如果是数组，首先会调用数组的valueOf 方法，在调用 toString 方法，最后再把字符串转回为数字类型。即 Number([]) === 0、Number([10]) === 10、Number([1,2,3]) !== NaN

[参考](https://javascript.ruanyifeng.com/grammar/conversion.html#toc1)

### parseInt(string, radix)

> 可解析一个字符串，并返回一个整数，radix 表示解析 string 的基数（进制）

* 它会逐个解析字符，直到解析完所有后续字符或者遇到了一个非数字字符结束（包括小数点）。例如：`parseInt('1a1') === 1`
* 我们建议无论在什么情况下都明确指定基数。

```javascript
// 常见的面试题
[10,10,10,10].map(parseInt);

// 这里就是在考 parseInt 方法第二个参数的作用。
```

[parseInt-MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)

### parseFloat(string)

> 函数解析 string 并返回一个浮点数。

* 它会逐个解析字符，直到解析完所有后续字符或者遇到了一个非数字字符结束（不包括第一个小数点）。例如：`parseFloat('1.1.1a1') === 1.1`
* 忽略字符串前导 0。

[parseFloat-MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseFloat)
