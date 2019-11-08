# 事件处理程序-事件流-事件对象

## 事件处理程序

绑定事件的方式有三种 HTML事件 、 DOM0级事件 、 DOM2级别事件

### HTML事件

直接写在 HTML 上的事件，`<button onclick="alert('HTML事件')">点击</button>`。

HTML 事件缺点:

* `HTML` 事件可以访问在 `window` 下定义的空间。这就要求页面显示的时候，绑定了交互事件的 DOM 相关的事件已经在全局存在了，否则用户在操作页面的时候就有可能报错。

* 通过 HTML 绑定的事件与 HTML 紧密的耦合到了一起，不利于维护修改。

### DOM0级事件

每个元素（包含window和document）都有着自己事件处理程序。通过对对应的事件赋值一个函数，在某些条件的触发下，就可以运行指定的事件函数。

DOM0级事件只能指定一个，并且如果在 HTML 中指定了同名的事件，会被DOM0级事件覆盖。

```javascript
document.body.onclick = _ => console.log('body 被点击');
```

### DOM2级事件

DOM2级为每个元素（包含window和document）添加了 [addEventListener](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener) 和 [removeEventListener](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/removeEventListener) 两个方法，来添加和删除事件的监听与删除。

它们接收三个参数: [事件名称](https://developer.mozilla.org/zh-CN/docs/Web/Events) 、事件处理函数、是否使用事件捕获。

DOM2级事件可以添加多个 DOM 事件处理程序。

移除事件处理程序时候，需要和添加事件处理程序引用同一个函数，因此在添加 DOM 事件处理程序时候不要使用匿名函数。

IE 中没有 addEventListener 和 removeEventListener ，但是有 [attachEvent](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/attachEvent) 和 [detachEvent](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/detachEvent)

它们接收两个参数: [事件名称](https://developer.mozilla.org/zh-CN/docs/Web/Events)（不同于 addEventListener 这里的事件名称需要有前缀 `on` ）

IE 的 DOM2级事件 仅支持事件的冒泡。

```javascript
// 兼容 IE 的 DOM2级绑定事件的方式
let addEvent = (_ => {
    if (addEventListener) {
        return (eventElement, eventType, eventHandle) => eventElement.addEventListener(eventType, eventHandle, false);
    } else if (attachEvent) {
        return (eventElement, eventType, eventHandle) => eventElement.attachEvent(`on${eventType}`, eventHandle);
    } else {
        return (eventElement, eventType, eventHandle) => eventElement[`on${eventType}`] = eventHandle;
    }
})();

let delEvent = (_ => {
    if (removeEventListener) {
        return (eventElement, eventType, eventHandle) => eventElement.removeEventListener(eventType, eventHandle, false);
    } else if (attachEvent) {
        return (eventElement, eventType, eventHandle) => eventElement.detachEvent(`on${eventType}`, eventHandle);
    } else {
        return (eventElement, eventType, eventHandle) => eventElement[`on${eventType}`] = null;
    }
})();

let con = _ => console.log(1);
let alt = _ => alert(1);

addEvent(document.body, 'click', con);
addEvent(document.body, 'click', alt);

delEvent(document.body, 'click', alt);
```

## 事件流

事件流表示的是浏览器对于事件的捕获顺序。事件流分为两种: 事件冒泡 和 事件捕获。**通过事件捕获或事件冒泡的方式进行事件的监听是互相独立的，移除时候也需要分别进行移除。**

例如如下的结构 `<body><div><span></span></div></body>`。

事件捕获 指的是浏览器对与DOM中事件触发顺序是从最外层向里的顺序，即 `body --> div --> span`。

事件冒泡 指的是浏览器对与DOM中事件触发顺序是从最内层向外的顺序，即 `span --> div --> body`。

事件冒泡更最初是 IE 的事件流处理机制，虽然事件捕获的性能相较于事件冒泡更好一些，但是 IE 有着很大的市场份额，因此两种事件流都被保留了下来。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1,minimum-scale=1.0, user-scalable=no">
    <title>eventFlow</title>
</head>

<body id="body">
    <div id="div">
        <span id="span">点击</span>
    </div>
    <script>
    let order = 1;
    let log = ele => console.log(`${ele}----${order++}`);

    body.addEventListener('click', _ => log('body----捕获'), true);
    div.addEventListener('click', _ => log('div----捕获'), true);
    span.addEventListener('click', _ => log('span----捕获'), true);

    span.addEventListener('click', _ => log('span----冒泡'), false);
    div.addEventListener('click', _ => log('div----冒泡'), false);
    body.addEventListener('click', _ => log('body----冒泡'), false);

    </script>
</body>

</html>
```

## 事件对象

每个[事件](https://developer.mozilla.org/zh-CN/docs/Web/Events) 都会默认传入一个事件对象的参数。

在IE中使用 DOM0 绑定的事件的方式绑定处理函数，不会传入事件对象，但是可以通过 `window.event` 的方式获取事件对象。（ `attachEvent` 添加事件处理函数的方式会传入事件对象，同时也可以通过 `window.event` 的方式获得事件对象）

每个事件都会有一些特有的事件对象属性，同时也有一些通用的事件对象和方法

> 通用属性

* [cancelable](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/cancelable)  属性表明该事件是否可以被取消默认行为, 如果该事件可以用 preventDefault() 可以阻止与事件关联的默认行为，则返回 true，否则为 false

* [target](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/target) 对事件起源目标的引用 ([IE低版本Event.srcElement](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/srcElement))

* [type](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/type) 表示该事件对象的事件类型。

> 方法

* [preventDefault](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/preventDefault) 取消事件的默认行为（[IE低版本Event.returnValue](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/returnValue)）。

* [stopPropagation](https://developer.mozilla.org/zh-CN/docs/Web/API/UIEvent/cancelBubble) 停止事件冒泡 ([IE低版本Event.cancelBubble](https://developer.mozilla.org/zh-CN/docs/Web/API/UIEvent/cancelBubble))