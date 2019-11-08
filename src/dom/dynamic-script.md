# DOM操作技术

## 动态脚本

动态脚本可以减少页面首次加载 Javascript 和 Style 的体积，按需加载等。

异步加载 Javascript ，可以通过动态加载外部 JS 实现。

```javascript
var script = document.createElement('script');
var head = document.getElementsByTagName('head')[0];
script.src = src;
// IE 没有 onload 事件，但是有 onreadystatechange 搭配 script.readyState 的属性值为 loaded|complete 时，表明加载完毕
script.onload = script.onreadystatechange = function () {
    if (!script.readyState || /loaded|complete/.test(script.readyState)) {
        script.onload = script.onreadystatechange = null;
    }
}
head.appendChild(script);
```

## 动态样式

在使用 Sass 、 Less 我们可以为网站 定义多个不同肤色的色值变量，并生成多个多个不同肤色的 css 文件，搭配 动态样式，我们可以加载不同的 css 实现网站换肤。

```javascript
// 动态样式
var link = document.createElement('link');
var link = document.getElementsByTagName('head')[0];

link.type = 'text/css';
link.rel = 'stylesheet';
link.href = src;
link.onload = link.onreadystatechange = function () {
    if (!link.readyState || /loaded|complete/.test(link.readyState)) {
        link.onload = link.onreadystatechange = null;
    }
}
head.appendChild(link);
```

## 使用 NodeList

“NodeList（节点集合，包含所有类型的节点）、 attributes（属性节点集合，`element.attributes`） 和 HTMLCollection（元素节点集合） , 这三个集合都是“动态的”；换句话说，每当文档结构发生变化时，它们都会得到更新。

因此，它们始终都会保存着最新、最准确的信息。从本质上说，所有NodeList对象都是在访问DOM文档时实时运行的查询。

一般来说，应该尽量减少访问NodeList的次数。因为每次访问NodeList，都会运行一次基于文档的查询”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” iBooks.

```javascript
// 因为 images 是个 动态合计 多以以下代码会导致浏览器死循环
let images = document.images;
for (let index = 0; index < images.length; index++) {
    let image = document.createElement('img');
    document.body.appendChild(image);
}

// 为了避免死循环 可以缓存初次获取 NodeLise 的长度
for (let index = 0, len = images.length; index < len; index++) {
    let image = document.createElement('img');
    document.body.appendChild(image);
}
```