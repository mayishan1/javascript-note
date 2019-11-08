# Window对象

<!-- toc -->

BOM的核心对象是window。

在浏览器中它表示浏览器的一个实例。它是JavaScript访问浏览器窗口的一个接口。

在浏览器中中替代ECMAScript规定的Global对象。这意味着在网页中定义的任何一个对象、变量和函数，都以window作为其Global对象，因此有权访问parseInt()等方法。

## 全局作用域 window

JS中，在全局作用域中定义变量和方法，都会被作为 `window` 变量和方法。

```javascript

// 使用 var function 等声明变量的语句 创建的变量 都有一个私有属性 [[Configurable]]
// 它的值为 false， 则不可以通过 delete 操作符删除
var global = '全局对象';

console.log(window.global);

globalCanBeDelete = '可以被delete的变量'

delete window.global;   // false
delete window.globalCanBeDelete; // true
```

全局对象 `window` 可以通过 `window` 的属性 `self` 访问到自身。可以通过 `self` 可以在函数中获取当前执行环境的根对象。

```javascript
(function() {
    let root = typeof self === 'object' && self === self.self && self;
    console.log(root)
})()

// 如果实在node环境
let root = typeof self === 'object' && self === self.self && self ||
    typeof global === 'object' && global.global === global.global && global;
```

一种将某个模块暴露到全局的方式

```javascript
let fn = function() {
    let root = typeof self === 'object' && self === self.self && self ||
    typeof global === 'object' && global.global === global.global && global ||
    this;

    let fns = {
        getName: function() {
            console.log('函数内部方法')
        }
    }

    if (typeof exports !== 'undefined') {
        if (typeof module !== 'undefined' && module.exports) {
            exports = module.exports = fns;
        }
        exports.fns = fns;
    } else {
        root.fns = fns;
    }
}

fn();

fns.getName();
```

## iframe 和 浏览器窗口

> iframe

[iframe](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe) 是 html 中的一个标签。

它表示一个可以被嵌入当前页面的窗口。 `iframe` 的 `src` 属性可以加载外部链接。

`name` 属性用来引用当前框架的标识。每个框架都可以使用 `window.name` 访问到 `name` 属性。如果是新页签，则 name 的属性值为 '' 空字符串。

`window.frames` 可以访问当前页面 `iframe` 所属的 `window` 对象。

如果页面没有 `iframe`, 则 `window.frames` 指向自身。如果包含多个 `iframe`，则可以通过 `window.frames[0]` 或 `window.frames[name]`访问对应的 `iframe`。

`window.top` 可以访问最顶级的框架。 `window.parent` 访问父级框架。

`contentWindow` 可以获取 iframe 的 window 对象。

```javascript
console.log(window.frames === window);
console.log(self === top);
```

并且每个 `iframe` 内部都有属于自己的 `window` 对象，因此每个 window 下对象的构造函数都是独立的。例如在多个框架之间传递数据，另一个 iframe 的数据，在当前的 window 环境下使用 `[] instanceof Array` 会返回 `false`。

```javascript
// 多个 iframe 通讯
// iframe
top.data = {a: 1}

// otherIframe
var getDate = function(data) {
    console.log(data);
}

// top 窗口
let isIframeReady = false;
let isOtherIframeReady = false;

function passDate() {
    if (isIframeReady && isOtherIframeReady) window.frames['otherIframe'].getDate(window.data)
}

// iframe 传递到当前 window 的Object类型的数据，其原型链中不包含 当前 window 下 Object.prototype
window.frames['iframe'].onload = _ => (isIframeReady = true, console.log(window.data instanceof Object), passDate());
window.frames['otherIframe'].onload = _ => (isOtherIframeReady = true, passDate());
```

向 iframe 注入方法

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>DEMO</title>
</head>

<body>

    <iframe id="test" name=”iframe1” src="./test.html"></iframe>
    <script>
    test.contentWindow.fns = {
        log: num => {
            console.log(num)
        }
    }

    var event = new Event('fns');
    test.contentWindow.addEventListener('load', function (params) {
        test.contentDocument.dispatchEvent(event);
    })

    </script>
</body>

</html>

