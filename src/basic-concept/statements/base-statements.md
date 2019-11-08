# 基础语句

## 表达式语句

javascript 中最简单的语句，赋值、delete、函数调用这三类即是表达式，又是语句，所以叫做表达式语句

```javascript
// 赋值语句
greeting = 'hello ' + name;
i* = 3;

// 递增运算符(++)和递减运算符(--)和赋值语句有关，它们的作用是改变一个变量的值，就像执行一条赋值语句一样
counter++;

// delete运算符
delete o.x;

// 函数调用
alert(greeting);
window.close();
cs = Math.cos(x);
```

## 空语句

它允许包含 0 条语句。一般在模块的代码开头开头定义 `;` 符号，是为了避免在js进行代码合并时，某个模块的结尾，没有使用 `;` 来结束末尾的语句。

```javascript
; // 由于语句以一个分号结尾，则单独的分号就表示一条空语句

// 避免合并代码造成的错误
;(function($){... 此处代码...})();

// e.g.

// 模块A
var Manager = { prop: '', method: function () {} }

// 模块B
(function () {})()  

// 避免合并出现以下情况
var Manager = { prop: '',method: function (){} }(function () {})()  
```

## 块语句

块语句又叫复合语句，javascript 将多条语句联合在一起，形成一条复合语句。复合语句只须用花括号将多条语句括起来即可

```javascript
// 可以为语句块声明一个标签，并且在语句块内部使用 `break 标签名` 的方式，退出语句块;
let printTwo = false;

printing: {
    console.log("One");
    if (!printTwo) break printing;
    console.log("Two");
};

// if() 语句，如果条件成立，它只会执行紧跟在小括号后面的一条语句。因此，如果需要执行多条语句，需要使用大括号括起来

if (true) console.log('一条语句');

if (true) {
    console.log('一条语句');
    console.log('另一条语句');
};

```

## 声明语句

声明语句包括变量声明和函数声明，分别使用 var 和 function 关键字。变量的创建三个过程：

1. 创建一个保存变量的名称
2. 确定作用域
3. 变量提升
4. 判断值是否为 object。是，则保存对象引用，否，则直接保存值

```javascript
// 如果声明的函数想要立即执行，需要将函数用小括号括起来。函数声明语句，被解析为一个表达式
(function(){console.log('立即执行')}());

// 或者使用 `!` `+` `-` `~`
~function(){console.log('立即执行')}();

// 或者使用 true、false
true && function(){console.log('立即执行')}();

false || function(){console.log('立即执行')}();
```

[小括号的使用参考](https://www.cnblogs.com/snandy/archive/2011/03/08/1977112.html)
