# Date类型

时间格式分为两种。

格林尼治标准时间（GMT，旧译 “格林威治平均时间” 或 “格林威治标准时间”）是指位于伦敦郊区的皇家格林尼治天文台的标准时间，因为本初子午线被定义在通过那里的经线。

协调世界时 (UTC)  英文：Coordinated Universal Time ，别称：世界统一时间，世界标准时间国际协调时间， 协调世界时，又称世界统一时间，世界标准时间，国际协调时间，简称 UTC。

UTC 是根据 GMT 时间及时区计算出的平均值。

ECMAScript中的Date类型是在早期Java中的java.uti1.Date类基础上构建的。为此，Date类型使用自UTC（Coordinated Universal Time，国际协调时间）从`1970年1月1日午夜（零时`）开始经过的毫秒数来保存日期（我们用计算机起始时间代指1970年1月1日午夜）。在使用这种数据存储格式的条件下，Date类型保存的日期能够精确到1970年1月1日之前或之后的285 616年。

`Date.now()` 可以获取当前时区的 UCT 时间距离计算机起始时间的毫秒数。在不支持 `Date.now()`的浏览器中可以使用 `+new Date()` 或 `new Date().valueOf()`

## 指定日期转化为毫秒数

UTC 时间转化为毫秒数

`Date.parse()`（此方法效率较低）

ECMA-262没有定义Date.parse()应该支持哪种日期格式，因此这个方法的行为因实现而异，而且通常是因地区而异。将地区设置为美国的浏览器通常都接受下列日期格式：

1. “`月/日/年`”，例如: '1/1/2018'
2. “`英文月日，年`”，例如: 'January1, 2018'
3. “`英文星期 英文月 日 年 时:分:秒 时区`”，例如: 'Mon January 1 2018 00:00:00 GMT+0800'
4. “ISO 8601扩展格式`YYYY-MM-DDTHH:mm:ss.sssZ`（例如2004-05-25T00:00:00）。只有兼容ECMAScript 5的实现支持这种格式。

`Date.UTC()`

Date.UTC()的参数分别是年份、基于0的月份（一月是0，二月是1，以此类推）、月中的哪一天（1到31）、小时数（0到23）、分钟、秒以及毫秒数。
例如: `Date.UTC(2018, 0, 1, 0, 0, 0)`

在这些参数中，只有前两个参数（年和月）是必需的。如果没有提供月中的天数，则假设天数为1；如果省略其他参数，则统统假设为0。即`Date.UTC(2018, 0)` 与上面的例子等价。

## 创建时间对象

时间对象只能通过 new 操作符进行创建。

不传参的情况下，返回的时间对象是当前系统设置的时区时间，所以与现实的时间可能有偏差。因此一般时间都是使用服务器时间。

传时间戳或指定日期，可以返回对应的时间对象。

## 日期对象常用方法

[Date.prototype.getFullYear()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getFullYear)

根据本地时间返回指定日期对象的年份（四位数年份时返回四位数字）。

```javascript
console.log(new Date().getFullYear());
```

[Date.prototype.getMonth()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getMonth)

根据本地时间返回指定日期对象的月份（0-11）。

```javascript
console.log(new Date().getMonth() + 1);
```

[Date.prototype.getDate()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getDate)

根据本地时间返回指定日期对象的月份中的第几天（1-31）。

```javascript
console.log(new Date().getDate());
```

[Date.prototype.getDay()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getDay)

根据本地时间返回指定日期对象的星期中的第几天（0-6）。

```javascript
let weekMap = ['日', '一', '二', '三', '四', '五', '六'];
console.log(`星期${weekMap[new Date().getDay()]}`);
```

[Date.prototype.getHours()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getHours)

根据本地时间返回指定日期对象的小时（0-23）

```javascript
console.log(new Date().getHours());
```

[Date.prototype.getMinutes()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getMinutes)

根据本地时间返回指定日期对象的分钟（0-59）

```javascript
console.log(new Date().getMinutes());
```

[Date.prototype.getSeconds()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getSeconds)

根据本地时间返回指定日期对象的分钟（0-59）

```javascript
console.log(new Date().getSeconds());
```

[Date.prototype.getTime()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getTime)

返回从 1970-1-1 00:00:00 UTC（协调世界时）到该日期经过的毫秒数，对于 1970-1-1 00:00:00 UTC 之前的时间返回负值。

```javascript
console.log(new Date().getTime());
```

[Date.prototype.getTimezoneOffset()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/getTime)

返回当前时区的时区偏移。

```javascript
// 获取 国际标准时间
let d = new Date();
console.log(new Date(d.getTime() + d.getTimezoneOffset() * 60000))
```

## 应用

1.根据时间戳返回对应格式时间

```javascript
function formatTime(timeStamp, format) {

    let d = new Date(timeStamp);
    let Y = d.getFullYear();
    let M = d.getMonth() + 1;
    let D = d.getDate();
    let h = d.getHours();
    let m = d.getMinutes();
    let s = d.getSeconds();

    const supplementZero = num => num < 10 ? `0${num}` : num;

    const DATE = {
        Y,
        M: supplementZero(M),
        D: supplementZero(D),
        h: supplementZero(h),
        m: supplementZero(m),
        s: supplementZero(s)
    }

    return format.replace(/[YMDhms]+/g, (match) => DATE[match[0]]);
}

console.log(formatTime(-27080340872000, 'Y:M:D h:m:s'))
```