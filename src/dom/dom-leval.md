# DOM层级

正在浏览器中，我们可以通过 DOM 提供的 API 去访问编辑网页的DOM。

## NODE类型

“DOM1级定义了一个Node接口，该接口将由DOM中的所有节点类型实现。这个Node接口在JavaScript中是作为Node类型实现的；除了IE之外，在其他所有浏览器中都可以访问到这个类型。JavaScript中的所有节点类型都继承自Node类型，因此所有节点类型都共享着相同的基本属性和方法。”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” iBooks.

> 每个节点都要一个 `nodeType` 属性，来区分不同的 NODE 类型

数值 | 常量名称
-- | --
1 | ELEMENT_NODE，普通元素节点，如 `<html>,<p>,<div>,<span>,<img>`
2 | ATTRIBUTE_NODE，元素属性
3 | TEXT_NODE，文本节点
4 | CDATA_SECTION_NODE，即 `<![CDATA[ ]]>`
5 | ENTITY_REFERENCE_NODE，实体引用，如 `&amp;&nbsp;`
6 | ENTITY_NODE，实体，如 `<!ENTITY copyright “Copyright 2010, impng. All rights reserved”]>`
7 | PROCESSING_INSTRUCTION_NODE，PI，处理指令，如 `<?xml  version=”1.0″?>`
8 | COMMENT_NODE，注释 `<!–   –>`
9 | DOCUMENT_NODE，根节点，即 document.nodeType
10 | DOCUMENT_TYPE_NODE，DTD，文档类型 `<!DOCTYPE   >`
11 | DOCUMENT_FRAGMENT_NODE，文档片段
12 | NOTATION_NODE，DTD中定义的记号

> nodeName 和 nodeValue

这是属于元素节点的属性

```javascript
// 标签名 大写
document.getElementById('id').nodeName
//  文本节点的值
document.getElementById('id').nodeValue
```

> NODE常用属性

“每个节点都有一个 childNodes 属性，其中保存着一个NodeList对象。 NodeList 是一种类数组对象，用于保存一组有序的节点，可以通过位置来访问这些节点”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” iBooks.

NodeList 转换为数组

```javascript
Array.prototype.slice.call(document.querySelector('body').childNodes)
// es6
Array.from(document.querySelector('body').childNodes)
```

[parentElement-父节点](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/parentElement)

[firstChild-第一个子节点](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/firstChild)

[lastChild-最后一个子节点](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/lastChild)

[nextSibling-兄弟节点(后一个)](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nextSibling)

[previousSibling-兄弟节点(前一个)](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nextSibling)

[innerText-文本内容](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/innerText)

[textContent-一个节点及其后代的文本内容](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/textContent)-对于设置 DOM 的文本内容，它的性能优于 innerHTML 和 innerText。 并且由于对于设置 textContent 内容，不会被当成 html 解析，可以防止 XSS 攻击。

> NODE常用方法

[hasChildNodes-是否含有子节点](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/hasChildNodes)

[appendChild-插入子节点](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/appendChild)

[insertBefore-在参考节点之前插入一个节点作为一个指定父节点的子节点](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/insertBefore)

[replaceChild-替换节点](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/replaceChild)

[removeChild-删除节点](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/removeChild)

[cloneNode-克隆节点](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/cloneNode)-克隆一个元素节点会拷贝它所有的属性以及属性值,也就包括了属性上绑定的事件(比如onclick="alert(1)"),但不会拷贝使用addEventListener()方法或者node.onclick = fn这种用JavaScript动态绑定的事件。并且深度克隆可以复制 ELEMENT 的子节点。

[normalize-将当前节点和它的后代节点”规范化“（normalized）](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/normalize)-"规范化"后的DOM树中，不存在一个空的文本节点，或者两个相邻的文本节点。因此，此方法可以将追加的多个文本节点，合并为一个。

## Document 类型

“JavaScript通过Document类型表示文档。在浏览器中，document对象是HTMLDocument（继承自Document类型）的一个实例，表示整个HTML页面。而且，document对象是window对象的一个属性，因此可以将其作为全局对象来访问。”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” iBooks.

> 子节点

`document.documentElement` 指向页面的 html 节点

`document.body` 指向页面的 html 的 body 节点

> 属性

`document.title` 当前网页标题。

`document.URL` 当前网页完整 URL。

`document.referrer` 链接到当前网页的 url。

`document.domain` 当前网页的域名。方法可以通过修改域名解决由于域名引起的跨域问题（主域名必须相同）。

`document.doctype` 返回当前文档的文档类型定义。

`document.cookie` 返回与当前文档有关的所有 cookie。

```javascript
// 方法只能设置 具体的域名 向 不具体的域名方向设置
// 例如当前域名为 zhidao.baidu.com

document.domain = 'bidu.com'; // 允许

document.domain = 'music.bidu.com'; // 不允许
```

> 查找元素

下列方法返回的都是节点的动态合集。即随着节点的增加或删除，获取返回的对象，在进行访问时，也会被更新。

[getElementById-返回一个匹配特定 ID的元素](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementById)-这个方法是文档对象的方法，Element 对象上不存在[原因](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementById#%E6%B3%A8%E6%84%8F)

[getElementsByClassName-返回包含了所有指定类名的子元素的类数组对象](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementsByClassName)

[getElementsByTagName-返回一个包括所有给定标签名称的元素的HTML集合](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementsByTagName)-`document.getElementsByTagName('*')` 可以获取所有的DOM节点

> 特殊合集

`document.forms` 返回当前文档中的 `<form>` 元素的一个集合

`document.images` 返回当前文档中所有 image 元素的集合

`document.links`  属性返回一个包含文档中所有具有 href 属性值的 `<area>` 元素 `<a>` 元素的集合

## Element 类型

> 常用属性

id - 元素唯一标示

title - 有关元素的附加说明信息，一般通过工具提示条显示出来。常用于 img 标签

dir - 语言方向，默认为 `ltr` （left to right）。也可以被设置为 `rtl` （right to left）。类似使用 css 的 `float`

className - 元素指定的css类

> 属相相关操作

获取属性

`element.getAttribute(key)` , DOM元素中除了一些已经定义好的公有属性外，想要获取自定义的属性值，就需要用到此方法。

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
    <div id="demo" data-index="1"></div>
    <script>
        // 在JS中， 可以使用DOM的 id 直接访问 DOM
        console.log(demo.id);
        console.log(document.getElementById('demo').id);

        console.log(demo['data-index']);    // undefined
        console.log(demo.getAttribute('data-index'))
    </script>
</body>

</html>
```

设置属性

`element.setAttribute(key, value)`。在 HTML5 中规定，自定义的属性名，应该以 `data-` 来开头，用于区分是自定义属性还是预定义属性。并且可以通过 `element.dataset` 访问。

删除属性

`element.removeAttribute(key)`。

> 创建元素

`document.createElement(TageName)`

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” iBooks.

## DocumentFragment 类型

“DOM规定文档片段（document fragment）是一种“轻量级”的文档，可以包含和控制节点，但不会像完整的文档那样占用额外的资源。”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” iBooks.

我们可以借助 `DocumentFragment` 在里面进行 DOM 的操作，从而避免网页多次的重排和重绘。（现代浏览器会把多次的DOM操作优化成一次，所以性能可能没有显著的区别）
