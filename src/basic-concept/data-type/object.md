# Object类型

ECMAScript中的对象其实就是一组数据和功能的集合。

## 与基础类型的不同

* 具有属性
* 具有方法
* 数据可变

> 基础类型之所以访问属性或方法，JS临时创建了一个由基础类型转化而来的包装对象，在执行完属性的访问或方法之后，该包装对象就被销毁了

## JS部分对象

本地对象

* [Error 错误对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Error)
* [Boolean 布尔对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Boolean)
* [String 字符串对象](https://developer.mozilla.org/cn/docs/Web/JavaScript/Reference/Global_Objects/String)
* [Number 数字对象](https://developer.mozilla.org/cn/docs/Web/JavaScript/Reference/Global_Objects/Number)
* [Array 数组对象](https://developer.mozilla.org/cn/docs/Web/JavaScript/Reference/Global_Objects/Array)
* [Date 时间对象](https://developer.mozilla.org/cn/docs/Web/JavaScript/Reference/Global_Objects/Date)
* [Object Object对象](https://developer.mozilla.org/cn/docs/Web/JavaScript/Reference/Global_Objects/Object)
* [Function 函数对象](https://developer.mozilla.org/cn/docs/Web/JavaScript/Reference/Global_Objects/Function)
* [RegExp 正则对象](https://developer.mozilla.org/cn/docs/Web/JavaScript/Reference/Global_Objects/RegExp)

内置对象

* [Math 数学相关对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math)
* [Global 全局对象](https://msdn.microsoft.com/zh-cn/library/52f50e9t(v=vs.94).aspx)
* [JSON JSON对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON)

## 对象转换

基本类型转换为对象，返回值都是一个对象，它们的内容分别包含了：

* Object(Boolean) 包含:一个原始值，值为 Boolean
* Object(Number) 包含:一个原始值，值为 Number
* Object(String) 包含:一组 key 为字符串索引， value 为字符串索引对应值的一组键值对、length属性，值为字符串长度、原始值，值为 String
* Object(null)/Object(undefined) 一个空对象

对象转换为基本类型

* Boolean(Object)，总是返回 true
* Number(Object)，首先是调用对象的 valueOf() 方法，返回结果。如果返回不是一个数字，则接着调用 toString() 方法，返回结果

```javascript
var num = Number([]);   // 返回0
var num1 = Number([2]);   // 只有一个，则返回数组第 0 项的 Number(Array[0])的值
var num2 = Number([2,1,2]);   // 多个返回 NaN
var num3 = Number({});  // NaN
```

* String(Object)，首先是调用对象的 toString() 方法，返回结果。如果返回的是不是一个字符串，则接着调用 valueOf() 方法，返回结果

```javascript
var str = String([1，2]);   // 返回"1，2"。将数组的每一项进行 String()，然后把所有结果拼接起来，中间用逗号分割。如果是空数组，返回 ""
var str1 = String({});  // 返回 [object, object]
var str2 = String(function(){});    // 原样输出 "function(){}"
```

* 对象转换为原始值，首先会调用对象的 valueOf 方法，如果返回结果不是原始值，则在调用 toString 方法。（如果是时间对象，则首先会调用对象的 toString 方法）

对象转化为数组

* 如果对象属性不含有 length 属性，则转换为 [] 数组，如果含有 length 属性，则转换的数组长度等于 length 的值。并且对象中，以小于 length 的属性值与数组的下标对应的值相同，其余用 undefined 补充

```javascript
let obj = {
    0: 1,
    2: 1,
    length: 3
}

// es3
Array.prototype.slice.call(obj, 0, 3);

// es6
Array.from(obj)
```

## 声明对象

* 对象直接量

```javascript
// 在通过对象字面量定义对象时，实际上不会调用Object构造函数
var obj = {
    age: 11,
    name: '张三'
}

var arr = [11, '张三']
```

* new 操作符

```javascript
var obj = new Object;   //不传值，可以省略括号
var obj1 = new Object({like: 'basketball'})
```

* [Object.create(proto, propertiesObject)。](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create) proto 新创建对象的原型对象、propertiesObject 是添加到新创建对象，可枚举属性对象的属性名称和属性描述符

```javascript
var father = {
    eyes: '双眼皮'
}

var son = Object.create(father);    // son的原型链，即 son.__proto__ 指向了 father 对象
console.log(son.eyes);

var o = Object.create({},  {
    bar: {
        configurable: false,
        get: function() { return 10 },
        set: function(value) {
            console.log("Setting `o.bar` to", value);
        }
    }
})
```

## 属性访问

可以通过 `object.属性名` 和 `object[属性名]` 方式进行访问。JS解释器，在遇到 . 或者 []（[] 里面的值会被转化为字符串） 时：

1. 首先判断前面的字符是否是 undefined、null，是则报错。
2. 是对象，然后根据后面的属性名进行取值。
3. 不是，则尝试将它们前面的字符转化为对象，然后根据后面的属性名进行取值。

## 对象的原型

```javascript
function F() {};
function FCopy() {};
F.prototype.name = '小李';
let o = new F('xiaoli');
o.age = 13;

Object.defineProperties(o, {
    'sex': {
        value: '男',
        enumerable: false
    }
})
```

* `constructor`  保存着用于创建当前对象的函数。

```javascript
o.constructor === F;
```

* `hasOwnPropery` xx.hasOwnPropery(x) 检测传入的属性 x，在对象的实例(xx)中是否存在，则不是原型链中

```javascript
o.hasOwnProperty('age');
o.hasOwnProperty('name');
```

* `isPrototypeOf`  xx.isPrototypeOf(x) 检测传入的对象x的原型链，是否是 xx，它会沿着原型链查找

```javascript
F.prototype.isPrototypeOf(o);
FCopy.prototype.isPrototypeOf(o);
Object.prototype.isPrototypeOf(o);
```

* `propertyIsEnumerable` 判断对象属性，是否可以被枚举。例如使用for-in 是都可以拿到

```javascript
o.propertyIsEnumerable('age');
o.propertyIsEnumerable('sex');
```

* `toLocaleString` 方法返回一个该对象的字符串表示。此方法被用于派生对象为了特定语言环境的目的（locale-specific purposes）而重载使用。

    `Object` `toLocaleString` 返回调用 `toString()` 的结果。

    有些对象覆盖了 `Object.toLocaleString` 方法

  * Array: `Array.prototype.toLocaleString()`
  * Number: `Number.prototype.toLocaleString()`
  * Date: `Date.prototype.toLocaleString()`

```javascript
var num = 123456789;
var str = 123456789;
num.toLocaleString();
num.toString();

str.toLocaleString();
str.toString();
```

* `valueOf()` 返回调用者自身

* `toString()` 与 [String()](http://localhost:4000/src/basic-concept/data-type/string.html#%E6%95%B0%E6%8D%AE%E8%BD%AC%E6%8D%A2) 方法相同

## 对象的比较

不同于基本类型，基本类型是对值进行比较，对象是对引用进行比较。在赋值语句中，如果右侧为一个对象，则赋值的是一个引用。因此，修改了对象的属性，另一个变量取对象的某个属性值时，是改变之后的。

```javascript
var a = {
    c:1
}
var b = {
    c:1
}
console.log(a === b);

var c = a;
c.c = 2;
console.log(a);
```