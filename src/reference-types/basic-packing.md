# 基本包装类型

基本包装类型是基础类型的数据，在进行方法调用的时候，创建的临时对象。这个临时对象的类型，称为基本包装类型。

类似以下步骤:

```javascript
let str = '123456';
let strSplit = str.split('');

// 相当于
let basicPicking = new String('123456');
let strSplit = basicPicking.split('');
basicPicking = null;
```

这也是基本类型的数据可以调用属性和方法的原因。

在调用完这个临时对象的属性或方法后，会被立即消除。

`undefined` 和 `null` 没有包装对象，所以在访问它们属性或方法的时候会报错。

## Boolean类型

Boolean类型的包装对象，它对 `valueOf` 和 `toString` 方法进行了重写。

`valueOf` 返回原始的布尔值。

`toString` 返回字符串形式的布尔值。

## Number类型

Number类型的包装对象，它对 `valueOf` 、`toString` 和 `toLocaleString` 方法进行了重写。

`valueOf` 返回原始的数值。

`toString` 和 `toLocaleString` 返回字符串形式的数值。

Number类型的包装对象还保存了三个方法：

> [toExponential](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toExponential)

`toExponential()` 方法返回指定数值以指数表示法，表示该数值的字符串。

```javascript
let num = 100;
console.log(num.toExponential());
console.log(+num.toExponential());
```

> [toPrecision](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toPrecision)

`toPrecision()` 方法返回该数值对象，以指定的精度的字符串表示。参数区间为 1-100 整数。

```javascript
let num = 100.123;
// 参数区间为 1-100 整数
console.log(num.toPrecision(1));
console.log(num.toPrecision(3));
console.log(+num.toPrecision(4));
```

> [toFixed](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed)

`toFixed()` 方法返回保留指定小数位数的数值。默认保留 0 个。

```javascript
let pointNum = 100.125;

console.log(pointNum.toFixed());
console.log(pointNum.toFixed(1));
// 后面的小数 4舍5入（每个浏览器表现不一致）
console.log(pointNum.toFixed(2));
console.log(pointNum.toFixed(3));
```

## String类型

String类型的包装对象，在调用 `valueOf` 、 `toString` 和 `toLocaleString` 都返回字符串的原始值。

> [length](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/length)

String类型包装对象都有个 length 属性，表示字符串的长度。即使字符串中包含双字节字符（不是占一个字节的ASCII字符），每个字符也仍然算一个字符。

> [charAt](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/charAt) 和 [charCodeAt](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt)

`charAt` 返回指定位置的字符。

`charCodeAt` 返回指定位置字符的字符编码。

> [endsWith](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith)

此方法用来判断当前字符串是否是以另外一个给定的子字符串“结尾”的，根据判断结果返回 true 或 false。

> [includes](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/includes)

方法用于判断一个字符串是否包含在另一个字符串中，根据情况返回 true 或 false。

> [indexOf](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)

`str.indexOf(searchValue[, fromIndex])` 方法返回调用  String 对象中第一次出现的指定值的索引，开始在 fromIndex进行搜索。

如果未找到该值，则返回-1。

> [slice](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/slice)

`str.slice(beginSlice[, endSlice])` 方法提取一个字符串的一部分，并返回一新的字符串。

> [match](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match)

`str.match(regexp);` 当一个字符串与一个正则表达式匹配时， match()方法检索匹配项。

> [replace](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)

`str.replace(regexp|substr, newSubStr|function)` 方法返回一个由替换值替换一些或所有匹配的模式后的新字符串。模式可以是一个字符串或者一个正则表达式, 替换值可以是一个字符串或者一个每次匹配都要调用的函数。

> [trim](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/Trim)

`str.trim()` 方法会从一个字符串的两端删除空白字符。在这个上下文中的空白字符是所有的空白字符 (space, tab, no-break space 等) 以及所有行终止符字符（如 LF，CR）。

> [toLocaleLowerCase](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/toLocaleLowerCase) 和 [toLocaleUpperCase](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/toLocaleUpperCase)

`str.toLocaleLowerCase()` 将指定字符串转化为小写

`str.toLocaleUpperCase()` 将指定字符串转化为大写

> [split](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)

`str.split([separator[, limit]])` 方法使用指定的分隔符字符串将一个String对象分割成字符串数组，以将字符串分隔为子字符串，以确定每个拆分的位置
