# Undefined 和 Null 类型

## 相同点

* 都只有一个值
* 转换为布尔值都为`false`
* 都没有方法

## 不同点

* `undefined` 表示变量未初始化， `null` 表示一个空对象指针，但是已经是初始化过的（初始化即赋值）
* typeof 返回值不同
* undefined 派生自 null
* null 是JS关键字， undefined 不是。undefined 是window对象的一个属性，它的值为 未定义 （即undefined）
* null 转化为数字 0，undefined 转化为数字为 NaN

## 使用场景

* 检测变量是否初始化，使用 `xxx === undefined`
* 检测变量值是否为空，使用 `xxx === null`
* 判断一个变量值是否存在，使用 `xxx == null`, `undefined` 会隐式转化为 `null`
* 在一个函数开始的位置，定义一个 `var undefined`。这样可以避免函数中判断一个变量的值，是否是未定义，在函数的作用域中就可以就可以查找到，而不必向上一层的作用域查找。
* 一般而言，不存在需要显式地把一个变量设置为undefined值的情况
* 只要意在保存对象的变量还没有真正保存对象，就应该明确地让该变量保存null值

## 注意事项

 `typeof null` 返回一个 "object"。

 曾经有提案 typeof null === 'null'，但最终提案被拒绝。([原因](http://2ality.com/2013/10/typeof-null.html))

因此ECMA-262第3版之前的版本中并没有规定 undefined 这个值。第3版引入这个值是为了正式区分空对象指针与未经初始化的变量。
