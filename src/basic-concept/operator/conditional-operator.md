# 逻辑运算符

## 逻辑与

逻辑与操作符由两个和号（&&）表示。逻辑与应用于多个表达式时，它需要判断所有的表达式都为真值。在执行判断过程中，遇到的表达式转化为布尔值的值为 true 的情况下，接着判断下一个表达式，若值为 false 的情况下，则将改表达式的值返回，后面的表达式将不会进行判断。若执行到最后，则把最后的值返回回来。即：

```javascript
console.log(1 && 0);
console.log(0 && 1);
console.log([] && NaN);
console.log({} && null);
console.log(undefined && []);
```

常用于在某些条件为真的情况下执行一些东西。

```javascript
var a = 1,b=2;
if (a === 1 && b === 2) console.log('条件都成立');

a === 1 && b === 2 && console.log('条件都成立');
```

## 逻辑非

逻辑或操作符由两个竖线符号（||）表示。逻辑非应用于多个表达式时，它需要判断表达式存在真值。在执行判断过程中，遇到的表达式转化为布尔值的值为 false 的情况下，接着判断下一个表达式，若值为 true 的情况下，则将改表达式的值返回，后面的表达式将不会进行判断。若执行到最后，则把最后的值返回回来。即：

```javascript
console.log(1 || 0);
console.log(0 || 1);
console.log([] || NaN);
console.log({} || null);
console.log(undefined || []);
```

常用于函数的参数设置默认值、在多个条件下，单独某个条件成立时执行一些操作。

```javascript
var a = 1,b = 2;
if (a === 2 || b === 2) console.log('某些条件成立');

function demo(a, b) {
    a = a || 1;
    b = b || 2;
}
```