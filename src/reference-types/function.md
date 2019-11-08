# Function类型

函数有独立的作用域。

函数相当于是将JS语句和表达式封装到一起，可以在不同的地方进行调用。

## 函数声明

> 函数声明语句

`function fnName () { 函数体... }`

这里的函数名必须的声明的。可以通过函数名获取函数。

函数声明在代码的预解析阶段，会被提前。因此如果在函数声明前调用该函数，则在函数执行阶段，调用该函数就可以正常运行。

> 函数表达式(匿名函数表达式)

`let fn = function () {}`

es6 声明匿名函数的方式：

`let fnArrow = _ => {}`

这里 `=>` 前面的参数如果是一个可以省略小括号。后面的语句如果只有一条，可以省略大括号，并且将改语句的结果作为函数的返回值。

匿名函数不存在声明提前，因此不可以在函数声明前调用。

如果右侧的声明的函数有名字，则该函数名，只可以在函数体内访问。因此可以在使用到递归的通过函数名调用自身。

```javaScript
let fn = function tmp() {
    console.log(tmp);
}

// 函数的 name 属性，储存了函数的名称
console.log(fn.name);

// 函数表达式无法通过函数名访问到函数
console.log(tmp);
```

> new Function()

`const sum = new Function（"num1", "num2", "return num1 + num2");`

这种语法会导致解析两次代码（第一次是解析常规ECMAScript代码，第二次是解析传入构造函数中的字符串），从而影响性能。这里调用了 `eval` 函数，会让浏览器JS解释器在解析JS的时候，由于对eval内容的不确定，而启用 `slow path` 模式，这种方式比较慢，从而影响性能。

## 函数内部属性

> arguments

这个是一个类数组对象，里面储存了调用函数的参数。参数的传递顺序，可以 `arguments` 的属性 0 ~ N 去访问，并且它有一个 length 属性，可以得到参数的个数。

es6 的箭头函数，函数内部没有 arguments 对象。

`arguments` 可以通过 `Array.prototype.slice.call(arguments)` 转化为数组。

es6 可以通过 `Array.form(arguments)` 将 `arguments` 转化为对象。

```javascript
function sum() {
    let arrArgs = Array.prototype.slice.call(arguments);
    let num = arrArgs.reduce((pre, cur) => (pre + cur), 0);

    return function () {
        if (arguments.length === 0) {
            return num;
        } else {
            let arrArgs = Array.from(arguments);
            num = arrArgs.reduce((pre, cur) => (pre + cur), num);
        }

        return arguments.callee;
    };
}

console.log(sum(1)(2)(3,4)())
```

> this

使用 `function` 声明的函数，在调用时会初始化 `this` 的指向。

* 直接调用函数

直接调用的函数， `this` 总会指向 `window`。如果是严格模式，则会指向 `undefined`

```javascript
function fn() {
    console.log(this);
}
fn();
```

严格模式

```javascript
'use strict';
function strictFn() {
    console.log(this);
}
strictFn();
```

* 作为对象的属性进行调用

函数中的 `this` 指向，直接调用函数的对象。即 函数 `.` 操作符左侧的对象。

```javascript
function fn() {
    console.log(this.num)
}

let a = {
    num: 1,
    fn,
    b: {
        num: 2,
        fn
    }
}

a.fn();
a.b.fn();
```

* 构造函数

实例化构造函数 `this` 指向实例化构造函数返回的对象。

实例化构造函数的 `new` 操作符，会做三件事：

1. 创建一个 原型指向指向 构造函数 prototype 的对象
2. 使用指定的参数调用狗在函数，并将 this 指向 新创建的对象。
3. 若构造函数内没有显示的返回一个对象，则默认将步骤一创建的对象返回。

```javascript
// 构造函数一般使用大写字母开头
function Fn() {
    this.num = 1;
}

Fn.prototype.conNum = function() {
    console.log(this.num);
}

const newFn = new Fn();

newFn.conNum();
```

