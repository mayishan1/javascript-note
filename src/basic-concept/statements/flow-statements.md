# 流程控制语句

<!-- toc -->

## if 语句

条件判断的分支语句。

 if（condition）statement1 else statement2 ( 可以单独使用 `if () statement1` 语句)

其中条件 condition 可以是任意表达式，如果表达式的计算结果转化为布尔值为 `true` 则执行 statement1 ，否则执行 statement2。

无论是 if ()  语句，还是 else 语句，它们都只会执行，紧跟在执行后面的一条一句。如果想要执行多个语句，则需要使用  {} 将多个语句包起来。

```javascript
    // 单个条件判断
    if (true) console.log(1) else console.log(2);

    // 如果需要进行多个条件的判断，可以在 else 后面紧跟着另一条 if 语句
    let num = 1;

    if (num === 1) {
        console.log(1);
    } else if (num === 2) {
        console.log(2)
    } else if (num === 3) {
        console.log(3)
    } else {
        // 这里的 else 是跟着最近的 if 语句 if(num === 3) 对应的
        console.log(4)
    }
```

## switch 语句

switch语句与if语句的关系最为密切，而且也是在其他语言中普遍使用的一种流控制语句。

switch 语句执行中，它会判断 case 后面一条语句的返回值 和 switch 括号内的值做 === 全等比较，如果成立，则执行 case 后面的所有语句。`default` 是在所有 case 都不成立的情况下，默认执行其后面的语句。

```javascript
// 例如下面例子：2 、3、10 都会被打印。
let num = 2;
switch(num) {
    case 1:
        console.log(1);
    case 2:
        console.log(2);
    case 3:
        console.log(3);
    default:
        console.log(10);
}

// 为了避免以上的情况，需要每一个 case 后面的语句结尾，都添加一个中断语句 break，来跳出 switch 语句
switch(num) {
    case 1:
        console.log(1);
        break;
    case 2:
        console.log(2);
        break;
    case 3:
        console.log(3);
        break;
    // default 可以写在 switch 语句的任意位置
    default:
        console.log(10);
}
```

> switch 语句，避免了多个条件的判断场景下，使用多个 `if ... else ...`

## while 语句

while语句属于前测试循环语句，也就是说，在循环体内的代码被执行之前，就会对出口条件求值。以下是while语句的语法：

`while (expression) statements`

```javascript
// 一般会在 statements 中控 expression 返回 false
let numArray = []
while (numArray.length < 100) {
    numArray.push(numArray.length + 1);
}
```

## for 语句

for语句也是一种前测试循环语句，但它具有在执行循环之前初始化变量和定义循环后要执行的代码的能力。以下是for语句的语法：

`for ( initialization; expressin; post-loop-expression ) statement`

执行顺序为 `initialization` -> `expressin` -> `statement` -> `post-loop-expression` -> `expressin` (如果返回 `false`，则终止循环。如果返回 `true` ，则按照 `expressin -> statement -> post-loop-expression` 的顺序执行, `initialization` 只会执行一次)

```javascript

let numArray = []
for (let num = 100; num > 0; numArray.push(num--));
// 由于 var 声明的变量有作用域提升的问题，initialization 初始化变量，尽量使用 let
```

使用 for 注意事项

* expressin 返回值尽量是一个布尔值，避免了再次类型转换
* expressin 中避免对重复访问同一个属性。可以在 initialization 保留一个引用，可以在 expressin 直接访问引用

## for-in 循环

for-in 语句是一种精准的迭代语句，可以用来枚举对象的属性。以下是 for-in 语句的语法

`for（property in expression）statemen`

`Object.getOwnPropertyDescriptor(xxx, key).enumerable` 获取对象的指定属性是否可被枚举

```javascript
let ObjectCreate = function () {
    this.name = '张三';
}
let proto = {
    constructor: ObjectCreate,

    age: 12
};

ObjectCreate.prototype = proto;

Object.defineProperty(proto, 'constructor', {
    enumerable: false
})

let obj = new ObjectCreate();

for (let key in obj) {
    console.log(obj[key])
}

// 获取对象可枚举属性
let index = 0;
let keys = [];
for (keys[index++] in obj);
console.log(keys);
```

for-in 注意事项

* expression 一定是一个对象，如果不是，就会被转化为一个包装对象，在进行枚举。如果值为 null 或 undefined，在 es3 环境下会报错，但是 es5 修正了这一行为。
* ECMAScript对象的属性没有顺序。因此，通过for-in循环输出的属性名的顺序是不可预测的。一般情况下是遵循属性定义的前后顺序，进行枚举。如果存在数字，则优先根据数字的大小顺序进行枚举（正式由于对枚举的顺序有了复杂的判断，所以 fon-in 的性能，比 for 差很多）。
* property 一定是一个左值，即可以被赋值的语句。并且每次属性的枚举，property 都会被执行一遍。
* 在 statemen 如果对对象进行了属性的删除或增加，则该属性不会被枚举

## label 语句

使用label语句可以在代码中添加标签，以便将来使用。label的语法：

`label : statement`

```javascript
// 这个例子中定义的start标签可以在将来由break或continue语句引用。加标签的语句一般都要与for语句等循环语句配合使用。
start: for (let i = 0; i < 10; i++) {
    alert(i)
}
```

## break和continue语句

break和continue语句用于在循环中精确地控制代码的执行。其中，break语句会立即退出循环（for、for-in、switch）），强制继续执行循环后面的语句。而continue语句虽然也是立即退出循环（for、for-in），但退出循环后会从循环的顶部继续执行

```javascript
// 当 num 等于 5 跳出循环
for (let num = 10; num--;) {
    if (num === 5) {
        break;
    }
    console.log(num);
}


// 嵌套的 for 循环，可以通过 break label 的方式，指定推出某个循环，
parent: for (let num1 = 10; num1--;) {

    for (let num2 = 1; num2--;) {
        if (num1 === 5) {
            break parent;
        }
        console.log(num);
    }
}
```

## return 语句

return 语句会终止函数的执行并返回函数的值。如果没有显示的返回一个确切的值，则默认返回 `undefined`

```javascript
var x = myFunction(4, 3);        // 调用函数，将返回值赋予 x 变量

function myFunction(a, b) {
    return a * b;                // 函数返回 a 和 b 的乘积
}
```