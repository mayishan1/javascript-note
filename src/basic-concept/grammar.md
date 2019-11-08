# 语法

<!-- toc -->

## 区分大小写

要理解的第一个概念就是ECMAScript中的一切（变量、函数名和操作符）都区分大小写。这也就意味着，变量名test和变量名Test分别表示两个不同的变量，而函数名不能使用typeof，因为它是一个关键字，但typeOf则完全可以是一个有效的函数名。

> **注意事项**

* 在 `HTML` (标签、属性) 和 `CSS` （选择器、样式）中，不区分大小写（html中属性不区分大小写，属性值区分大小写）

```html
<!-- 两者等效 -->
<button onClick="alert('大写')" A="123">大写点击</button>
<button onclick="alert('小写')" a="321">小写点击</button>

<script>
    // 可以获取上面两个 DOM
    console.log([...document.querySelectorAll('*[a]')])
</script>
```

* css选择器，id和类名区分大小写，标签选择器不区分大小写 

> **出于对规范的要求，尽量一致使用小写**

## 标识符

所谓标识符，就是指变量、函数、属性的名字，或者函数的参数。标识符可以是按照下列格式规则组合起来的一或多个字符：

□　第一个字符必须是一个字母、下划线（_）或一个美元符号（$）（字母包含的任何国家语言。而使用汉字会有全角和半角的问题）。

□　其他字符可以是字母、下划线、美元符号或数字。

### 变量命名示例

> `变量` 使用 驼峰命名法

```javascript
var loadingModules = {};
```

> 常量 使用 全部字母大写，单词间下划线分隔 的命名方式

```javascript
var HTML_ENTITY = {};
```

> 枚举变量 使用 Pascal命名法，枚举的属性 使用 全部字母大写，单词间下划线分隔 的命名方式。

```javascript
var TargetState = {
    STATE_ONE: 1,
    STATE_TWO: 2,
    STATE_THREE: 2
};
```

> boolean 类型的变量使用 is 或 has 开头。

```javascript
var isReady = false;
var hasMoreCommands = false;
```

