# DOM拓展

## 选择符API

相对于 HTMLCollection 和 NodeList ，新增的 DOM 选择器，返回的 DOM 合集，都是一个 DOM 集合的快照，而不是动态合集。（即之后添加的 DOM，不会反映到上一次获取选择器返回值的集合中）

> querySelector 和 querySelectorAll

querySelector 和 querySelectorAll 都接收一个 CSS 选择符 类似 jquery 的选择器，并返回匹配的 DOM。

querySelector 返回的是单个DOM，如果页面中有多个匹配的DOM，则返回第一个。

querySelectorAll 返回的是匹配 CSS 选择符的合集。

```javascript
console.log(document.querySelector('body') === document.querySelectorAll('body')[0])
```

> matchesSelector

`element.matches(selectorString)` 判断当前 element 的 是否匹配某个 selectorString 选择符。但是每个浏览器的兼容性不同，需要做特殊处理。

```javascript
if (!Element.prototype.matches) {
    Element.prototype.matches =
    Element.prototype.matchesSelector ||
    Element.prototype.mozMatchesSelector ||
    Element.prototype.msMatchesSelector ||
    Element.prototype.oMatchesSelector ||
    Element.prototype.webkitMatchesSelector ||
    function(s) {
        var matches = (this.document || this.ownerDocument).querySelectorAll(s),
            i = matches.length;
        while (--i >= 0 && matches.item(i) !== this) {}
        return i > -1;
    };
}
```

> 元素遍历

在开发网址时，对于 DOM 的操作一般都是元素节点。DOM元素添加了几个直接获取元素节点的方法。

childElementCount: 返回元素节点的个数

firstElementChild: 返回第一个子元素节点

lastElementChild: 返回最后一个元素节点

previousElementSibling: 返回上一个元素兄弟节点

nextElementSibling: 返回后一个元素兄弟节点

## HTML5（DOM相关）

> 与class相关

获取指定 class 的 NodeList。 `element.getElementsByClassName()`。

* classList属性

    可以获取 DOM 的 class 集合对象，包含了所有的 class 和 value属性(即 html 中指定的所有 class) 并且具有一些方法，可以操作这些 class。

    add: 添加指定 class

    contains: 判断是否包含指定 class，返回布尔值

    remove: 删除指定 class

    toggle: 切换指定 class （没有则添加，有则删除）

> 焦点管理

* `document.activeElement`  获取焦点

    获取用户正在与页面交互的 DOM，即获取DOM中获取焦点的元素。在页面未加载完成时，返回的是 null。加载完成后的 元素是 body。如果页面某个可以被聚焦的元素被点击，则焦点元素为当前的元素。

* `document.hasFocus()` 是否正在与页面交互。例如点击了页面的其它部分，则此方法返回 `false`。

> HTMLDocument的变化

* document.readyState 属性:

    loading: 正在加载文档

    complete: 已经加载完文档

    document.compatMode 兼容模式

    CSS1Compate: `CSS1Compate` 表示标准模式

    BackCompat: `BackCompat` 表示混杂模式

* document.head 表示对 html 中 head 标签的引用。

* document.charset 表示页面的字符集属性。charset 的值，可以通过 meta 标签设置。`<meta charset="utf-8">`

* 自定属性。在 HTML5 中规定，自定义的属性名，应该以 `data-` 来开头，用于区分是自定义属性还是预定义属性。并且可以通过 `element.dataset` 访问。

> 插入标记

* [element.innerHTML](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/innerHTML) 属性设置或获取HTML语法表示的元素的后代。如果是动态设置脚本元素标签，大多数浏览器不会生效。

* [element.outerHTML](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/outerHTML) 属性设置或获取HTML语法表示的元素。等价于调用 `element.innerHTML`，并且调用此方法的元素也会被替换。

* [element.insertAdjacentHTML](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/insertAdjacentHTML) 将指定的文本解析为HTML或XML，并将结果节点插入到DOM树中的指定位置。

**内存与性能问题。在使用 `outerHTML` 或 `innerHTML` 进行写模式时，被替换的元素可能绑定了某些事件处理程序，这些引用关系，在 html 被替换后，不会被删除。因此在使用 `outerHTML` 或 `innerHTML` 时，需要手动删除之前绑定的事件。**

> [element.scrollIntoView](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/scrollIntoView)

指定当前元素滚动到窗口的可视区域，此方法支持支持传递一个布尔值。如果是 `true` 元素的顶端将和其所在滚动区的可视区域的顶端对齐, `false` 元素的底端将和其所在滚动区的可视区域的底端对齐。

## 专有拓展

专有拓展指的是 各个浏览器对与 DOM 的拓展，但并不属于 HTML 规范。以下是几个常用的 API

> children 属性

返回一个 HTMLCollection 的实例。不同于 childNodes 里面包含指定节点的所有类型子节点，children 返回的对象里面只包含了指定节点的所有元素子节点。

> [contains](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/contains) 方法

判断一个节点是否是一个节点的后代。如果是返回 true ，否则返回 false。

在某些不兼容的浏览器，可以使用 [Node.compareDocumentPosition](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/compareDocumentPosition) 替代。此方法可以比较当前节点与任意文档中的另一个节点的位置关系。

```javascript
if (window.Node && Node.prototype && !Node.prototype.contains) {
    Node.prototype.contains = function (arg) {
        return !!(this.compareDocumentPosition(arg) & 16)
    }
}
```

> innerText 和 outerHtml

`innerText` 和 `outerHtml` 属性返回调用此属性的节点及其子节点的文本节点。

设置 `innerText` 和 `outerHtml` 时，都会将调用此属性的节点的所有子节点删除，并使用新的文本替换。 `outerHtml` 会把调用的节点也进行替换。

> 滚动相关

`scrollIntoViewIfNeeded` 只在当前元素在视口中不可见的情况下，才滚动浏览器窗口或容器元素。如果当前元素在视口中可见，这个方法什么也不做。如果将可选的alignCenter参数设置为true，则表示尽量将元素显示在视口中部（垂直方向）。