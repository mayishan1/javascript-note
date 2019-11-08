# String

String类型用于表示由零或多个16位Unicode字符组成的字符序列，即字符串。字符串可以由双引号（"）或单引号（'）表示。（双引号和单引号可以互相嵌套，但是不可以嵌套相同类型）

```javascript
var a = "'字符'"; //正确
var a = '"字符'"; // 错误
```

## 长度

可以通过 字符串.length 的方式，获取字符串的长度。即字符串中每个字符所对应的 Unicode编码的长度

## 转义字符

Code | Output
-- | --
\0 | 空字符
\'、\" | 单引号、双引号
\\ | 反斜杠
\n | 换行
\r | 回车

[String-MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String#Syntax)

## 数据转换

`toString()` 可以将任意类型(不包含 undefined null)变量转化为字符串类型。

其结果就是，原样输出，两边用引号括起来。

`String()` 可以将 null 和 undefined 转化为字符串

```javascript
console.log(String(null)); // 'null'
console.log(String(undefined)); // 'undefined'
```

## 使用

* 尽量使用单引号，有利于 HTML 的拼接

```javascript
var html1 = '<div  data-name="test">';
var html2 = '单引号</div>';
var html = html1 + html2;
```

* 使用 `x + ''`，可以快速将 x 转换为字符串类型
* 在想后台传输 input 的文本时，尽量使用 `xxx.trim()` 去除用户可能误输入的空格
* 生成随机字符串 `Math.random().toString(36).match(/[a-zA-Z]/g)`