如果是箭头函数。函数在调用时初始化时，不会创建 `this`，箭头函数中的 `this` 在创建时，默认使用它的创建环境中 `this` 的指向。

```javascript

const outsideFn = () => console.log(this);

const obj = {
    fn: outsideFn
}

function fn() {
    // inslideFn 默认使用 fn this 的指向
    const inslideFn = () => console.log(this);
    inslideFn();
}

obj.fn();   // this 指向 window
fn();   // 因为是直接调用， fn 中 this 指向 window 。 inslideFn 默认使用 fn 的 this 指向，则 inslideFn 函数体内的 this 也指向 window

fn = fn.bind(fn);

fn();   // 此时 fn 的 this 指向 fn，则 inslideFn 中 this 指向 fn
```

## 修改 this 指向

> call

`Function.prototype.call(thisArg, arg1, arg2, ...)`

`thisArg` 表示 函数执行时 `this` 的指向。如果是 非严格模式，传 `undefined` 或 `null`，则 `this` 默认指向 `window`

`arg1, arg2, ...` 表示 函数参数列表

```javascript
const obj = {
    num: 1
}

function fn(otherNumber) {
    console.log(this.num);
    console.log(otherNumber);
}

fn.call(obj, 2)
```

模拟 call 简单实现

```javascript
Function.prototype.likeCall = function(target, ...args) {
    target.fn = this;
    target.fn(...args);
    delete target.fn;
}
```

> apply

`Function.prototype.apply(thisArg, [argsArray])`

`thisArg` 同 `call` 的 `thisArg`

`argsArray` 表示 函数参数列表。不同于 `call`，它支持数组形式的传参

```javascript
const obj = {
    num: 1
}

function fn(otherNumber) {
    console.log(this.num);
    console.log(otherNumber);
}

fn.apply(obj, [2])
```

模拟 apply 简单实现

```javascript
Function.prototype.likeApply = function(target, args) {
    target.fn = this;
    target.fn(...args);
    delete target.fn;
}
```

> bind

`bind` 函数返回一个新的函数。

`Function.prototype.bind(thisArg[, arg1[, arg2[, ...]]])`

`thisArg`: 当绑定函数被调用时，该参数会作为原函数运行时的 this 指向。当使用new 操作符调用绑定函数时，该参数无效。

`arg1, arg2, ...`: 当绑定函数被调用时，这些参数将置于实参之前传递给被绑定的方法。

```javascript
const obj = {
    num: 1
}

function fn(oneNumber, twoNumber) {
    console.log(this.num);
    console.log(oneNumber, twoNumber);
}

const bindFn = fn.bind(obj, 2);

bindFn(3);
```

使用 `apply` 模拟实现 `bind`

```javascript

Function.prototype.linkBind = function() {
    let fn = this;
    let arrArgs = Array.from(arguments);
    let thisTarget = arrArgs[0];
    let args = arrArgs.slice(1);
    return function () {
        let fnArgs = args.concat(Array.from(arguments))

        fn.apply(thisTarget, fnArgs);
    }
}

const obj = {
    num: 1
}

function fn(oneNumber, twoNumber) {
    console.log(this.num);
    console.log(oneNumber, twoNumber);
}

const bindFn = fn.linkBind(obj, 2);

bindFn(3);
```

## 立即执行函数

> 避免全局命名冲突

```javascript
var a = 1;
(function() {
    var a = 1;
})()
```

> 模块化-UMD 规范

```javascript
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD
        define(['jquery', 'underscore'], factory);
    } else if (typeof exports === 'object') {
        // Node, CommonJS之类的
        module.exports = factory(require('jquery'), require('underscore'));
    } else {
        // 浏览器全局变量(root 即 window)
        root.returnExports = factory(root.jQuery, root._);
    }
}(this, function ($, _) {
    //    方法
    function a(){};    //    私有方法，因为它没被返回 (见下面)
    function b(){};    //    公共方法，因为被返回了
    function c(){};    //    公共方法，因为被返回了

    //    暴露公共方法
    return {
        b: b,
        c: c
    }
}));
```