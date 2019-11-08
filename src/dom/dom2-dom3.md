# DOM2和DOM3

## 框架的变化

iframe 都可以通过 iframe.contentDocument 来访问框架的 document 对象。（contentWindow 可以访问框架 window 对象）

## 样式

为 DOM 添加样式的方法有三种。 行内样式表、style 标签内部样式表 、link 标签外部样式表。

> 访问样式-行内样式

DOM 的 style 属性返回一个 CSSStyleDeclaration 实例对象，里面包含了通过行内样式设置的 css 样式，但不含外不设置的样式表(style标签、link标签)。

style 属性是个可读可写的属性。 一般设置具有长度单位的 css 时，需要将长度单位带上，否则在浏览器的标准模式下，不会生效。在混杂模式下，浏览器会自动添加 px 的单位。

设置具有横线分割的 css 属性时，需要转化为 驼峰命名法。

style 常用属性和方法:

名称 | 类型 | 说明
--  | -- | --
cssText | 属性 | 可访问和设置 DOM style 属性的值。IE 不支持此属性
length | 属性 | 应用给元素的CSS属性的数量
parentRule | 属性 | 表示CSS信息的CSSRule对象
getPropertyValue | 方法 | 返回给定属性的字符串值
item | 方法 | 返回给定位置的CSS属性的名称
removeProperty | 方法 | 从样式中删除给定属性
setProperty | 方法 | 从样式中设置指定的样式

> 访问样式-计算样式

style 无法获取 DOM 的外部样式。而 [window.getComputedStyle()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/getComputedStyle) 方法返回的就是 应用到 DOM 的 CSSStyleDeclaration 最终样式表的对象。（即在网页中呈现的样式）

getComputedStyle 方法返回的对象是实时的，在设置过新的属性值后，在访问原对象的属性时，总是返回最新的值。

“IE不支持getComputedStyle()方法，但它有一种类似的概念。在IE中，每个具有style属性的元素还有一个currentStyle属性。这个属性是CSSStyleDeclaration的实例，包含当前元素全部计算后的样式”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” Apple Books.

> 操作样式表

从 StyleSheet 接口继承而来的属性和方法

名称 | 类型 | 说明
--  | -- | --
disabled | 属性 | 可读写，是否禁用当前样式表
href | 属性 | 如果样式表是通过`<link>`包含的，则是样式表的URL；否则，是null
media | 属性 | 当前样式表支持的所有媒体类型的集合
cssRules | 方法 |  样式表中包含的样式规则的集合。 IE不支持这个属性，但有一个类似的rules属性
 deleteRule （index） | 方法 | 返回给定位置的CSS属性的名称
removeProperty | 方法 |  删除cssRules集合中指定位置的规则。IE不支持这个方法，但支持一个类似的removeRule()方法
insertRule（rule, index） | 方法 | 向cssRules集合中指定的位置插入rule字符串。IE不支持这个方法，但支持一个类似的addRule()方法

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>StyleSheet</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.0/normalize.min.css">
    <style>
    body {
        background-color: deepskyblue;
    }

    button {
        border: 1px solid #fff;
        outline: none;
    }

    button {
        background-color: #000;
    }

    button {
        color: deepskyblue;
    }

    </style>
</head>

<body>
    <div id="demo">StyleSheet</div>
    <button id="disabled">禁用</button>
    <button id="addRule">添加规则</button>
    <button id="delRule">删除规则</button>
    <script>
    let link = document.querySelector('link');
    disabled.onclick = _ => link.disabled = true;

    // 访问 style 标签的 StyleSheet 设置的第一条规则
    let styleSheet = document.styleSheets[1];
    console.log(styleSheet.cssRules[0]);

    // 添加一条规则
    addRule.onclick = _ => {

        styleSheet.insertRule('button {color: #fff;}', styleSheet.cssRules.length)
        console.log(styleSheet.cssRules);
    }

    // 删除规则
    delRule.onclick = _ => {
        let cssRule = styleSheet.cssRules || styleSheet.rules;
        for (let i = 0; i < cssRule.length; cssRule[i].selectorText === 'button' ? styleSheet.deleteRule(i) : i++);
    }

    </script>
</body>

</html>
```

## 元素大小

> 偏移量

element.offsetWidth ： 元素在水平方向上占用的空间(包含边框和滚动条宽度)

element.offsetHeight ： 元素在竖直方向上占用的空间(包含边框和滚动条高度)

offsetLeft ：元素的左外边框至包含元素的左内边框之间的像素距离

offsetTop ：元素的上外边框至包含元素的上内边框之间的像素距离

“其中，offsetLeft和offsetTop属性与包含元素有关，包含元素的引用保存在offsetParent属性中。offsetParent属性不一定与parentNode的值相等。例如，`<td>`元素的offsetParent是作为其祖先元素的`<table>`元素，因为`<table>`是在DOM层次中距`<td>`最近的一个具有大小的元素。”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” Apple Books.

```javascript
// 获取元素距离页面距离

