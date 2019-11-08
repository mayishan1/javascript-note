# Javascript历史

<!-- toc -->

## 介绍

JavaScript，一种高级编程语言，通过解释执行，是一门动态类型，[面向对象](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1)（[基于原型](https://zh.wikipedia.org/wiki/%E5%8E%9F%E5%9E%8B%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88)）的解释型语言。它已经由ECMA（欧洲电脑制造商协会）通过ECMAScript实现语言的标准化。它被世界上的绝大多数网站所使用，也被世界主流浏览器（Chrome、IE、Firefox、Safari、Opera）支持。JavaScript是一门基于原型、函数先行的语言[5]，是一门多范式的语言，它支持[面向对象](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1)编程，[命令式编程](https://zh.wikipedia.org/wiki/%E6%8C%87%E4%BB%A4%E5%BC%8F%E7%B7%A8%E7%A8%8B)，以及[函数式编程](https://zh.wikipedia.org/wiki/%E5%87%BD%E6%95%B8%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80)。它提供语法来操控文本、数组、日期以及正则表达式等，不支持[I/O](https://zh.wikipedia.org/wiki/I/O)，比如网络、存储和图形等，但这些都可以由它的宿主环境提供支持。

虽然JavaScript与Java这门语言不管是在名字上，或是在语法上都有很多相似性，但这两门编程语言从设计之初就有很大的不同，JavaScript的语言设计主要受到了Self（一种基于原型的编程语言）和Scheme（一门函数式编程语言）的影响。在语法结构上它又与C语言有很多相似（例如if条件语句、while循环、switch语句、do-while循环等）。

在客户端，JavaScript在传统意义上被实现为一种解释语言，但在最近，它已经可以被[即时编译](https://zh.wikipedia.org/wiki/%E5%8D%B3%E6%99%82%E7%B7%A8%E8%AD%AF)（JIT）执行。随着最新的HTML5和CSS3语言标准的推行它还可用于游戏、桌面和移动应用程序的开发和在服务器端网络环境运行，如Node.js。

## 开始于网景

1995年，当时在网景公司就职的[布兰登·艾克](https://zh.wikipedia.org/wiki/JavaScript)正为Netscape Navigator 2.0浏览器开发的一门名为LiveScript的脚本语言，后来网景公司与昇阳电脑公司组成的开发联盟为了让这门语言搭上java这个编程语言“热词”，将其临时改名为“JavaScript”，日后这成为大众对这门语言有诸多误解的原因之一。

JavaScript推出后在浏览器上大获成功，微软公司在不久后就为Internet Explorer 3.0浏览器推出了JScript，以与处于市场领导地位的网景产品同台竞争。JScript也是一种JavaScript实现，这两个JavaScript语言版本在浏览器端共存意味着语言标准化的缺失，对这门语言进行标准化被提上了日程，在1997年，由网景、昇阳、微软、宝蓝等公司组织及个人组成的技术委员会在ECMA（欧洲计算机制造商协会）确定定义了一种名叫ECMAScript的新脚本语言标准，规范名为ECMA-262。JavaScript成为了ECMAScript的实现之一。

完整的JavaScript实现应该包含三个部分，即[ECMAScript](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Language_Resources)（语言核心）、[DOM](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model/Introduction)（文档对象模型）、[BOM](http://www.w3school.com.cn/js/js_window.asp)（浏览器对象模型）

## 规范

网景在最初将其脚本语言命名为LiveScript，后来网景在与昇阳公司合作之后将其改名为JavaScript。JavaScript最初受Java启发而开始设计的，目的之一就是“看上去像Java”，因此语法上有类似之处，一些名称和命名规范也借自Java。但JavaScript的主要设计原则源自Self和Scheme。JavaScript与Java名称上的近似，是当时网景为了营销考虑与太阳微系统达成协议的结果。为了获取技术优势，微软推出了JScript来迎战JavaScript的脚本语言。为了互用性，Ecma国际（前身为欧洲计算机制造商协会）创建了ECMA-262标准（ECMAScript）。现在两者都属于ECMAScript的实现。尽管JavaScript作为给非程序人员的脚本语言，而非作为给程序人员的脚本语言来推广和宣传，但是JavaScript具有非常丰富的特性。

发展初期，JavaScript的标准并未确定，同期有网景的JavaScript，微软的JScript双峰并峙。1997年，在ECMA（欧洲计算机制造商协会）的协调下，由网景、昇阳、微软、宝蓝组成的工作组确定统一标准：ECMA-262。

## 概论

一般来说，完整的JavaScript包括以下几个部分：

* ECMAScript，描述了该语言的语法和基本对象
* 文档对象模型（DOM），描述处理网页内容的方法和接口
* 浏览器对象模型（BOM），描述与浏览器进行交互的方法和接口

它的基本特点如下：

* 是一种解释性脚本语言（代码不进行预编译）。
* 主要用来向HTML页面添加交互行为。
* 可以直接嵌入HTML页面，但写成单独的js文件有利于结构和行为的分离。

JavaScript常用来完成以下任务：

* 嵌入动态文本于HTML页面
* 对浏览器事件作出响应
* 读写HTML元素
* 在数据被提交到服务器之前验证数据
* 检测访客的浏览器信息
* 控制cookies，包括创建和修改等

## 特性

不同于服务器端脚本语言，例如PHP与ASP，JavaScript主要被作为客户端脚本语言在用户的浏览器上运行，不需要服务器的支持。所以在早期程序员比较青睐于JavaScript以减少对服务器的负担，而与此同时也带来另一个问题：安全性。而随着服务器的强壮，虽然现在的程序员更喜欢运行于服务端的脚本以保证安全，但JavaScript仍然以其跨平台、容易上手等优势大行其道。同时，有些特殊功能（如AJAX）必须依赖JavaScript在客户端进行支持。随着引擎如V8和框架如[Node.js](http://nodejs.cn/)的发展，及其事件驱动及异步IO等特性，JavaScript逐渐被用来编写服务器端程序。且在近几年中，Node.js的出世，让JavaScript也具有了一定的服务器功能，且在某些方面比PHP的效果更为显著。

## 版本

时间 | 版本 | 与前版本的差异
-- | -- | --
1997年7月 | ECMAScript 1.0发布 | 首版
1998年6月 | ECMAScript 2.0版发布 | 格式修正，以使得其形式与ISO/IEC16262国际标准一致
1999年12月 | ECMAScript 3.0版发布 | 正则表达式，更好的字符串处理，新的状态控制符，try/catch 错误控制等等。
2007年10月 | ECMAScript 4.0版草案发布 | 由于关于语言的复杂性出现分歧,第4版本被放弃,其中的部分成为了第5版本及Harmony的基础。
2009年12月 | ECMAScript 5.0版正式发布 | 该版本力求澄清第3版中的歧义，并添加了新的功能。新功能包括：`原生JSON对象`、`继承的方法`、`高级属性的定义`以及引入`严格模式`。
2015年6月17日 | ECMAScript 6正式发布 | ES6增添了许多必要的特性，新功能包括：模块和类以及一些实用特性，例如Maps、Sets、Promises、生成器（Generators）等。

## 浏览器兼容

浏　览　器 | ECMAScript兼容性 |浏　览　器| ECMAScript兼容性
-- | -- | -- |--
Netscape Navigator 2 | —| Opera 6～7.1 | 第2版
Netscape Navigator 3 |—| Opera 7.2+  |第3版
Netscape Navigator 4～4.05 |—  | Safari 1～2.0.x | 第3版*
Netscape Navigator 4.06～4.79|第1版|Safari 3.x|第3版
Netscape 6+（Mozilla 0.6.0+）|第3版|Safari　4.x～5.x|第5版*
IE3|—|Chrome 1+|第3版
IE4|—|Firefox 1～2|第3版
IE5|第1版|Firefox 3.0.x|第3版
IE5.5～IE7|第3版|Firefox 3.5～3.6|第5版*
IE8|第5版*|Firefox 4.0 +|第5版
IE9+|第5版

## 参考

[Javascript维基百科](https://zh.wikipedia.org/wiki/JavaScript)

[JavaScript语言的历史](https://javascript.ruanyifeng.com/introduction/history.html)

[ECMAScript 历史与展望](https://www.lazycoffee.com/articles/view?id=598a598890c5953674e3b636)
