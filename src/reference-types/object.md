# Object类型

对象的属性名都是字符串类型。

[API参考链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)

## 创建

创建Object实例的方式有两种。

1.使用 new 操作符

```javascript
let obj = new Object();

// 也可定义初始值
let initObj = new Object({
    name: 'xiaoli'
});
```

2.对象字面量

```javascript
// 直接定义对象的初始值
let initObj = {
    name: 'xiaoli'
}
```

> 在通过对象字面量定义对象时，实际上不会调用Object构造函数（Firefox2及更早版本会调用Object构造函数；但Firefox 3之后就不会了）

## 取值

对对象的取值方式有两种。

```javascript
let obj = {
    1: 1,
    name: '小李',
    'little name': '小小李'
}
```

1.使用 `.` 操作符。前面是是对象，后面属性名称。

```javascript
console.log(obj.name)
```

2.使用 `[]` 操作符。前面是是对象，里面属性名称。

支持对特殊属性名称进行访问，例如属性名使用了关键字和保留字、属性名中包含会导致语法错误的字符（空格等）、变量

```javascript
let key = 'name';

console.log(obj[1]);    // 1 被转化为字符串
console.log(obj[key]);
console.log(obj['little name']);
```