// 方法-1
let getEleOffsetTopAndLeftByOffset = (ele) => {
    let offsetTop = ele.offsetTop;
    let offsetLeft = ele.offsetLeft;
    let offsetParent = ele.offsetParent;

    while (offsetParent) {
        offsetTop += offsetParent.offsetTop;
        offsetLeft += offsetParent.offsetLeft;
        offsetParent = offsetParent.offsetParent;
    }

    return {
        offsetTop,
        offsetLeft
    }
}


// 方法-2
let getScrollLeftAndTop = _ => document.compatMode === 'BackCompat' ?
{
    scrollLeft: document.body.scrollLeft,
    scrollTop: document.body.scrollTop
} : {
    scrollLeft: document.documentElement.scrollLeft,
    scrollTop: document.documentElement.scrollTop,
}

let getEleOffsetTopAndLeftByRect = ele => {
    let scroll = getScrollLeftAndTop();
    let domRect = ele.getBoundingClientRect();
    let rectLeft = domRect.left;
    let rectTop = domRect.top;

    return {
        offsetTop: scroll.scrollTop + rectTop,
        offsetLeft: scroll.scrollLeft + rectLeft
    }
}
```

> 客户区大小

element.clientWidth: 元素内容区宽度加上左右内边距宽度

element.clientHeight: 元素内容区高度加上上下内边距高度

element.clientTop : 一个元素顶部边框的宽度

element.clientLeft : 一个元素左侧边框的宽度

```javascript
// 获取浏览器窗口宽度
// 这些值不会随着页面缩放变化 (meta 标签)

let getViewSize = _ => window.innerWidth ? {
    width: window.innerWidth,
    height: window.innerHeight
} : document.compatMode === 'BackCompat' ? {
    width: document.body.clientWidth,
    height: document.height.clientHeight,
} : {
    width: document.documentElement.clientWidth,
    height: document.documentElement.clientHeight
}
```

> 滚动大小

scrollHeight : 在没有滚动条的情况下，元素内容的总高度。

scrollWidth : 在没有滚动条的情况下，元素内容的总宽度。

scrollLeft : 被隐藏在内容区域左侧的像素数。通过设置这个属性可以改变元素的滚动位置。

scrollTop : 被隐藏在内容区域上方的像素数。通过设置这个属性可以改变元素的滚动位置。

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” Apple Books.

在查看文档总高度时（document.documentElement）scrollHeight 和 clientWidth 在不同的浏览器中表现不一致，有的返回的是文档可见区域的高度。因此在获取 DOM 的实际高度时，应获取两个数值的最大值。

```javascript

let getDocumentSize = _ => document.compatMode === 'BackCompat' ? {
    width: Math.max(document.body.clientWidth, document.body.scrollWidth),
    height: Math.max(document.body.clientHeight, document.body.scrollHeight)
} : {
    width: Math.max(document.documentElement.clientWidth, document.documentElement.scrollWidth),
    height: Math.max(document.documentElement.clientHeight, document.documentElement.scrollHeight)
}
```

> 确定元素大小

[Element.getBoundingClientRect()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect) 方法返回元素的大小及其相对于视口的位置()。

“IE8及更早版本认为文档的左上角坐标是（2, 2），而其他浏览器包括IE9则将传统的（0,0）作为起点坐标。因此，就需要在一开始检查一下位于（0,0）处的元素的位置，在IE8及更早版本中，会返回（2,2），而在其他浏览器中会返回（0,0）。”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” Apple Books.

```javascript
let getBoundingClientRect = (_ => {
    let offset = 0;
    let scrollTop = document.documentElement.scrollTop;
    let scrollLeft = document.documentElement.scrollLeft;

    if (document.body.getBoundingClientRect) {
        let tmp = document.createElement('div');
        tmp.style.cssText = 'position: absolute;top: 0; left: 0';
        document.body.appendChild(tmp);
        let tmpRectTop = tmp.getBoundingClientRect().top;
        offset = -tmpRectTop - scrollTop;

        document.body.removeChild(tmp);
        tmp = null;
    }


    return ele => {

        if (document.body.getBoundingClientRect) {
            let eleRect = ele.getBoundingClientRect();

            return {
                top: eleRect.top + offset,
                right: eleRect.right + offset,
                left: eleRect.left + offset,
                bottom: eleRect.bottom + offset,
            }
        } else {
            let eleOffset = getEleOffsetTopAndLeftByOffset(ele);
            return {
                top: eleOffset.offsetTop - scrollTop,
                right: eleOffset.offsetLeft - scrollLeft + ele.offsetWidth,
                left: eleOffset.offsetLeft - scrollLeft,
                bottom: eleOffset.offsetTop - scrollTop + ele.offsetHeight,
            }
        }
    }
})();
```

## DOM 遍历

[Document.createNodeIterator()](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createNodeIterator)

[Document.createTreeWalker()](https://developer.mozilla.org/zh-CN/docs/Web/API/TreeWalker)

## DOM 范围

[Range表示包含节点和部分文本节点的文档片段](https://developer.mozilla.org/zh-CN/docs/Web/API/Range)