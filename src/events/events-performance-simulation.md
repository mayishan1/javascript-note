# 事件性能-模拟事件

## 内存和性能

在页面中绑定过多的事件，表示需要大量的事件处理函数，会占用过多的内存。并且对于DOM过多的访问，会延迟页面的访问的就绪时间。

> 事件委托

为多个目标元素的共同的父级绑定事件，根据事件对象的目标元素来此处理相应的程序。这样就避免了为多个DOM绑定事件了。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
    <title>load</title>
    <style type="text/css">
    * {
        margin: 0;
        padding: 0;
    }
    li:nth-child(odd) {
        background: deepskyblue; 
    }
    </style>
</head>

<body>
    <ul id="list">
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
    <script>
        list.addEventListener('click', e => {
            alert(e.target.textContent);
        })
    </script>
</body>
</html>s
```

> 移除事件处理程序

在使用JS删除HTML的时候，有些浏览器会保留原HTML绑定的事件处理程序在内存这种，所以注意删除HTML绑定好的事件处理程序。

## 事件模拟

> 创建自定义事件

JS创建自定义事件可以使用 [Event](https://developer.mozilla.org/zh/docs/Web/API/Event) 对象（旧版本的自定义事件已不推荐使用 [createEvent](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/createEvent)）。如果需要传递数据可以使用 [CustomEvent](https://developer.mozilla.org/zh-CN/docs/Web/API/CustomEvent)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
    <title>Event</title>
</head>

<body>
    <div id="customEventEle" style="width: 200px;height: 200px; background: deepskyblue"></div>
    <br>
    <button id="button1">触发自定义事件</button>
    <hr>
    <div id="customEventWithDateEle" style="width: 200px;height: 200px; background: deepskyblue"></div>
    <br>
    <button id="button2">触发自定义事件并携带数据</button>
    <hr>
    <label for="checkbox">
        <input id="checkbox" type="checkbox">DEMO
    </label>
    <br>
    <button id="button3">触发浏览器内置事件</button>
    <script>
    // 自定义事件
    let customEvent = new Event('customEvent');

    customEventEle.addEventListener('customEvent', _ => {
        alert('自定义事件 customEvent 触发');
    })

    button1.onclick = _ => {
        customEventEle.dispatchEvent(customEvent);
    }

    // 自定义事件并携带数据
    // detail 属性可用于传递自定义数据
    let customEventWithDate =  new CustomEvent("customEventWithDate", {
        detail: {
            customDate: '小明'
        }
    })

    customEventWithDateEle.addEventListener('customEventWithDate', e => {
        alert(`自定义事件传递的数据  ${e.detail.customDate}`)
    });

    button2.onclick = _ => {
        customEventWithDateEle.dispatchEvent(customEventWithDate);
    }

    // 触发浏览器内置事件
    button3.onclick = _ => {
        let evt = new MouseEvent('click', {
            'view': window,
            'bubbles': true,
            'cancelable': true
        });
        checkbox.dispatchEvent(evt);
    }

    </script>
</body>

</html>
```