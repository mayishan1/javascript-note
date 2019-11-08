# RegExp类型

正则表达式是用于匹配字符串中字符组合的模式。在 JavaScript 中，正则表达式也是对象。这些模式被用于 RegExp 的 exec 和 test 方法, 以及 String 的 match、replace、search 和 split 方法。语法：

`let expession = /pattern/flags` 或者 `let expession = new RegExp('pattern', flags)`

ECMAScript 5之前，第一种方式，只有在第一次会创建RegExp实例，之后做了修改。

## flags

flags：支持6个参数 `g` 、 `i` 、 `m` 、 `u` 、 `y` 、 `s`

> g: 表示全局模式，在匹配字符数时，不会在匹配到一个结果后停止匹配

    > i: 匹配忽略大小写

    > m: 多行匹配模式。将开始和结束字符（^ 和 $）视为在多行上工作（也就是，分别匹配每一行的开始和结束（由 \n 或 \r 分割），而不只是只匹配整个输入字符串的最开始和最末尾处。

在进行匹配时，一些特殊符号需要进行转义。`(` `[`  `{` `\` `^` `$` `|` `)` `?` `*` `+` `.` `]` `}`，需要在字符前添加 `\` 反斜杠。

## 正则表达式-字符匹配

正则表达式匹配字符分类：

1. 两种模糊匹配
2. 字符组
3. 量词
4. 分支结构
5. 案例分析

### 模糊匹配

模糊匹配，指的是对匹配内容的不确定。有两个方向上的“模糊”：横向模糊和纵向模糊。

> 横向模糊

匹配中某个字符出现次数的不确定。

正则使用了 `x{m,n}` 大括号的方式，来表示 `x` 最少出现 `m` 次，最多出现 `n` 次。

```javascript
let str = 'abababbbbacb';

let fuzzyReg = /ab{1,4}/;

console.log(str.match(fuzzyReg));

// 全局进行匹配

let fuzzyRegG = /ab{1,4}/g;

console.log(str.match(fuzzyRegG));
```

> 纵向模糊

匹配中某个字符内容的不确定。

正则使用了 `x[bcd]` 中括号的方式，来表示 `x` 后面的字符可能是 `b`、`c`或`d`

```javascript
let str = 'acabadaa';

let fuzzyReg = /a[bcd]/;

console.log(str.match(fuzzyReg));

// 全局进行匹配

let fuzzyRegG =  /a[bcd]/g;

console.log(str.match(fuzzyRegG));
```

### 字符组

字符组表示了某一类的字符。在匹配时，某个字符只要在这个字符组中，都可以被匹配到。

### 范围表示法

范围及一定的区间。而范围的开始和结束，由 `-` 来表示。在程序中的字符，某些关联性的字符都可以使用连接符号来表示。例如：字母、数字。

```javascript
let str = '1aA';

let scopeNumReg = /[1-9a-zA-Z]/;

console.log(str.match(scopeNumReg));

// 全局进行匹配

let scopeNumRegG = /[1-9a-zA-Z]/g;

console.log(str.match(scopeNumRegG));
```

### 排除字符组

在进行纵向匹配时的取反效果，即 排除字符组。正则使用用 `^` 来表示排除。例如：匹配 `abc` 中的一种 `[abc]`，如果匹配的是排除 `abc` 任意一种之外的字符，则可以使用 `[^abc]`

```javascript
let str = 'abcd';

let ruleOutReg = /[^abc]/;

console.log(str.match(ruleOutReg));
```

### 字符组简写形式

> `\d` 表示任意数字[0-9]。d 表示数字的英文 digit 的缩写。

    > `\D` 表示任意数字以外的字符 `[^0-9]`。

    > `\w` 表示[0-9a-zA-Z_]中的任意一个。w 表示字母的英文 word 的缩写。

    > `\W` 表示任意[^0-9a-zA-Z_]以外的字符。

    > `\s` 表示[ \t\v\n\r\f]中的任意一个。即 空格、水平制表符、垂直制表符、换行符、回车符和换页符。

    > `\S` 表示任意[^ \t\v\n\r\f]以外的字符。

    > `.` 表示[^\n\r\u2028\u2029]以外的字符。即除去 换行符、回车符、 行分隔符和段落分隔符的任意字符，因此在单行匹配中可以表示任意字符。

而所用字符就可以通过这种方式来表示 `[\d\D]` `[\w\W]` `[\s\S]` 和 `[^]` 中任何的一个

### 量词

量词，即表示某个字符的出现次数。正则使用了 `x{m,n}` 大括号的方式，来表示 `x` 最少出现 `m` 次，最多出现 `n` 次。

#### 量词简写形式

> `{m,}` 至少出现n次

    > `{m}` 出现m次，固定的次数

    > `?` 等价于 {0,1} 出现一次或者没有。

    > `+` 等价于 {1,} 表示至少出现一次

    > `*` 等价于 {0,} 表示出现任意次或者没有。

#### 贪婪匹配和惰性匹配

贪婪匹配：尽量多的匹配

惰性匹配：尽量少的匹配

```javascript
let str = 'ababbabbbabbbb';

// 这里的匹配就是 贪婪模式 它会尽量多的对字符 b 进行匹配
let greedReg = /ab{1,4}/g;

