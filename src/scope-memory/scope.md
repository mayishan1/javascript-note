# 执行环境和作用域

## 执行环境

> 概念：
执行环境定义了变量或函数有权访问的其他数据，决定了它们各自的行为。

每个执行环境都有一个与之关联的变量对象（variable object），环境中定义的所有变量和函数都保存在这个对象中。在 JavaScript 中，全局执行环境的变量对象 `variable object` 即 window，而函数的执行环境使用 `activation object`(它是由 arguments 创建，全局的变量对象不存在此属性) 充当变量对象。

某个执行环境中的所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数定义也随之销毁（全局执行环境直到应用程序退出——例如关闭网页或浏览器——时才会被销毁）

> 执行流：

1. 每个函数都有自己的执行环境。当执行流进入到函数时，函数的执行环境就会被推入到执行环境栈中，
2. 当执行流进入到函数时，会创建一个作用域链。作用域链用于维护执行环境对有权限的变量及函数有序访问。函数的的变量对象即`activation object`,它起初由 arguments 创建。作用域链的下一个变量来自包含它的执行环境，而下一个变量来自下一个包含它的执行环境，直至到全局的执行环境。
3. 当函数执行完，函数的执行环境就会被弹出。

## 作用域（scope chain）

简单的说，作用域就是变量与函数的可访问范围，即作用域控制着变量与函数的可见性和生命周期。

变量的作用域有全局作用域和局部作用域两种，局部作用域又称为函数作用域。

作用域是可以互相嵌套的，子作用域可以直接访问父作用域。

JavaScript 采用词法作用域 (lexical scoping)，也就是静态作用域。

> 静态作用域

因为 JavaScript 采用的是词法作用域，函数的作用域在函数定义的时候就决定了。

```javascript
var value = 1;

function foo() {
    console.log(value);
}

bar();
```

词法作用域: foo 在定义的时候已经确定了它的作用域链。scope = [foo.作用域、Golbal.作用域]

> 动态作用域
函数的作用域是在函数调用的时候才决定的

```javascript
var value = 1;

function bar() {
    var value = 2;
    foo();
}
bar();
```

动态作用域：foo 在执行的时候确定它的作用域链。 scope = [foo.作用域、bar.作用域、Global.作用域]

## 作用域链及闭包的形成过程

> 闭包的概念:

1. 在函数外部，可以访问函数在内部定义的方法或变量
2. 函数内部访问自由变量（自由变量：在函数中使用的，但既不是函数参数也不是函数的局部变量的变量。因此从技术上来讲，在JS中，每个function都是闭包，因为它总是能访问在它外部定义的数据）

[参考链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)

```javascript
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f;
}
var fn = checkscope();
fn();
```

JavaScript 在解析代码时：

1.预解析阶段，创建全局执行上下文，全局上下文被压入执行上下文栈

```javascript
// ECStack 代指执行上线文栈
ECStack = [
    globalContext
]
```

2.变量提升，全局上下文初始化。

```javascript
globalContext = {
    VO: [global],
    Scope: [globalContext.VO],
    this: globalContext.VO
}
```

并且全局的作用域链赋值给 checkscope 的内部属性 [[scope]]。

```javascript
checkscope.[[scope]] = [globalContext.VO]
```

3.执行 checkscope 。预解析阶段，checkscope 的函数执行上下文被压入到函数的执行上下文栈，变量提升。

```javascript
ECStack = [
    checkscopeContext,
    globalContext
]
```

4.函数执行上下文初始化:

1. 赋值函数内部属性[[scope]]，创建作用域链
2. 用 arguments 创建活动对象
3. 初始化活动对象，即加入形参、函数声明、变量声明
4. 将活动对象压入 checkscope 作用域顶端

```javascript
checkscopeContext = {
    AO: {
        arguments: {
            length: 0
        },
        scope: undefined,
        f: <reference to function>
    },

    Scope: [AO, globalContext.VO],

    this: undefined
}
```

同时 f 函数被创建，保存checkscopeContext的作用域链赋值给 f 函数的内部属性 [[scope]]

```javascript
f.[[scope]] = [checkscopeContext.VO, globalContext.VO]
```

最后将 f 返回

6.checkscope 执行完毕。checkscope 执行上下文被执行上下文栈弹出，把控制权返回给之前的执行上下文。

```javascript
ECStack = [
    globalContext
]
```

7.执行函数 fn ，创建 f 执行上先文，将 f 执行上下文压入到执行上下文栈

```javascript
ECStack = [
    fContext,
    globalContext
]
```

8.变量提升，f 函数执行上下文初始化

1. 复制函数内部属性[[scope]]创建作用域链
2. 用 arguments 创建活动对象
3. 初始化活动对象，即加入形参、变量声明、函数声明
4. 将活动对象压入到 f 作用域顶端

```javascript
fContext: {
    AO: {
        arguments: {
            length: 0
        }
    },

    Scope: [AO, checkscopeContext.VO, globalContext.AO],

    this: undefined
}
```

9.查找 scope 变量。沿着 fContext.Scope 向后找 `AO` -> `checkscopeContext.VO`，找到并返回 scope 的值。

由于 fn 保留着 f 的引用，f 中保留着 `checkscope` 的活动对象 `checkscopeContext.VO`。即在 `checkscope` 外部访问了 `checkscope` 内部的变量，这样就形成了 **闭包**

10.fn 执行完毕。f 函数执行上下在执行上下文栈弹出。 `checkscope` 的执行环境的作用域链被销毁，但它的活动对象仍然保持存在内存中。

```javascript
ECStack = [
    globalContext
]
```

为了释放 `checkscope` 活动对象所占的内存，可以 `fn` 的值设置为 `null`

## 块作用域

在某个特定的语句块中才能访问的变量。

es5 的 with 及 eval语句可以自定义独立的作用域。 由于浏览器在预编译代码的时候采用了很多的优化，而这两个语句改变了javascript的作用域规则，则预编译的优化改为了安全模式，这就导致了性能的损失。

ES6增加了声明块作用域的变量标识符 let 和 const 。

```javascript
{
    let num = 1;
    console.log(num);
}

console.log(num);

for(var a = 0;a < 5; a++);
for(let b = 0;b < 5; b++);
console.log(a)
console.log(b)
```

## 作用域的延长

在当前执行环境中 scope 中最前面临时追加一个对象，该变量会在代码执行后移除。

es5 的 try...catch

```javascript
// catch 创建了一个只可以在后面大括号内可以访问的变量
try {
    throw new Error(1);
} catch (err) {
    console.log(err)
}

// catch外部无法访问
console.log(err);
```

因此这里可以通过在 `try` 中 `throw new Error()` 一个变量，这样就可以在 `catch` 中访问这个变量。