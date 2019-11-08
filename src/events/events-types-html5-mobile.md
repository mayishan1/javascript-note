# 事件类型-HTML5事件-设备事件

## 事件类型

“Web浏览器中可能发生的事件有很多类型。而“DOM3级事件”规定了以下几类事件。

□　UI（User Interface，用户界面）事件，当用户与页面上的元素交互时触发；

□　焦点事件，当元素获得或失去焦点时触发；

□　鼠标事件，当用户通过鼠标在页面上执行操作时触发；

□　滚轮事件，当使用鼠标滚轮（或类似设备）时触发；

□　文本事件，当在文档中输入文本时触发；

□　键盘事件，当用户通过键盘在页面上执行操作时触发；等等”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” Apple Books.

> UI事件

* load: 当页面完全加载后在 window 上面触发、当所有框架都加载完毕时在框架集上面触发、当图像加载完毕时在`<img>`元素上面触发，

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1,minimum-scale=1.0, user-scalable=no">
    <title>load</title>
    <style>
    * {
        margin: 0;
        padding: 0;
    }

    #box img {
        width: 200px;
        height: 200px;
        border: 1px solid #000;
        text-align: center;
    }

    .loading-img {
        background: url(https://loading.io/spinners/balls/index.circle-slack-loading-icon.gif) no-repeat center;
        background-size: 50px auto;
    }

    </style>
</head>

<body id="body">
    <div id="box">
        <img  class="lazy" lazy-src="http://www.qq7b.com/uploads/allimg/100131/2222254405-0.jpg" alt=""/>
    </div>
        <script>
        const LAZY_CLASS = 'loadimg-img';

        let lazyImg = document.querySelector('img.lazy');

        let toggleLazyClass = ele => ele.classList.toggle(LAZY_CLASS);

        let proxyLoadImg = ele => new Promise(resolve => {
            let img = new Image(),
                src = ele.getAttribute('lazy-src');

            img.src = src;

            img.onload = _ => {
                ele.src = src;
                resolve();
            }
        })

        let loadImg = ele => {
            toggleLazyClass(ele);
            proxyLoadImg(ele).then(_ => toggleLazyClass(ele));
        }

        loadImg(lazyImg)

        </script>
</body>

</html>
```

* error : 当发生JavaScript错误时在window上面触发，当无法加载图像时在`<img>`元素上面触发

* select ：当用户选择文本框（`<input>`或`<texterea>`）中的一或多个字符时触发

* resize  ：当窗口或框架的大小变化时在window或框架上面触发。

```javascript
(function flexible(window, document) {
    var docEl = document.documentElement;

    function setRemUnit() {
        var rem = docEl.offsetWidth / 10;
        docEl.style.fontSize = rem + 'px'
    }

    setRemUnit()

    window.addEventListener('resize', setRemUnit);
}(window, document));
```

* scroll：当用户滚动带滚动条的元素中的内容时，在该元素上面触发。`<body>`元素中包含所加载页面的滚动条

```javascript

let span = document.createElement('span');
span.style.cssText = 'position: fixed;padding: 3px 20px;color: #fff; background: #000; top: 10px; left: 50%;transform: translateX(-50%);';
document.body.appendChild(span);

// 监听页面的滚动 一般搭配函数节流
let throttle = (fn, delay) => {

    let atFirst = true;
    let timmer = null;
    let preDate = null;

    return function (...arg) {
        let _this = this;

        timmer && clearTimeout(timmer);

        if (atFirst) {
            fn.apply(_this, arg);

            preDate = Date.now();
            atFirst = false;
            return;
        }

        if (Date.now() - preDate >= delay) {

            fn.apply(_this, arg);
            preDate = Date.now();

        } else {
            timmer = setTimeout(() => {
                fn.apply(_this, arg);
                preDate = timmer = null;
                atFirst = true;
            }, delay);
        }
    }
}

let renderPage = _ => {
    let page = Math.ceil(document.documentElement.scrollTop / window.innerHeight)
    page = page <= 1 ? 1 : page;
    span.textContent = page;
}

renderPage();

window.addEventListener('scroll', throttle(renderPage, 300));
```

> 焦点事件

焦点事件会在页面获得或失去焦点时触发。 `document.hasFocus()` 返回一个布尔值 判断当前页面是否聚焦的元素。`document.activeElement` 返回当前聚焦的元素，页面初始加载完毕，返回 `doccument.body` 元素。

* [focus](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/focus) 元素获得焦点时触发

* [blur](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/blur) 元素失去焦点时触发

> 鼠标事件

* [click](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/click) ：单击主鼠标按钮时触发。

* [dblclick](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/dblclick) ：双击主鼠标按钮时触发。

* [mousedown](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/mousedown) : 按下了任意鼠标按钮时触发。

* [mouseup](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/mouseup): 在用户释放鼠标按钮时触发。

* [mouseenter](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/mouseenter) ：在鼠标光标从元素外部首次移动到元素范围之内时触发。这个事件不冒泡。

* [mouseleave](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/mouseleave) ：在位于元素上方的鼠标光标移动到元素范围之外时触发。这个事件不冒泡。

* [mouseout](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/mouseout): 在鼠标指针位于一个元素上方，然后用户将其移入另一个元素时触发。又移入的另一个元素可能位于前一个元素的外部，也可能是这个元素的子元素。

* [mouseover](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/mouseover)：在鼠标指针位于一个元素外部，然后用户将其首次移入另一个元素边界之内时触发。不能通过键盘触发这个事件。

* [mousemove](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/mousemove): 当鼠标指针在元素内部移动时重复地触发。移动端不支持，轻击可单击元素会触发mousemove事件，但是也只是触发一次。

鼠标事件的正常执行顺序 `mousedown -> mousedup -> click -> mousedown -> mousedup -> click -> dblclick`

鼠标事件的事件对象包含的信息:

1. [e.clientX](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/clientX)/[e.clientY](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/clientY): 鼠标相对于页面可视区域左侧/顶部距离

2. [e.pageX](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/pageX)/[e.pageY](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent/pageY): 鼠标相对于页面左侧/顶部距离 （对于不兼容 pageX/pageY 的浏览器，可以使用 `e.clientX/e.clientY + 滚动的偏移量` ）

3. e.shiftKey、e.ctrlKey、e.altKey和e.metaKey（在Windows键盘中是Windows键，在苹果机中是Cmd键）。在发生鼠标事件的同时，如果键盘按下了以上对应的键，则对应的属性值即为true。

4. [target](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/target) 对事件起源目标的引用 ([IE低版本Event.srcElement](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/srcElement))

[更多查看](https://developer.mozilla.org/zh-CN/docs/Web/API/MouseEvent)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
    <title>load</title>
    <style>
    * {
        margin: 0;
        padding: 0;
    }

    #box {
        position: relative;
        left: 175px;
        width: 200px;
        height: 200px;
        background-color: deepskyblue;
    }

    </style>
</head>

<body>

    <div id="box"></div>
    <script>
    let boxSize = {
        width: box.offsetWidth,
        height: box.offsetHeight
    }

    let limitSize = {
        top: boxSize.width / 2,
        left: boxSize.height / 2,
        right: innerWidth - boxSize.width / 2,
        bottom: innerHeight - boxSize.height / 2
    }

    document.documentElement.addEventListener('mousemove', e => {
        // 鼠标相对于页面的 距离
        let x = e.pageX;
        let y = e.pageY;

        if (y < limitSize.top) {
            box.style.top = 0;
        } else if (y >= limitSize.top && y <= limitSize.bottom) {
            box.style.top = `${y - boxSize.height/2}px`;
        } else if (y > limitSize.bottom) {
            box.style.top = `${innerHeight - boxSize.height}px`;
        }

        if (x < limitSize.left) {
            box.style.left = 0;
        } else if (x >= limitSize.left && x <= limitSize.right) {
            box.style.left = `${x - boxSize.height/2}px`;
        } else if (x > limitSize.right) {
            box.style.left = `${innerWidth - boxSize.height}px`;
        }
    })

    </script>
</body>

</html>
```

> 键盘与文本事件

* keydown : 当用户按下键盘上的任意键时触发，而且如果按住不放的话，会重复触发此事件。

* keypress:当用户按下键盘上的字符键时触发，而且如果按住不放的话，会重复触发此事件。按下Esc键也会触发这个事件。Safari 3.1之前的版本也会在用户按下非字符键时触发keypress事件。

* keyup:当用户释放键盘上的键时触发。

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” Apple Books.

`keydown` 和 `keypress` 的事件对象都有一个 `event.keyCode` 的属性，表示此键键值的 ASCII 编码。搭配 `String.fromCodePoint(e.keyCode)` 可以确定是按的哪个键。

`keypress`  的事件对象都有一个 `event.charCode` 的属性，表示此键键值的 ASCII 编码。IE8及之前版本和Opera则是在keyCode，因此在进行取值时，应使用 `event.charCode || event.keyCode`

> DOM变动事件

[MutationObserver](https://developer.mozilla.org/zh-CN/docs/Web/API/MutationObserver) 提供了监视对DOM树所做更改的能力。

```javascript
let targetNode = document.querySelector(`#box`);

const mutationCallback = (mutationsList) => {
    for (let mutation of mutationsList) {
        console.log(mutation.type);
    }
};

let observer = new MutationObserver(mutationCallback);

box.setAttribute('data-index', 1)
box.innerHTML = '<span></span>'
```

## HTML5事件

> contextmenu 事件

在右击或其它方式打开浏览器默认菜单的时候触发此事件。一般有些有版权需要的图片网站，会阻止菜单的打开进行下载。

> DOMContentLoaded 事件

window的load事件会在页面中的一切都加载完毕时触发，而 DOMContentLoaded 事件则在形成完整的DOM树之后就会触发。不管图像、JavaScript文件、CSS文件或其他资源是否已经下载完毕。在 DOMContentLoaded 绑定无依赖其它JS的事件处理程序，可以让用户能够尽早地与页面进行交互。

> readystatechange 事件

IE为 DOM 文档中的某些部分提供了 readystatechange 事件。它的事件对象具有 readyState 的属性，它的值有五种状态。

“□　uninitialized（未初始化）：对象存在但尚未初始化。

□　loading（正在加载）：对象正在加载数据。

□　 loaded （加载完毕）：对象加载数据完成。

□　interactive（交互）：可以操作对象了，但还没有完全加载。

□　 complete （完成）：对象已经加载完毕。”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” Apple Books.

但是 readyState 的顺序有一些不确定。但是我们确定 DOM 的 loaded 和 complete 则表示已经进入交互阶段或完成阶段。

我们可以借用此事件，在 IE 中判断 Javascript 和 link 的资源是否加载完毕。[参考](/src/dom/dynamic-script.html#动态脚本)

> pageshow 和 pagehide 事件

“Firefox和Opera有一个特性，名叫“往返缓存”（back-forward cache，或bfcache），可以在用户使用浏览器的“后退”和“前进”按钮时加快页面的转换速度。这个缓存中不仅保存着页面数据，还保存了DOM和JavaScript的状态；实际上是将整个页面都保存在了内存里。如果页面位于bfcache中，那么再次打开该页面时就不会触发load事件。”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” Apple Books.

pageshow 事件的事件对象有个 persisted 的属性，用来表示当前页面是否是在缓存当中读取的，如果想要避免缓存。可以使用 `localhost.reload(true)` 从新向服务器拉取页面。

指定了 pagehide 事件的页面，不会被“往返缓存”，每次页面的加载都会触发load事件。原因：

“onunload最常用于撤销在onload中所执行的操作，而跳过onload后再次显示页面很可能就会导致页面不正常”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” Apple Books.

> hashchange 事件

url 中 # 后面的值，发生变化的时候 hashchange 事件就会被触发。[应用](http://localhost:4000/src/object-bom/location.html#%E5%BA%94%E7%94%A8)

## 设备事件

在智能手机的普及下，移动端应用增多，浏览器增加了许多针对性的事件。

> orientationchange 事件

在浏览器进行横屏的时候，触发的事件。而查看浏览器是否横屏展示的信息需要通过 `window.orientation` 属性。 [更多实践](https://div.io/topic/1828)

> touchstart、touchmove 和 touchend 事件

* touchstart ：当手指触摸屏幕时触发；即使已经有一个手指放在了屏幕上也会触发。

* touchmove ：当手指在屏幕上滑动时连续地触发。在这个事件发生期间，调用preventDefault()可以阻止滚动。

* touchend ：当手指从屏幕上移开时触发

触摸事件的事件对象

“

□　 touches ：表示当前跟踪的触摸操作的Touch对象的数组。

□　 targetTouchs ：特定于事件目标的Touch对象的数组。

□　 changeTouches ：表示自上次触摸以来发生了什么改变的Touch对象的数组。

每个Touch对象包含下列属性。

□　clientX：触摸目标在视口中的x坐标。

□　clientY：触摸目标在视口中的y坐标。

□　identifier：标识触摸的唯一ID。

□　pageX：触摸目标在页面中的x坐标。

□　pageY：触摸目标在页面中的y坐标。

□　screenX：触摸目标在屏幕中的x坐标。

□　screenY：触摸目标在屏幕中的y坐标。

□　target：触摸的DOM节点目标。

”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” Apple Books.

当用户停止触摸的时候 ，事件对象 touches 属性就不存在触摸信息， 可以通过 changeTouches 查看最终信息。

🌰[栗子](https://developer.mozilla.org/zh-CN/docs/Web/API/Touch_events)

> gesturestart 、 gesturechange 和 gestureend 事件

这些事件都是IOS特有的。**该特性是非标准的，请尽量不要在生产环境中使用它！**

“

□　gesturestart：当一个手指已经按在屏幕上而另一个手指又触摸屏幕时触发。

□　gesturechange：当触摸屏幕的任何一个手指的位置发生变化时触发。

□　gestureend：当任何一个手指从屏幕上面移开时触发。

”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” Apple Books.

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
    <title>gesturechange</title>
    <style type="text/css">

    #box {
        position: fixed;
        top: 50%;
        left: 50%;
        width: 200px;
        height: 200px;
        margin-top: -100px;
        margin-left: -100px;
        background: deepskyblue;
    }

    </style>
</head>

<body>
    <div id="box"></div>
    <script>
    let preRotation = 0;
    let preScale = 1;

    box.addEventListener('gesturechange', e => {
        box.style.cssText = `transform: rotate(${preRotation + e.rotation}deg) scale(${preScale * e.scale})`;
    })

    box.addEventListener('gestureend', e => {
        preRotation += e.rotation % 360;
        preScale *= e.scale;
    })

    </script>
</body>

</html>
```