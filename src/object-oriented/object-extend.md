# 继承

<!-- toc -->

继承：子类可以使用父类的所有功能，并且对这些功能进行扩展。

在JS中实现继承有很多方式，但每一个都有一定的问题，而后随之出现更友好的方式。

## 对象继承对象

### 原型式继承

如果一个新对象想要继承另一个对象 `a` 的属性或方法。

首先可以想到，对象访问属性或方法可以通过原型链 `[[prototype]]` 想上查找。

因此我们可以通过设置新的对象的 `[[prototype]]` 指向 `a` 。

我们可以一个做到这一点。

```javascript
let a = {
    a: 1
}
// 获取可以 指定原型链指向 的对象
function getAssignPrototypeObj(targetObj) {
    function Fn() {};
    Fn.prototype = targetObj;

    return new Fn();
}

let b = getAssignPrototypeObj(a);

console.log(b.a);

// 如果想要一个对象 继承另一个对象的属性和方法
// 可以正进行一次封装
function extendObj(currentObj, targetObj) {
    let tmpObj = getAssignPrototypeObj(targetObj);
    return Object.assign(tmpObj, currentObj);
}

let obj = {
    a: 1
}

let otherObj = {
    b: 2
}

let c = extendObj(obj, otherObj);
console.log(c.b);
```

ES5 中 [Object.create(proto, [propertiesObject])](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create) 也可以实现创建一个新对象，使用现有的对象来提供新创建的对象的 `[[prototype]]` 。

`proto` 新创建对象的原型对象。

`propertiesObject` 这些属性对应Object.defineProperties()的第二个参数，表示新建对象的属性。

```javascript
let a = {
    a: 1
}

let b = Object.create(a);

console.log(b.a);
console.log(Object.getPrototypeOf(b));
```

### 寄生式继承

寄生式继承是在原型继承的基础上并添加了一些属性或方法。

```javascript
let person = {
    name: '小李'
}

function getAssignPrototypeObj(targetObj) {
    function Fn() {};
    Fn.prototype = targetObj;

    return new Fn();
}

// 寄生式继承
function parasiticExtend(targetObj) {
    let tmpObj = getAssignPrototypeObj(targetObj);

    tmpObj.getName = function() {
        console.log(this.name)
    }
    return tmpObj;
};


let obj = parasiticExtend(person);

obj.getName()
```

## 构造函数继承构造函数

### 原型链

对象访问属性或方法可以通过原型链 `[[prototype]]` 想上查找。 因此我们可以通过设置子类 Child 的 `[[prototype]]` 指向父类 `Parent` 的 `prototype` 实现继承。

```javascript
// 父类
function Parent() {
    this.parent = 'parent';
}

// 子类
function Child() {
    this.child = 'child';
}

Child.prototype = new Parent();

Object.defineProperty(Child.prototype, 'constructor', {
    enumerable: false,
    value: Child
})

// 在为子类添加自定义方法时，应在执行 new Parent() 之后
// 因为 new Parent() 会整个改写原型的引用
Child.prototype.getParentName = function () {
    console.log(this.parent)
}

let instance = new Child();

instance.getParentName();
console.log(instance.child)
```

缺点：

1. 因为子类继承父类是通过 `Child.prototype = new Parent()` 。所以实例 `instance` 无法通过传参定义特定的属性值(即无法通过参数，设置 `parent` 的属性值)。
2. 如果父类构造函数 `Parent` this中定义的引用类型的数据。在实例化 `Child` 生成多个对象时，某一个实例对象修改了这个引用类型的数值。由于所有的实例对象共享的这个引用类型的数据，所以其它实例对象也会被影响，

```javascript
// 父类
function Parent() {
    this.arr = [1,2,3];
}

// 子类
function Child() {}

Child.prototype = new Parent();

let instance1 = new Child();
let instance2 = new Child();

instance1.arr.push(4);
console.log(instance2.arr);
```

### 借用构造函数

为了避免实例对象公用引用属性，我们可以让继承的构造函数 `Child` this 中定义父类中相同的属性。

```javascript
// 父类
function Parent() {
    this.arr = [1,2,3];
    this.name = 'parent';
}

// 子类
function Child() {
    this.arr = [1,2,3];
    this.name = 'child';
}
```

为了简化上面的过程，我们可以让父类在子类函数体内运行一遍

```javascript
// 父类
function Parent() {
    this.arr = [1,2,3];
    this.name = 'parent';
}

// 子类
function Child() {
    Parent.apply(this, arguments)
    this.name = 'child';
}
```

这样实例化的对象就不会共享数据了

```javascript
// 父类
function Parent() {
    this.arr = [1,2,3];
    this.name = 'parent';
}

// 子类
function Child() {
    Parent.apply(this, arguments)
    this.name = 'child';
}

let instance1 = new Child();
let instance2 = new Child();

instance1.arr.push(4);
console.log(instance2.arr)
```

缺点：

1. 无法做到共享的属性（例如通用的方法）

### 组合模式

组合模式即 `原型链模式` + `借用构造函数模式`。它即实现了继承时的传参，也实现了通用数据和方法的共享与独有的属性和方法的私有。

```javascript
// 父类
function Parent(name) {
    this.arr = [1,2,3];
    this.name = name;
}

Parent.prototype.getName = function() {
    console.log(this.name)
}

// 子类
function Child() {
    Parent.apply(this, arguments)
}

Child.prototype = new Parent(Child.prototype, 'constructor', {
    value: Child,
    enumerable: false
});

let instance1 = new Child('instance1');
let instance2 = new Child('instance2');

// 属性或方法的共享 及 传参
instance1.getName()
instance2.getName()

// 数据的私有
instance1.arr.push(4);
console.log(instance1.arr)
console.log(instance2.arr)
```

> 缺点：

1. 父类构造函数运行了两遍（在子类函数体内运行一遍、设置子类原型指向父类实例运行了一遍）
2. 父类的属性在子类的 `this` 中有一份，在子类的原型中也有一份（由于同名屏蔽，在实例对象访问属性时，返回的是定义在构造函数 `this` 的属性）

## 寄生组合式继承

寄生组合式继承 即 `寄生式继承` + `原型式继承` + `借用构造函数` 。它解决了 `组合式继承` 的两个缺点。

由于在 `组合式继承` 中子类构造函数必须要在内部执行父类以便获取父类this中定义的内容。我们只能修改 `组合式继承` 中对子类构造函数设置原型的步骤。我们可以利用 `寄生式继承`，设置子类的原型链 `[[prototype]]` 指向父类的 `prototype`。

```javascript
// 获取可以 指定原型链指向 的对象
function getAssignPrototypeObj(prototype) {
    function Fn() {};
    Fn.prototype = prototype;

    return new Fn();
}

// 设置构造函数型链指向
function inheritPrototype(subConstructor, superConstructor) {
    let prototype = getAssignPrototypeObj(superConstructor.prototype);
    Object.defineProperty(prototype, 'constructor', {
        value: subConstructor,
        enumerable: false
    });
    subConstructor.prototype = prototype;
};

// 父类
function Parent(name) {
    this.arr = [1,2,3];
    this.name = name;
}

Parent.prototype.getName = function() {
    console.log(this.name)
}

// 子类
function Child() {
    Parent.apply(this, arguments)
}

inheritPrototype(Child, Parent);

let instance = new Child('instance');
console.log(instance);

// 查看原型链的指向的构造函数
let prototype = Object.getPrototypeOf(instance)
while(prototype) {
    console.log(prototype.constructor);
    prototype = Object.getPrototypeOf(prototype)
}
```