```

iframe

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>iframe</title>
</head>

<body>
    <script>
    document.addEventListener('fns', function () {
        fns.log(11)
    });

    </script>
</body>

</html>
```

> 窗口

打开新的窗口可以通过 [window.open](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/open) 或者 html的 `a` 标签。`window.open(url, tabName, ...)` `tabName` 表示新页签的名称，如果当前html 有对应 `name` 的 `iframe`，则是控制该 `iframe` 跳转至指定链接。(window.open 可能会被浏览器拦截)

`window.open()` 方法返回一个对于新页签的引用。可以打开的是新页签，则可以调用该引用的 `close` 方法，关闭新开的页签。相对的新开的页签，也可以调用 `parent.close()` 关闭自身。

```javascript
let newTabPage = window.open('https://www.baidu.com/');

setTimeout(_ => newTabPage.close(), 3000)
```

`window.outerWidth` 和 `window.outerHeight` 获取浏览器窗口宽高，但是每个浏览器的表现不一致。（会受到 meta 标签的缩放页面影响）

`window.innerWidth` 和 `window.innerHeight` 可以获取页面可视区域的宽高。（不会受到 meta 标签的缩放页面影响）

获取页面可视区域的宽高，也可以通过 `document.documentElement.clientWidth` 和 `document.documentElement.clientHeight`

IE在混杂模式（[document.compatMode](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/compatMode)）下需要使用 `document.body.clientWidth` 和 `document.body.clientHeight` 获取页面可视区域的宽高。

```javascript
let log = function (name, size) {
    console.log(name + '----' + size)
}

log('outerHeight', window.outerHeight)

log('innerHeight', window.innerHeight)

log('outerWidth', window.outerWidth)

log('innerWidth', window.innerWidth)

log('clientWidth', document.documentElement.clientWidth)

log('clientWidth', document.body.clientWidth)
```

如果设置浏览器滚动条的宽度为 0, 则 `document.documentElement.clientWidth` 和 `window.innerWidth` 相同。

```css
// 兼容使用 webkit 内核的浏览器
::-webkit-scrollbar {
    width: 0;
}
```

```javascript
console.log(document.documentElement.clientWidth === window.innerWidth)
```

## 计时器

浏览器提供了两个异步方法。[setTimeout(fn, time, params)](https://developer.mozilla.org/zh-CN/docs/Web/API/window/setTimeout) 可以在指定的时间后执行指定的方法。[setInterval(n, time, params)](https://developer.mozilla.org/zh-CN/docs/Web/API/window/setInterval) ，可以在每隔指定的时间后，执行指定的方法。

`setTimeout` 和 `setInterval` 都会返回一个引用，通过对用的 `clearTimeout()` 和 `clearInterval()` 终止计时器的执行。

这里先说一下JS执行机制:

1. 所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
2. 主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
3. 一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
4. 主线程不断重复上面的第三步

`setTimeout` 和 `setInterval` 概括：

1. JavaScript引擎只有一个线程，无论是 `setTimeout` 还是 `setInterval` 这些异步事件必需列队等待执行。

    `setTimeout` 是在指定的是时间将指定的事件放入到异步的事件队列等待执行。

    `setInterval` 是每隔指定的事件，将指定的任务放入到异步队列等待执行。如果队列中有上次未执行完毕的任务在队列中或还有为未执行完的同步任务，则会等待执行完毕再放入队列。因此队列中只有一个 同一个的 `setInterval` 任务（因此 `setInterval` 任务不会积累，并且每个浏览器对超时的异步任务处理方式也可能略有不同）。

2. 如果`setTimeout` 在将要执行的时候被阻塞，它将会等待下一个时机，因此指定的任务可能比预期的延时要长。
3. 如果 `setInterval` 的执行时间较长（比指定的延时长），那么它们将连续地执行而没有延时。

[参考](https://www.cnblogs.com/youxin/p/3354924.html)

```javascript
function delay() {
    var start = Date.now();
    for ( i = 1; i <= 2000000000; i++) {
        if ( i == 2000000000 ) {
            console.log('');
        }
    }
    console.log(Date.now() - start);
}

// delay 模拟延时操作，这里的计时器不会因为其它同步任务时间执行过长 而一次性打印好几次
setInterval(_ => console.log(1), 2000);

delay();
```

`setInterval` 常被用来做动画。但是如果某个JS任务执行的时间过长，则计时器放入到异步任务队列的方法，在指定时间内可能不会执行。

浏览器的帧率一般为60HZ，即浏览器重绘页面每秒最多绘制60次，相当于 1000/60 ms 每次。因此在使用 `setInterval` 时，设置的毫秒小于 1000/60，是不会生效的。

HTML 5标准规定，setInterval 的最短间隔时间是10毫秒，也就是说，小于10毫秒的时间间隔会被调整到10毫秒。

因此尽量使用 CSS3 或 HTML5 提供的 `requestAnimationFrame` 做动画。`requestAnimationFrame` 动画采用的是系统时间间隔，不会过度绘制和延迟绘制，可以保证最佳的帧率绘制动画。

`requestAnimationFrame` 优势：

1. requestAnimationFrame会把每一帧中的所有DOM操作集中起来，在一次重绘或回流中就完成，并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率，一般来说，这个频率为每秒60帧。

2. 在隐藏或不可见的元素中，requestAnimationFrame将不会进行重绘或回流，这当然就意味着更少的的cpu，gpu和内存使用量。

> DEMO: div 在 2s 增长 1000像素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>DEMO</title>
    <style>
    #box,
    #otherbox,
    #anotherBox {
        width: 200px;
        height: 200px;
        margin-top: 10px;
        background-color: aqua;
        font-size: 25px;
        line-height: 200px;
    }

    #anotherBox {
        animation: grownWidth 2s;
        animation-fill-mode: forwards;
        animation-timing-function: linear;
    }

    @keyframes grownWidth {
        from {
            width: 200px;
        }

        to {
            width: 1200px;
        }
    }

    </style>