console.log(str.match(greedReg))

// 在要匹配的内容后面添加一个 ? 问号
let inertReg = /ab{1,4}?/g;

console.log(str.match(inertReg))
```

常见的惰性匹配

> {m,n}? 匹配m次

    > {m,}? 匹配m次

    > ?? 匹配0次

    > +? 匹配1次

    > *? 匹配0次

### 多选分支

类似正则 `/[abc]/`，匹配 `abc` 中任意一个。小括号 `()` 搭配 `|` 可以达到相同的效果。但是如果匹配的是多个字符，则必须使用 小括号 `()` 搭配 `|` 的方式。

```javascript
let str = 'abc';

console.log(/[abc]/.test(str));
console.log(/(a|b|c)/.test(str));

let str1 = 'goodMorning goodNight';
let branchReg = /good(Morning|Night)/g;

console.log(str1.match(branchReg))
```

[参考案例](http://jsrun.net/3tgKp/edit)

## 正则表达式-位置匹配

字符串中每个字符间的位置，都可以使用正则匹配到。

在 `es5` 中常用的位置匹配字符：

> `^` 匹配开头

    > `$` 匹配结尾

    > `\b` 匹配单词的边界，即`\w`和`\W`之间的位置，也包括`\w`和`^`之间的位置，也包括`\w`和`$`之间的位置。

    > `\B` 匹配非单词的边界，即`\w`和`\w`、`\W`与`\W`、`^`与`\W`，`\W`与`$`之间的位置。

    > (?=p) 后面的字符为 `p` 的位置

    > (?!p) 后面的字符不为 `p` 的位置

> `$` 和 `^`

去除字符串前后的空格

```javascript
const str = '   abc abc   ';
console.log(str.replace(/^\s*|\s*$/g, ''))
```

> '(?=p)'和 `(?!p)`

比如把"12345678"，变成"12,345,678"。

```javascript
const str = '123456789';
console.log(str.replace(/(?!^)(?=(\d{3})+$)/g, ','));
```

## 正则表达式-括号的作用

括号的作用可以分为以下几类：

1. 分组或分支
2. 捕获分组
3. 非捕获分组

### 分组或分支

> 分组

使用 `()` 来表示括号内的字符为一个整体

```javascript
console.log(/(bc)$/.test('abc'));

console.log("ababa abbb ababab".match(/(ab)+/g));
```

> 分支

在小括号中，以 `|` 分割的字符，可以用来表示多个不同的匹配规则

```javascript
const str1 = 'abc';
const str2 = 'aef';
const str3 = 'agh';

console.log(/a(bc|ef|gh)/.test(str1))
console.log(/a(bc|ef|gh)/.test(str2))
console.log(/a(bc|ef|gh)/.test(str3))
```

### 引用分组

每个小括号代表一组。在使用了正则的 `test` 和 `exec` 方法后，可以使用 `RegExp.$1` 来访问第一个分组、`RegExp.$2` 来访问第二个分组，以此类推。

小括号的分组，递增的顺序为从外到里再从后从左往后的顺序。

字符串的 `match` 方法的返回值 retuanValueByMatch

1. retuanValueByMatch[0]  表示正则匹配的字符
2. retuanValueByMatch[1]~retuanValueByMatch[N] 表示匹配组 '$1'~'$N'
3. retuanValueByMatch['index']: 匹配到的位置
4. retuanValueByMatch['input']: '原始字符串

字符串的 `replace` 方法，如果第二个参数为 `function`，则其默认参数同上

```javascript
let dataStr = '2017-10-01';

dataStr.replace(/(\d{4})-(\d{2})-(\d{2})/, function (match, $1, $2, $3) {
    console.log(match, $1, $2, $3)
})

console.log(dataStr.replace(/(\d{4})-(\d{2})-(\d{2})/, '$1/$2/$3'));
```

### 反向引用

可以在正则表达式中引用分组。但是只能引用之前出现的分组。

例如:
> 2016-06-12

    > 2016/06/12

    > 2016.06.12

为了确保分隔符号前后保持一致，因此我们要求 `/` 对应 `/` ，`-` 对应 `-` ， `.` 对应 `.`。

```javascript
let dateReg = /\d{4}(\/|-|\.)\d{2}\1\d{2}/;

console.log(dateReg.test('2016-06-12'))
console.log(dateReg.test('2016/06/12'))
console.log(dateReg.test('2016.06.12'))

console.log(dateReg.test('2016/06-12'))
console.log(dateReg.test('2016.06/12'))
console.log(dateReg.test('2016-06.12'))
```

其中 `\1` 表示引用之前的分组。

### 非捕获分组

即使用小括号的分组分支功能。小括号内的匹配内容，不会被 `RegExp.$1` 访问。

```javascript
const str = 'ababa abbb';

let regQuote = /(ab)+/;

let regNotQuote = /(?:ab)+/;

console.log(str.match(regQuote));
console.log(str.match(regNotQuote));
```

[参考案例](http://jsrun.net/KngKp)

## 注意事项

> 需要被转义的字符

`^` `$` `.` `*` `+` `?` `|` `\` `/` `(` `)` `[` `]` `{` `}` `=` `!` `:` `-` `,`