[变量命名-参考链接](https://github.com/fex-team/styleguide/blob/master/javascript.md#23-%E5%91%BD%E5%90%8D)

## 注释

### 单行注释

```javascript
// 单行注释
```

### 多行注释

```javascript
/*
 * 多行注释
 * 多行注释
 */
```

### 函数注释示例

> 方法与函数的注释

```javascript
/**
* 合并Grid的行
* @param grid {Ext.Grid.Panel} 需要合并的Grid
* @param cols {Array} 需要合并列的Index(序号)数组；从0开始计数，序号也包含。
* @param isAllSome {Boolean} ：是否2个tr的cols必须完成一样才能进行合并。true：完成一样；false(默认)：不完全一样
* @return void
* @author polk6 2015/07/21
* @example
* _________________                             _________________
* |  年龄 |  姓名 |                             |  年龄 |  姓名 |
* -----------------      mergeCells(grid,[0])   -----------------
* |  18   |  张三 |              =>             |       |  张三 |
* -----------------                             -  18   ---------
* |  18   |  王五 |                             |       |  王五 |
* -----------------                             -----------------
*/
function mergeCells(grid: Ext.Grid.Panel, cols: Number[], isAllSome: boolean = false) {
  // Do Something
}
```

注释名 | 语法 | 含义 | 示例
-- | -- | -- | --
@param | @param 参数名 {参数类型} 描述信息 | 描述参数的信息 | @param name {String} 传入名称
@return | @return {返回类型} 描述信息 | 描述返回值的信息 | @return {Boolean} true:可执行;false:不可执行
@author | @author 作者信息 [附属信息：如邮箱、日期] | 描述此函数作者的信息 | @author 张三 2015/07/21
@version | @version XX.XX.XX | 描述此函数的版本号 | @version 1.0.3
@example | @example 示例代码 | 演示函数的使用 | @example setTitle(‘测试’)

[注释规范-参考链接](https://github.com/fex-team/styleguide/blob/master/javascript.md#24-%E6%B3%A8%E9%87%8A)

## 严格模式

ECMAScript 5引入了严格模式（strict mode）的概念。严格模式是为JavaScript定义了一种不同的解析与执行模型。在严格模式下，ECMAScript 3中的一些不确定的行为将得到处理，而且对某些不安全的操作也会抛出错误。要在整个脚本中启用严格模式，可以在顶部添加如下代码：

"use strict";（IE10以上兼容）

### 注意事项

> 为了在代码合并时，模块间不受影响。尽量避免在某一个JS模块中使用 `use strict` ，而是在匿名函数中使用。

[严格模式-参考链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)

## 语句

ECMAScript中的语句以一个分号结尾(表达式不需要分号结尾)；如果省略分号，则由解析器确定语句的结尾，解释器会判断后一句代码是否可以和前一句代码合并执行(++、--会尝试与后面的代码合并)，可以则加分号，不可以则不加。

表达式可以嵌套在别的表达式中，但语句不行。

语句只能独立出现，并且语句没有返回值。

```javascript
var a = 1;
var b = 2
a
++
b
console.log(a, b);
```

在我们平时开发当中，应尽量添加上分号（可以添加 [ESlint](http://eslint.cn/docs/rules/semi) 代码检测辅助书写规范），它有以下几个好处：

> 1. 开发人员也可以放心地通过删除多余的空格来压缩ECMAScript代码（代码行结尾处没有“分号会导致压缩错误，因此某些插件的代码会以 `;` 开头）。
> 2. 加上分号也会在某些情况下增进代码的性能，因为这样解析器就不必再花时间推测应该在哪里插入分号了。

[更多参考](/src/basic-concept/statements/)

## 表达式

在 javascript 中所有表达式都有返回值（如果没有返回值就是undefined），这个返回值就可以继续作为表达式的一部分。

* 原始表达式包括：常量，关键字和变量。常量 3.14,"test"、关键字 null,this,true、变量 i,k,j。
* 对象和数组的初始化表达式

```javascript
[1,2,3] // 数组初始化表达式
{a: 1}  // 对象初始化表达式
```

* 函数定义表达式

```javascript
var quare = function(x) { return x * x;}
```

* 函数调用表达式

```javascript
quare();
```

* 属性访问表达式

```javascript
Math.PI;
```

* 对象创建表达式

```javascript
new Object();
```

## 关键字和保留字

ECMAScript规范已经规定的关键字，不能用作变量、函数名、过程、和对象名

> **避免方式**
1. 由于 JS 关键字和保留字，都是小写。为了避免冲突，可使用驼峰命名，来避免此问题
2. 使用中文拼音，易懂实用

[关键字和保留字列表](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Reserved_words)

## 变量

ECMAScript的变量是松散类型的，所谓松散类型就是可以用来保存任何类型的数据。例如：

```javascript
var message;
```

此时 `message` 在未赋值之前，会保留一个特殊的值 [undefined](http://localhost:4000/src/basic-concept/data-type.html#undefined%EF%BC%88%E6%9C%AA%E5%AE%9A%E4%B9%89%EF%BC%89%E7%B1%BB%E5%9E%8B)。

```javascript
var message = 'hellow';
```

此时 `message` 保留了一个值 `hellow` 但是并没有保留值得类型。因此，可以修改变量值的同时修改类型

```javascript
var message = 'hellow';
message = 0；
```

只有在对变量进行 `+` 、`typeof` 等操作时，它才会动态的去取变量的数据类型

`var` 是声明变量的关键字，它会在当前作用域下创建。若不使用，直接对变量赋值，会创建一个全局的属性。(这样生成的全局变量，可以使用 [delete](http://localhost:4000/src/basic-concept/operator/unary-operator.html#delete-%E8%BF%90%E7%AE%97%E7%AC%A6) 操作符删除, 使用 `var` 声明的变量，不可以使用 [delete](http://localhost:4000/src/basic-concept/operator/unary-operator.html#delete-%E8%BF%90%E7%AE%97%E7%AC%A6) 操作符删除，因为属性可以使用 delete 操作符删除，而变量不可以)之所以会产生这种结果，是因为在进行变量赋值时，首先解释器会在当前作用域内查找该变量，若是找到了，则直接赋值，若是没找到，则会沿着作用域链向上查找。以此类推，一直查找到作用域的最顶端 window，若还是没有，则会window下创建以此变量为 key 的属性，并进行赋值。

```javascript
function demo1() {
    var a = 1;
    console.log(a);
}

function demo2() {
    b = 2;
    console.log(b);
}
demo1();
demo2();
console.log(b);
console.log(a);
```