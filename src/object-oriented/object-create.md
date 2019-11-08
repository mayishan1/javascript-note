# 创建对象

在创建多个对象时，如果对象中包含相同的属性名和方法。为了更加方便的生成此类对象和节省代码，就出现了很多的创建模式。

## 工厂模式

通过函数内部返回一个新对象的方式。

通过参数来区分不同对象的属性值。

```javascript
let createPerson = (name, age, sex) => ({name, age, sex});
```

> 缺点

无法确定创建出对象的来源（不知道对象时由谁创建的）

## 构造函数

通过实例化构造函数返回新的对象。实例化是，通过参数来区分不同对象的属性值。

```javascript
// 构造函数 函数名默认使用大写字母
function CreatePerson (name, age, sex) {
    this.name = name;
    this.age = name;
    this.sex = name;

    this.getName = _ => console.log(this.name);
}
```

> 特点

1. 没有显式地创建对象
2. 直接将属性和方法赋给了this对象
3. 没有return语句

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）”。 iBooks.

> 构造函数和原型模式

因为 `getName` 方法是通用的，所以为了避免每个生成的对象都有 `getName` 方法，我们可以将 `getName` 方法定义在，构造函数的 `prototype` 属性中。(在访问对象的属性时，如果本地的对象中不存在，它就会沿着对象的原型链向上查找，如果没有找到则返回 undefined)

```javascript
function CreatePerson (name, age, sex) {
    this.name = name;
    this.age = name;
    this.sex = name;
}

CreatePerson.prototype.getName = function () {
    console.log(this.name);
}
```

每个实例化对象都有一个 `[[prototype]]`，它指向原构造函数的 `prototype` 属性。在各个浏览器中通过对象的 `__proto__` 属性实现了 `[[prototype]]` 。

`a.isPrototypeOf(b)` 方法可以判断 `b` 的 `[[prototype]]` 是否指向 `a`。

ES5 中可以 `Object.getPrototypeOf(x)` 获取对象的构造函数的原型。

原构造函数的原型内部有个 `constructor` 指向原构造函数。因此可以区分创建对象的来源是哪一个构造函数。

```javascript
function CreatePerson () {}
function CreateDog () {}

let person = new CreatePerson();
let dog = new CreateDog();

console.log(CreatePerson.prototype.isPrototypeOf(person));
// Object.prototype 在 person 的原型链中，因此也返回 true
console.log(Object.prototype.isPrototypeOf(person));

console.log(Object.getPrototypeOf(person) === CreatePerson.prototype);
console.log(Object.getPrototypeOf(person).constructor === CreatePerson);
```

> 如何判断对象属性是自有属性还是原型中的属性

`Object.getOwnPropertyNames()` 可以获取对象的所有自有属性集合。

`Object.keys()` 以获取对象的所有自有属性中可枚举属性集合。

`Object.hasOwnProperty()` 可以判断属性是否没对象的自有属性。

```javascript
function CreateObj () {
    this.a = 1;
    this.b = 2;
}

CreateObj.prototype.c = 3;

let obj = new CreateObj();

Object.defineProperty(obj, 'b', {
    enumerable: false
})

// 包含不可枚举的属性
console.log(Object.getOwnPropertyNames(obj));

// 不包含不可枚举的属性
console.log(Object.keys(obj));

// 不包含不可枚举的属性
let ownKeys = [];
for (let i in obj) (obj.hasOwnProperty(i) && ownKeys.push(i));

console.log(ownKeys)
```

> 更简单的原型语法

为了避免在原型添加的东西过多，从而多处使用 `CreatePerson.prototype`。为了避免代码重复。可以使用以下方式：

```javascript
function CreateObj () {}

// 每个函数都会有一个 `prototype` 对象，它默认有个 `constructor` 的属性指向 原构造函数。
// 这里需要显示声明 constructor 的指向，并且设置它的属性描述符的 枚举属性 为false
CreateObj.prototype = {
    constructor: CreateObj,
    a: 1,
    conA: function () {
        console.log(this.a);
    }
}

let obj = new CreateObj();

Object.defineProperty(CreateObj.prototype, 'constructor', {
    enumerable: false
})
```

> 原型的动态性 和 动态原型模式

在实例化构造函数后再向构造函数的原型中添加方法，实例化对象可以访问添加的属性，这就是原型的动态性。原因是实例化对象保存了访问原型的指针 `[[prototype]]`。

如果实例化构造函数后更改整个构造函数的原型，则实例对象的原型链指向的还是构造函数更改前的原型。

```javascript
function CreateObj() {}

let obj = new CreateObj();

// 修改构造函数原型
CreateObj.prototype.name = '小李';

// 在实例化构造函数后可以访问  构造函数对原型的修改
console.log(obj.name);

let anotherObj = new CreateObj();

// 更改整个构造函数的原型
CreateObj.prototype = {
    name: '小名'
}

// 实例对象的原型链指向的还是构造函数更改前的原型
console.log(anotherObj.name);

console.log(Object.getPrototypeOf(anotherObj) === Object.getPrototypeOf(obj));

console.log(CreateObj.prototype)
```

因此衍生出了 `动态原型模式` 。它可以在构造函数进行实例化时在动态对构造函数原型添加方法。节省了内存。

```javascript
function CreateObj() {
    this.name = '小李';

    if (typeof this.conName !== 'function') {
        console.log('第一次实例化时执行~');
        CreateObj.prototype.conName = () => {
            console.log(this.name)
        }
    }
}

new CreateObj();
new CreateObj();
```

> 寄生构造函数模式

使用实例化构造函数，可以让构造函数内的 this 指向实例化的对象。因此可以创建一些具有特定方法的数据。

```javascript
function CreatePrefixArray() {
    let arr = [...arguments];

    arr.prefixItem = function (prefix) {
        return this.map(i => `${prefix}${i}`)
    }

    return arr;
}

let numberArr = new CreatePrefixArray(1, 2, 3, 4, 5);

console.log(numberArr.prefixItem('number-'));
```