</head>

<body>
    <div id="anotherBox">css3</div>
    <div id="box">setInterval</div>
    <div id="otherbox">requestAnimationFrame</div>

    <script>
    function delay() {
        var start = Date.now();
        for (i = 1; i <= 500000; i++) {
            if (i == 500000) {
                console.log('');
            }
        }
        console.log(Date.now() - start);
    }

    let box = document.getElementById('box');
    let otherbox = document.getElementById('otherbox');
    let distance = 1000;
    let duration = 2000;
    let boxWidth = otherbox.offsetWidth = box.offsetWidth;
    let maxWidth = boxWidth + distance;

    // 模拟延时
    setInterval(delay, timerDuractin);

    </script>
</body>

</html>

```

使用 `setInterval`

```javascript
let currentWidth = boxWidth;
let timerDuractin = 1000 / 60;
let step = distance / (duration / timerDuractin);

let timer = setInterval(function () {
    currentWidth += step;
    box.style.width = Math.min(currentWidth, maxWidth) + 'px';
    if (currentWidth > maxWidth) {
        clearInterval(timer);
    }
}, timerDuractin);
```

使用 `requestAnimationFrame`

```javascript
let start = null;

function reqAnimation() {
    let now = Date.now();
    !start && (start = now);

    let growVal = (now - start) / duration * distance;
    otherbox.style.width = Math.min(boxWidth + growVal, maxWidth) + 'px';

    if ((now - start) < duration) {
        window.requestAnimationFrame(reqAnimation);
    }
}
window.requestAnimationFrame(reqAnimation);
```

因此可以看出如果有耗时的其它任务，则 `requestAnimationFrame` 可以做到不延时，`setInterval` 可以做到绘制每一帧（每一帧的任务耗时需不超过浏览器最小帧率耗时，即不超过 1000/60）。

## 系统对话框

系统对话框都会暂停 JS 的执行。

> [alert](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/alert)

显示一个警告对话框,上面显示有指定的文本内容以及一个"确定"按钮。

> [confirm](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/alert)

显示一个具有一个可选消息和两个按钮(确定和取消)的模态对话框。

如果JS是运行在一个原生的 WebView中，则此方法可能用于与原生交互，

[参考链接](https://github.com/pengwei1024/JsBridge/blob/master/JsBridge.md)

> [propmt](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/prompt)

显示一个对话框,对话框中包含一条文字信息,用来提示用户输入文字.