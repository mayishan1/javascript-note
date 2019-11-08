# 函数

函数对任何语言来说都是一个核心的概念。通过函数可以`封装任意多条语句`，而且可以在任何地方、任何时候调用执行。ECMAScript中的函数使用function关键字来声明，后跟一组参数以及函数体。所有的函数都会有一个返回值，如果没有显示 return，则返回 undefined。

es3 `function functionName(arg0, arg1, ···, argN) { statements }`

es6 `let functionName = (arg0, arg1, ···, argN) => { statements }`

函数的行参（函数定义是声明的参数名）是用来方便使用，函数调用时传递的实参（函数调用传递的变量）。如果不定义行参，可以通过函数内部一个对象 arguments 访问实参。

arguments 是一个伪数组。它的属性是从 0 开始，按照数字的顺序排列。最大值为实参的个数。arguments[0] 即为第一个实参。

如果形参数量大于实参，则多出来的行参值为 `undefined`。如果实参大于行参，多余可通过 arguments 进行访问。

ECMAScript中的所有参数传递的都是值，不可能通过引用传递参数。如果传递的参数是个对象，如果在函数中对其进行了修改，则外部的那个对象也会被修改。这是由于函数内部对值进行访问时，和外部的对象执行同一个地址。

```javascript
function add(a, b) {
    return a + b;
}

function argAdd() {
    return arguments[0] + arguments[1];
}

// 尖头函数的函数体内没有 arguments 对象
const addArrow = (a, b) => a + b;
```

## 函数传参

ECMAScript中所有函数的参数都是按值传递的。也就是说，把函数外部的值复制给函数内部的参数，就和把值从一个变量复制到另一个变量一样。

在向参数传递基本类型的值时，被传递的值会被复制给一个局部变量（即命名参数，或者用ECMAScript的概念来说，就是arguments对象中的一个元素）。

在向参数传递引用类型的值时，会把这个值在内存中的地址复制给一个局部变量，因此这个局部变量的变化会反映在函数的外部。

```javascript
let num = 1;
let numObj = {
    num: 1
}

let numFn = num => num = num + 1;
let numObjFn = numObj => numObj.num  = 2;

// 基础类型：函数内部的修改不会影响到外部
numFn(num);
console.log(num);
// 引用类型：函数内部的修改影响到外部
numObjFn(numObj);
console.log(numObj);
```

## 单例模式

```javascript

let singleton = (function() {
    function TmpFn() {
        this.instance = null;
    }

    TmpFn.prototype.getInitance = function(name = '') {
        if (!this.instance) {
            this.instance = {
                name
            };
        }

        return this.instance;
    }

    return new TmpFn;
}());

console.log(singleton.getInitance())
console.log(singleton.getInitance() === singleton.getInitance())
```