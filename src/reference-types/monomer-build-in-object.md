# 单体内置对象

JS提供了许多的内置对象（真正的内置对象可以直接进行访问 不可以进行 new 实例化），例如Object、Array、Boolean和String，它们不依赖于运行环境。

例如运行在浏览器中，则JS可以访问，DOM对象和BOM对象。

ECMA-262还定义了两个单体内置对象： `Global` 和 `Math`。

## Global对象

“Global（全局）对象可以说是ECMAScript中最特别的一个对象了，因为不管你从什么角度上看，这个对象都是不存在的。ECMAScript中的Global对象在某种意义上是作为一个终极的“兜底儿对象”来定义的。换句话说，不属于任何其他对象的属性和方法，最终都是它的属性和方法。事实上，没有全局变量或全局函数；所有在全局作用域中定义的属性和函数，都是Global对象的属性。本书前面介绍过的那些函数，诸如isNaN()、isFinite()、parseInt()以及parseFloat()，实际上全都是Global对象的方法。除此之外，Global对象还包含其他一些方法。”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）”。 iBooks.

> URI编码

在 [encodeURI](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI) 和 [encodeURIComponent](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent) 出现之前，使用的编码方法是 `escape`

`escape` 已经从 Web 标准中删除，虽然一些浏览器目前仍然支持它，应尽量避免使用。它仅支持 `ASCII` 编码（ASCII 是用来表示英文字符的一种编码规范），不支持中文。

`Unicode` 编码添加了对多种语言的支持。 `encodeURI` 和 `encodeURIComponent` 则支持`Unicode` 编码。

`encodeURI` 和 `decodeURI` 对应。它不会对ajax请求中包含的一些特殊字符的查询参数  "&", "+", 和 "="  编码，因此序列化请求参数一般使用 `encodeURIComponent`。

```javascript

// 序列化查询参数
let encodeUrlParams = params => {
    let queryUrl;
    let keys = Object.keys(params);

    if (keys.length === 0) return '';

    queryUrl = keys.reduce((pre, cur) => `${pre}${encodeURIComponent(cur)}=${encodeURIComponent(params[cur])}&`, '')

    queryUrl = queryUrl.substr(0, queryUrl.length - 1);

    return queryUrl;
}
```

> eval

[eval(string)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval) 可以将传进来的字符串当做JavaScript解析执行。

`eval` 中的可以访问到执行环境外部的变量。同时，在 `eval` 中定义的变量，外部也可以访问到。但是在严格模式下，`eval` 外部访问不到里面定义的变量。

```javascript
let message = 'hellow world~';
eval('var num = 1; function() alMsg{ alert(message) }; alMsg();');

console.log(num);
```

> Global对象属性

Global对象还包含一些属性，值undefined、NaN以及Infinity都是Global对象的属性。此外，所有原生引用类型的构造函数，Object和Function，也都是Global对象的属性。下表列出了Global对象的所有属性：

属性|说明|属性|说明
--|--|--|--
undefined | 空值 undefined | NaN | 特殊的 NaN
Infinity | 正无穷 | Object | 对象构造函数 Object
Array | 数组构造函数 Array | Function | 函数构造函数 Function
String | 字符串构造函数 String | Boolean | 布尔值构造函数 Boolean
Number | 数字构造函数 Number | Date | 日期构造函数 Date
RegExp | 正则构造函数 | Error | 错误对象
EvalError | 关于 eval 函数的错误 | RangeError | 当一个值不在其所允许的范围或者集合中的错误
ReferenceError | 当一个不存在的变量被引用时发生的错误 | SyntaxError | 尝试解析语法上不合法的代码的错误
TypeError | 值的类型非预期类型时发生的错误 | URIError | 一种错误的方式使用全局URI处理函数而产生的错误

> Window对象

当JS运行在浏览器的时候， `Window` 对象即充当了JS的 `Global` 对象。所有全局的对象和定义在全局的变量都可通过 `window.xxx` 来访问。

> Math对象

“ECMAScript还为保存数学公式和信息提供了一个公共位置，即Math对象。与我们在JavaScript直接编写的计算功能相比，Math对象提供的计算功能执行起来要快得多。Math对象中还提供了辅助完成这些计算的属性和方法”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）”。 iBooks.

* Math的属性

属　　性 | 说　　明
-- | --
Math.E | 自然对数的底数，即常量e的值
Math.LN10 | 10的自然对数
Math.LN2 | 2的自然对数
Math.LOG2E | 以2为底e的对数
Math.LOG10E | 以10为底e的对数
Math.PI | π的值
Math.SQRT1_2 | 1/2的平方根（即2的平方根的倒数）
Math.SQRT2 | 2的平方根

* Math的常用方法

函数 | 介绍
Math.min() | 返回给定的数字最小的数值
Math.max() | 返回给定的数字最大的数值
Math.ceil() | 向上取整
Math.floor() | 向下取整
Math.round() | 四舍五入
Math.random() | 0~1之间的随机数

应用:

获取最小的数字

```javascript
let numArr = [1,2,3,4];
consol.log(Math.min(1,2,3,4));

console.log(Math.min.apply(Math, numArr));
```

在某个区间内获取随机数

```javascript
let getRandomLimitByCeil = (min, max) => (Math.ceil(Math.random() * (max - min + 1)) - 1 + min);

let getRandomLimitByFloor = (min, max) => (Math.floor(Math.random() * (max - min + 1)) + min);
```

快速生成获取随机字符串

```javascript
let randomStr = _ => Math.random().toString(36).substr(2);
console.log(randomStr());
```

> JSON对象

`JSON.stringify()` 将一个对象转化为JSON类型的字符串。

`JSON.parse()` 将一个JSON类型的字符串转化为对象。

```javascript
let obj = {
    a: 1,
    b: 1
}

let jsonObj = JSON.stringify(obj);

console.log(jsonObj, typeof jsonObj);
console.log(JSON.parse(jsonObj));
```

作对象的深拷贝

```javascript
let obj = {
    a: 1,
    b: {
        c: 1
    }
}

let deepCloneObj = JSON.parse(JSON.stringify(obj));

deepCloneObj.b.c = 2;

console.log(obj);
console.log(deepCloneObj);
console.log(deepCloneObj !== obj);
```