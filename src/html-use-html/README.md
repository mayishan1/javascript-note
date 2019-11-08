# 在HTML中使用javascript

<!-- toc -->

## `<script>`元素

向HTML页面中插入JavaScript的主要方法，就是使用 `<script>` 元素。这个元素由Netscape创造并在Netscape Navigator 2中首先实现。后来，这个元素被加入到正式的HTML规范中。HTML 4.01为 `<script>` 定义了下列6个属性

### async：可选

这个属性与 `defer` 类似，都用于改变处理脚本的行为、只适用于外部脚本文件，并告诉浏览器立即下载文件。但与 `defer` 不同的是，标记为 `async` 的脚本并不保证按照它们的先后顺序执行。第二个脚本文件可能会在第一个脚本文件之前执行。因此确保两者之间互不依赖非常重要。指定 `async` 属性的目的是不让页面等待两个脚本下载和执行，从而异步加载页面其他内容。

### charset: 可选

表示通过src属性指定的代码的字符集。由于大多数浏览器会忽略它的值，因此这个属性很少有人用。

### defer: 可选

这个属性的用途是表明脚本在执行时不会影响页面的构造。也就是说，脚本会被延迟到整个页面都解析完毕后再运行。因此，在 `<script>` 元素中设置 `defer` 属性，相当于告诉浏览器立即下载，但延迟执行。HTML5规范要求脚本按照它们出现的先后顺序执行，因此第一个延迟脚本会先于第二个延迟脚本执行，而这两个脚本会先于 `DOMContentLoaded` 事件执行。在现实当中，延迟脚本并不一定会按照顺序执行，也不一定会在 `DOMContentLoad` 时间触发前执行，因此最好只包含一个延迟脚本。

### language：已废弃

原来用于表示编写代码使用的脚本语言（如JavaScript、JavaScript1.2或VBScript）。大多数浏览器会忽略这个属性，因此也没有必要再用了。

### src：可选

表示包含要执行代码的外部文件。

### type：可选

可以看成是language的替代属性；表示编写代码使用的脚本语言的内容类型（也称为MIME类型）。虽然 `text/javascript` 和 `text/ecmascript` 都已经不被推荐使用，但人们一直以来使用的都还是 `text/javascript` 。实际上，服务器在传送JavaScript文件时使用的MIME类型通常是 `application/x-javascript` ，但在type中设置这个值却可能导致脚本被忽略。另外，在非IE浏览器中还可以使用以下值： `application/javascript` 和 `application/ecmascript` 。考虑到约定俗成和最大限度的浏览器兼容性，目前type属性的值依旧还是`text/javascript`。不过，这个属性并不是必需的，如果没有指定这个属性，则其默认值仍为 `text/javascript`

可能偶尔会看见 `language` 的值为 VBScript（对 type 而言是 text/vbscript），表示包含的脚本代码是用 Microsoft 的 [Visual Basic Script](http://www.w3school.com.cn/vbscript/index.asp) 编写的。

## Javascript解释

### 过程

* 预处理阶段

```javascript
// 它包含了变量提升，即声明语句`var`、`function`，会被提升至代码顶部
console.log(a);
var a = 1;
function a() {
    console.log(111);
}
console.log(a);

// 实际效果
var a;
function a() {
    console.logg(111);
}
console.log(a);
a = 1;
console.log(a);
```

* 执行代码

### 特点

* 没有[浏览器同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)的限制
* 阻塞DOM树构建
* 若link标签在script之前，它会等待外联样式加载完毕

## 文档模型

IE5.5引入了文档模式的概念，而这个概念是通过使用文档类型（doctype）切换实现的。最初的两种文档模式是：`混杂模式`（quirks mode）和`标准模式`（standards mode）。混杂模式会让IE的行为与（包含非标准特性的）IE5相同，而标准模式则让IE的行为更接近标准行为。虽然这两种模式主要影响CSS内容的呈现，但在某些情况下也会影响到JavaScript的解释执行。

* 标准模式，可以通过使用下面任何一种文档类型来开启：

```html

<!-- HTML 4.01 严格型  该DTD包含所有HTML元素和属性，但不包含展示性的和弃用的元素（比如font），不允许框架集（Framesets）-->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

<!-- HTML5 -->
<!DOCTYPE html>

<!-- XHTML 1.0严格型  该DTD包含所有HTML元素和属性，但不包含展示性的和弃用的元素（比如font），不允许框架集（Framesets）。必须以格式正确的XML来编写标记-->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

* 准标准模式，可以通过使用过渡型（transitional）或框架型（frameset）文档类型来触发，如下所示:

```html
<!-- HTML 4.01 过渡型 --> 
<!DOCTYPE HTML PUBLIC  
"-//W3C//DTD HTML 4.01 Transitional//EN"  
"http://www.w3.org/TR/html4/loose.dtd"> 
 
<!-- HTML 4.01 框架集型 --> 
<!DOCTYPE HTML PUBLIC  
"-//W3C//DTD HTML 4.01 Frameset//EN"  
"http://www.w3.org/TR/html4/frameset.dtd"> 
 
<!-- XHTML 1.0 过渡型 --> 
<!DOCTYPE html PUBLIC  
"-//W3C//DTD XHTML 1.0 Transitional//EN"  
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 
 
<!-- XHTML 1.0 框架集型 --> 
< !DOCTYPE html PUBLIC  
"-//W3C//DTD XHTML 1.0 Frameset//EN"  
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```

准标准模式与标准模式非常接近，它们的差异几乎可以忽略不计。因此，当有人提到“标准模式”时，有可能是指这两种模式中的任何一种。而且，检测文档模式（本书后面将会讨论）时也不会发现什么不同。本书后面提到标准模式时，指的是除混杂模式之外的其他模式。

## 注意事项

* 尽量使用外链的方式加载JS文件
  * 可缓存（避免缓存：文件路径添加?Math.random()）
  * 可维护
  * 适应未来

* 如果JS内容比较简单，并且只是一个或几个页面使用。可以选择使用内链的方式，可以避免一次请求，提升性能（尤其是移动端）。

* 一般放到 `</body>` 前面，可以保证 DOM 渲染完毕。如需在页面渲染之前加载JS，则放入 `<head>` 中（例如：适配移动端，初始化根节点的 `font-size` ）

* 服务端返回的JS的MIME类型指定为 `application/xhtml+xml` 的情况下会触发XHTML模式

## 参考链接

[谈谈 `<script>` 标签以及其加载顺序问题](https://segmentfault.com/a/1190000013615988)

[原来 CSS 与 JS 是这样阻塞 DOM 解析和渲染的](https://juejin.im/post/59c60691518825396f4f71a1)

[浏览器模式](https://github.com/hoosin/lite/blob/master/knowledge/part1/%E6%B5%8F%E8%A7%88%E5%99%A8%E6%A8%A1%E5%BC%8F/readme.md)