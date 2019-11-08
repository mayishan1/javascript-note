---
title: 前端面试
date: 2017-07-09 20:58:44
tags:  [HTML, CSS, JAVASCRIPT]
categories: 面试
---
<!-- toc -->

# HTML

## 语义化标签有什么好处
* 有利于SEO，有助于爬虫抓取更多的有效信息，爬虫是依赖于标签来确定上下文和各个关键字的权重。
* 语义化的HTML在没有CSS的情况下也能呈现较好的内容结构与代码结构
* 用户体验：例如title、alt用于解释名词或解释图片信息、label标签的活用；
* 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）
* 便于团队开发和维护，语义化更具可读性，遵循W3C标准的团队都遵循这个标准，可以减少差异化。
* [参考链接](https://github.com/zhanyouwei/blog-source/issues/6)

## HTML5.2
* 原生的`<dialog>`元素
* 在iframe中的使用支付请求API（Payment Request API）
* Apple Icons支持多个尺寸
* 复数的`<main>`元素
* `<body>`中的样式
* 在`<legend>`中使用标题元素
* [参考链接](https://zhuanlan.zhihu.com/p/33174266)

## CSS

### 盒子模型
* 在一个文档中，每个元素都被表示为一个矩形的盒子。包含了：content + padding + border + margin。
```css
    /*
    * 更符合主观意志布局。譬如定义：盒子的宽度 = width（包含了padding 和 border）；
    */
    *, *:before, *:after {
        -moz-box-sizing: border-box;
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
    }
    
```
* [参考链接](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)


## BFC-块级元素格式化上下文

### 特点
* 对应一个独立、封闭的渲染区域，子元素的CSS样式不会影响BFC元素外部；例如: 
    * 普通块级元素，其子元素的margin-top，不会隔开自身与父元素（普通块级元素），但是会作用到父元素外部（将父元素和叔伯元素或祖父元素隔开）；
    * BFC元素，作为一个独立、封闭的渲染区域，其子元素的margin-top，则会隔开自身与父元素（BFC元素），而不会影响到父元素外部；
* 浮动子元素参与BFC父元素的高度计算，也就是BFC元素能够识别浮动元素
* BFC 在页面上是一个独立的容器，与其他元素互不影响
* BFC 区域不会与 浮动区域重叠

### 如何创建
* float 值不为 none，只要设置了浮动
* position值不为static，只要设置了定位
* display 值不为默认，只要设置了display
* overflow 值不为 visible，只要设置了overflow
* [参考链接](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)

## 常用布局说明

* [双飞翼和圣杯(三栏布局)](https://coding.net/u/mrmayi/p/front-note/git/blob/master/html/layout/%E5%8F%8C%E9%A3%9E%E7%BF%BC%E5%9C%A3%E6%9D%AF%E5%B8%83%E5%B1%80.html)

* 水平垂直居中
    * display: flex + margin: auto 不限宽高 [DEMO](https://jsfiddle.net/mrmayi/18a1qfap/)
    * position + translate 不限宽高 [DEMO](https://jsfiddle.net/mrmayi/5009ug4c/)
    * display: table + vertical-align: middle 不限宽高 [DEMO](https://jsfiddle.net/mrmayi/z41n48xq)
    * ::afert + vertical-align: middle 不限宽高 [DEMO](https://jsfiddle.net/mrmayi/3zwxs2by)
    * flex 不限宽高 [DEMO](https://jsfiddle.net/mrmayi/fdcfxy5t)
    * position + margin 限定宽高 [DEMO](https://jsfiddle.net/mrmayi/6cn05v1q)
    * position + calc 限定宽高 [DEMO](https://jsfiddle.net/mrmayi/4L97d2ay)

## Flex布局
> **当时设置 flex 布局之后，子元素的 float、clear、vertical-align 的属性将会失效。 **

### 容器

1. `flex-direction: row | row-reverse | column | column-reverse;` 主轴的排列方向
2. `flex-wrap: nowrap | wrap | wrap-reverse;` 当子元素超过父级是否换行
3. `flex-flow: <flex-direction> || <flex-wrap>`; 复合属性
4. `justify-content: flex-start | flex-end | center | space-between | space-around;` 子元素在主轴的排列方式 
5. `align-items: flex-start | flex-end | center | baseline | stretch;` 项目在交叉轴上的对齐方式
6. `align-content: flex-start | flex-end | center | space-between | space-around | stretch;` 多轴的对齐方式

### Flex 项目属性

1. `order: <integer>;` 容器中的排列顺序，数值越小，排列越靠前，默认值为 0
2. `flex-basis` 定义了在分配多余空间之前，项目占据的主轴空间
3. `flex-grow: <number>;` 项目在不超过父级宽度时的放大比例
4. `flex-shrink: <number>;`项目在超过父级宽度时的缩小比例，
5. `flexnone | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]` 复合属性
```css
    /*当 flex 取值为一个非负数字，则该数字为 flex-grow 值，flex-shrink 取 1，flex-basis 取 0%，如下是等同的*/
    .item {flex: 1;}
    .item {
        flex-grow: 1;
        flex-shrink: 1;
        flex-basis: 0%;
    }
    /*当 flex 取值为 0 时，对应的三个值分别为 0 1 0%*/
    .item {flex: 0;}
    .item {
        flex-grow: 0;
        flex-shrink: 1;
        flex-basis: 0%;
    }
```
6. `align-self: auto | flex-start | flex-end | center | baseline | stretch;` 覆盖父级的 `align-items` 属性，可单独定制自身对齐方式

# Javascript

## 原型链

* js一切皆源于对象
* 每个对象都有`__proto__`属性。该属性并不是语言本身的特性，这是各大厂商具体实现时添加的私有属性，虽然目前很多现代浏览器的 JS 引擎中都提供了这个私有属性，但依旧不建议在生产中使用该属性，避免对环境产生依赖。
* 每个函数都有`prototype`属性，值为一个对象，它包含一个`constructor`属性，指向函数自身
* 对象的 `__proto__` 指向 实例对象的 `prototype`，即:
```javascript
    function F() {
        this.num = 1;
    }

    F.prototype.con = function() {console.log(1)};

    // new 操作符，所做的事，包含四部
    // 1. 返回一个新的对象
    // 2. 将 后面的函数的 this 指针，指向 新返回的对象
    // 3. 将函数运行一遍
    // 4. 将新返回的对象 __proto__ 指向 函数的 prototype

    let obj = new F();
   
    console.log(obj.__proto__ === F.prototype)
```
## Function 和 Object 的关系
```javascript
// 对象的尽头为 Object.prototype 因此，它的 __proto__ 为 null
console.log(Object.prototype.__proto__) // true

// Function.prototype 的是由 Object.prototype 产生的，即:
console.log(Function.prototype.__proto__ === Object.prototype) // true

// Function.prototype 实例出 所有对象(Function、Object、Array) 即:
console.log(Function.prototype === Function.__proto__);
console.log(Function.prototype === Object.__proto__);
console.log(Function.prototype === Array.__proto__);

```
**总结: 先有Object.prototype（原型链顶端），Function.prototype继承Object.prototype而产生，最后，Function和Object和其它构造函数继承Function.prototype而产生。**

## 继承
> '子类'拥有'父类'的方法
### 原型继承（new）
```javascript
// 声明父类
function Person() {
    this.a = 1;
    this.arr = [1];
} 
Person.prototype.con = function() {console.log('this is parent~')}

// 声明子类
function Child() {
    //...
}
// 继承父类原型
Child.prototype = new Person();

// 缺点：实例化子类时，无法向父类传参
let sub1 = new Child();
let sub2 = new Child();

// 缺点 ：属性功效，修改 sub1 下的 arr 会影响到 其它实例对象
sub1.arr.push(2);  
console.log(sub1.arr);
console.log(sub2.arr);
```
### 借用构造函数
> 使用call/apply 在'子类'中将 '父类'执行一遍
```javascript
// 声明父类
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.arr = [1];
} 
Person.prototype.con = function() {console.log('this is parent~')}

// 声明子类
function Child() {
    Person.apply(this, arguments);
    this.b = 2;
}

// 继承父类原型
Child.prototype = new Person();

// 缺点：实例化子类时，无法功效父类的原型
// 优点: 创建子类实例时，可以向父类构造函数传参
let sub1 = new Child('小李', 23);
let sub2 = new Child('小明', 22);

// 优点: 解决了继承父类时，引用类型共享的问题
sub1.arr.push(2);  
console.log(sub1);
console.log(sub2);
```
### 组合继承（伪经典继承）
> 借用构造函数 + 原型模式
```javascript
// 声明父类
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.arr = [1];
} 
Person.prototype.con = function() {console.log(`this is ${this.name}~`)}

// 声明子类
function Child() {
    Person.apply(this, arguments);
    this.b = 2;
}
// 优点: 实现了函数复用
// 缺点: 父类被调用两次，同时父类的属性也创建了两份。在访问子类实例时，由于同名屏蔽，不会访问到父类的原型,造成了内存浪费。
// 继承父类原型
Child.prototype = new Person();

// 缺点：实例化子类时，无法功效父类的原型
// 优点: 创建子类实例时，可以向父类构造函数传参
let sub1 = new Child('小李', 23);
let sub2 = new Child('小明', 22);

// 优点: 解决了继承父类时，引用类型共享的问题
sub1.arr.push(2);  
console.log(sub1);
console.log(sub2);
```
### 原型式
> 借用函数得到得到一个“纯洁”的新对象（“纯洁”是因为没有实例属性），再逐步增强之（填充实例属性）
```javascript
function create(obj) {
    var F = function(){};
    F.prototype = obj;
    return new F();
}

var person = {
    name: 'kevin',
    friends: ['daisy', 'kelly']
}

var person1 = create(person);
var person2 = create(person);

person1.name = 'person1';
console.log(person2.name); // kevin

person1.friends.push('taylor');
console.log(person2.friends); // ["daisy", "kelly", "taylor"]
```
### 寄生式
> 创建新对象 -> 增强 -> 返回该对象，这样的过程叫寄生式继承。
```javascript
function create(obj) {
    var F = function(){};
    F.prototype = obj;
    return new F();
}

function createObj (o) {
    var clone = Object.create(o);
    clone.sayName = function () {
        console.log('hi');
    }
    return clone;
}
```
### 寄生组合继承（最佳方式）
> 借用一个构造函数，避免两次调用父类，同时继承父类方法。
```javascript
function create(obj) {
    var F = function(){};
    F.prototype = obj;
    return new F();
}

// 声明父类
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.arr = [1];
} 
Person.prototype.con = function() {console.log(`this is ${this.name}~`)}

// 声明子类
function Child() {
    Person.apply(this, arguments);
    this.b = 2;
}

// 优点: 避免父类被调用两次
// 继承父类原型
function prototype(child, parent) {
    var prototype = create(parent.prototype);
    prototype.constructor = child;
    child.prototype = prototype;
}

prototype(Child, Person);

let sub1 = new Child('小李', 23);
let sub2 = new Child('小明', 22);

sub1.arr.push(2);  
console.log(sub1);
console.log(sub2);
```

## 闭包

[参考链接](https://github.com/mqyqingfeng/Blog/issues/9)

> 1. 在函数外部，可以访问它在内部定义的方法，变量
> 2. 函数内部访问自由变量（自由变量：在函数中使用的，但既不是函数参数也不是函数的局部变量的变量。因此从技术上来讲，在JS中，每个function都是闭包，因为它总是能访问在它外部定义的数据）


**DEMO**
```javascript
function f() {
    var num = 1;

    return {
        add: function() {
            num++;
        },
        sub: function() {
            num--;
        },
        num: function() {
            return num;
        }
    }
}

let obj = f();
console.log(obj.num());
obj.add();
console.log(obj.num());
obj.sub()
console.log(obj.num());
```
### 应用场景
**setTimeout**
```javascript
function add(a, b) {
    return function() {
        console.log(a + b);
    }
}

setTimeout(add(1,2), 1000);
```
**封装相关的功能集**
```javascript
let createImg = (function() {
    let src = '一个loading图片地址';
    
    var buffAr = [
        '<img id="',
        '',
        '" class="',
        '',
        '" src="',
        '',
        '" width="',
        '',
        '" height="',
        '',
        '" alt="',
        '',
        '"/>'
    ];

    return function ({
        id="", 
        className="", 
        src = "https://static.segmentfault.com/v-5ad05bec/global/img/banner-login/banner-bg.svg", 
        width=600, 
        height=400, 
        alt="图片"}) {
        buffAr[1] = id;
        buffAr[3] = className;
        buffAr[5] = src;
        buffAr[7] = width;
        buffAr[9] = height;
        buffAr[11] = alt;

        document.querySelector('body').innerHTML += buffAr.join('');
    }
})()

createImg({src: 'https://static.segmentfault.com/v-5ad05bec/global/img/banner-login/banner-bg.svg', width: 600, height:400)
```

## 浏览器缓存

### 在HTTP请求和响应的消息报头中，常见的与缓存有关的消息报头有：

|规则|信息报头|值/示例|类型|作用|
|:--:|:--:|:--:|:--:|:--:|
|新鲜度|Expies|Sun,16 Oct 2016 GMT|响应|告诉浏览器在过期时间前可使用（Cache-Control存在时，以此为准）|
|-|Pragma|no-cache|响应|告诉浏览器忽略缓存副本|
|-|Cache-Control|no-cache|响应|告诉浏览器忽略缓存副本，强制每次请求直接发送给服务器|
|-|-|no-store|响应|强制浏览器在任何情况下都不要缓存副本|
|-|-|max-age=[秒]|响应|当前资源缓存时长，即从请求开始到过期之间的秒数|
|-|-|public|响应|任何途径的缓存者（本地缓存、代理服务器），都可以无条件缓存该资源|
|-|-|private|响应|只针对单个用户/实体（不同用户、窗口）缓存资源|
|-|Last-Modified|Sun,16 Oct 2016 GMT|响应|高速浏览器最后的修改时间|
|-|If-Modified-Since|Sun,16 Oct 2016 GMT|请求|如果上一次请求的响应包含`Last-Modified`，则下一次请求会把它的值作为此项的值发送给后后台|
|效验值|ETag|50b1cldkasa1:df3|响应|高速浏览器此资源的唯一标识(生成规则由服务器定)|
|-|If-None-Match|50b1cldkasa1:df3|请求|如果上一次请求的响应包含`ETag`，则下一次请求会把它的值作为此项的值发送给后后台|

**Cache-Control与Expires**
    Cache-Control与Expires的作用一致，都是指明当前资源的有效期，控制浏览器是否直接从浏览器缓存取数据还是重新发请求到服务器取数据。只不过Cache-Control的选择更多，设置更细致，如果同时设置的话，其优先级高于Expires。

**Last-Modified/ETag与Cache-Control/Expires**
    配置Last-Modified/ETag的情况下，浏览器再次访问统一URI的资源，还是会发送请求到服务器询问文件是否已经修改，如果没有，服务器会只发送一个304回给浏览器，告诉浏览器直接从自己本地的缓存取数据；如果修改过那就整个数据重新发给浏览器；

    Cache-Control/Expires则不同，如果检测到本地的缓存还是有效的时间范围内，浏览器直接使用本地副本，不会发送任何请求。两者一起使用时，Cache-Control/Expires的优先级要高于Last-Modified/ETag。即当本地副本根据Cache-Control/Expires发现还在有效期内时，则不会再次发送请求去服务器询问修改时间（Last-Modified）或实体标识（Etag）了。

    一般情况下，使用Cache-Control/Expires会配合Last-Modified/ETag一起使用，因为即使服务器设置缓存时间, 当用户点击“刷新”按钮时，浏览器会忽略缓存继续向服务器发送请求，这时Last-Modified/ETag将能够很好利用304，从而减少响应开销。

**Last-Modified与ETag**
    你可能会觉得使用Last-Modified已经足以让浏览器知道本地的缓存副本是否足够新，为什么还需要Etag（实体标识）呢？HTTP1.1中Etag的出现主要是为了解决几个Last-Modified比较难解决的问题：
    1. Last-Modified标注的最后修改只能精确到秒级，如果某些文件在1秒钟以内，被修改多次的话，它将不能准确标注文件的新鲜度
    2. 如果某些文件会被定期生成，当有时内容并没有任何变化，但Last-Modified却改变了，导致文件没法使用缓存
    3. 有可能存在服务器没有准确获取文件修改时间，或者与代理服务器时间不一致等情形

**用户操作行为与缓存**

| 用户操作 | Expries/Cache-Control | Last-Modified/ETag |
|:---:|:---:|:---:|
|地址栏回车|有效|有效|
|页面链接跳转|有效|有效|
|新开窗口|有效|有效|
|前进后退|有效|有效|
|F5刷新|无效|有效|
|Ctrl+F5强势刷新刷新|无效|无效|

### HTML5缓存
在html文件里添加配置.manifest后缀的文件
```html
<html lang="en" manifest="haorooms.manifest">
```
这种写法要在web 服务器上配置正确的 MIME-type，即 "text/cache-manifest"。

例如对Apache服务器进行配置的时候，需要找到 ｛apache_home｝/conf/mime.type这个文件(.htaccess)，并在文件最后添加如下所示代码：
```
text/cache-manifest .manifest 
```
新的写法：
```html
<html manifest="haorooms.appcache">
```
扩展名".appcache"为后缀，不需要我们再进行服务器端的配置了。就可以纯前端的进行离线缓存的操作。

**接下来详细说说manifest的细节，一个典型的manifest文件代码结构像下面这样：**
```text
CACHE MANIFEST
# 注释：需要缓存的文件，无论在线与否，均从缓存里读取
chched.js
cached.css

# 注释：不缓存的文件，无论缓存中存在与否，均从新获取
NETWORK:
uncached.js
uncached.css

# 注释：获取不到资源时的备选路径，如index.html访问失败，则返回404页面
FALLBACK:
index.html 404.html
```

## 函数柯里化
> 可以将一个多参数的函数简化为使用单个参数的形式
```javascript
    
    function getAdd(a,b,c) {
        console.log(Array.prototype.slice.call(arguments, 0).reduce((a,b) => a + b));
    }

    // curry函数后，参数等于原函数所需参数时执行
    function curry1(fn, length) {
        let len = length || fn.length,
            slice = Array.prototype.slice,
            args = [];
        
        function cb() {
            len = len - arguments.length;
            args = args.concat(slice.call(arguments, 0));
            if (len) {
                return cb;
            } else {
                return fn.apply(this, args);
            }
        }

        return cb;
    }


    let sum = curry1(getAdd);
    sum(1)(2)(3);

    function getMoney(...arg) {
        console.log(`花了${arg.reduce((a, b) => a + b)}块~`)
    }

    // curry函数后，调用时，不传参时执行
    function curry2(fn) {
        let args = [];
        return function cb(...arg) {
            if (arg.length === 0) {
                fn.apply(this, args);
            } else {
                args = args.concat(arg);
                return cb;
            }
        }

        return cb;
    }
    let money = curry2(getMoney);

    money(1)(2)(3)()
```
## MVC和MVVM
### mvc的简单实现
首先是实现Modal层，包含了数据的管理和更新视图的函数
html
```html
    <span bind="hour"></span> : <span bind="minute"></span> : <span bind="second"></span>
```
```javascript
    function Modal(value) {
        this._value = typeof value === 'undefined' ? '' : value;
        this.view = [];
    }

    Object.assign(Modal.prototype, {
        set: function (dom) {
            return function () {
                dom.innerHTML = this._value;
            }
        },

        watch: function (view) {
            this.view.push(this.set(view));
        },

        publish: function (value) {
            this._value = value;
            this.view.forEach(item => {
                item.call(this, this._value)
            })
        },
    })

    function Controller(callback) {
        var models = {};
        var views = Array.prototype.slice.call(document.querySelectorAll('[bind]'), 0);
        views.forEach(dom => {
            var modelName = dom.getAttribute('bind');
            (models[modelName] = models[modelName] || new Modal(0)).watch(dom);
        })
        callback.call(this, models)
    }

    new Controller(function (models) {
        function setTime() {
            var date = new Date();
            models.hour.publish(date.getHours());
            models.minute.publish(date.getMinutes());
            models.second.publish(date.getSeconds());
        }
        setTime();
        setInterval(setTime, 1000);
    });
```
## 跨域
> 使用jsonp跨域
因为同源策略是针对`ajax`请求，而`script`的请求是没有限制的
```javascript
    window.baidu = {
        sug: function name(data) {
            console.log('%c%s', 'color:#1781c5', '-----------------------------')
            console.log(data)
            console.log('%c%s', 'color:#1781c5', '-----------------------------')
        }
    }

    let scriptDom = document.createElement('script');
    scriptDom.src = 'http://suggestion.baidu.com/su?wd=123';

    document.querySelector('body').appendChild(scriptDom);
```
> cors跨域
当使用XMLHttpRequest发送请求时，浏览器如果发现违反了同源策略就会自动加上一个请求头:`origin`,后端在接受到请求后确定响应后会在Response Headers中加入一个属性:`Access-Control-Allow-Origin`,值就是发起请求的源地址，浏览器得到响应会进行判断`Access-Control-Allow-Origin`的值是否和当前的地址相同，只有匹配成功后才进行响应处理。
```javascript
// 前端
var xhr = new XMLHttpRequest();
xhr.onload = function(data){
  var _data = JSON.parse(data.target.responseText)
  for(key in _data){
    console.log('key: ' + key +' value: ' + _data[key]);
  }
};
xhr.open('POST','http://127.0.0.1:2333/cors',true);
xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
xhr.send();

// 后端
app.post('/cors',(req,res) => {
  if(req.headers.origin){
    res.writeHead(200,{
        "Content-Type": "text/html; charset=UTF-8",
        "Access-Control-Allow-Origin":'http://127.0.0.1:8888'
    });
    let people = {
        type : 'cors',
        name : 'weapon-x'
    }
    res.end(JSON.stringify(people));
  }
})
```

## 算法
### 数组去重
indexOf去重
```javascript
let array = [1,2,1,'11','1',2];

function uniqueByIndexOf(array) {
    let arr = [];
    array.forEach(item => {
        if (arr.indexOf(item) === -1) {
            arr.push(item);
        }
    })
    return arr;
}

function uniqueBySort(array) {
    var res = [];
    var array = array.sort();
    var seen;
    for (var i = 0, len = array.length; i < len; i++) {
        // 如果是第一个元素或者相邻的元素不相同
        if (!i || seen !== array[i]) {
            seen = array[i];
            res.push(array[i])
        }
    }
    return res;
}

function uniqueBySet(array) {
    return [...new Set([...array])]
}
```
### 排序
> sort
```javascript
let sort = (arr) => {
    return arr.sort((a, b) => {
        return a - b;
    })
}
```
> 快速排序
1. 在数据集之中，选择一个元素作为"基准"（pivot）。
2. 所有小于"基准"的元素，都移到"基准"的左边；所有大于"基准"的元素，都移到"基准"的右边。
3. 对"基准"左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。
```javascript
var swap = function (data, i, j) {
    var tmp = data[i];
    data[i] = data[j];
    data[j] = tmp;
};
var quickSort = (data, left, right) => {
    if (left < right) {
        let i = left,
            j = right + 1,
            len = data.length;
        while (true) {
            while (i + 1 < len && data[++i] < data[left]);
            while (j > -1 && data[--j] > data[left]);

            if (i >= j) break;

            swap(data, i, j);
        }

        swap(data, left, j);

        quickSort(data, left, j - 1);
        quickSort(data, j + 1, right);
    }
    return data;
};
```
> 冒泡排序
```javascript
let bubbleSort = (arr) => {
    let num = 1,
    pos = 0,
    len = arr.length;
    for (let i = 0; i < length; i++) {
        for(var j = 0; j < len - num; j++) {
            let pre = arr[j],
                next = arr[j + 1];
            if (pre > next) {
                arr[j] = next;
                arr[j + 1] = pre;
                pos = j;
            }
        }
        num = pos;
    }
}
```
> 插入排序
```javascript
let binaryInsertionSort = arr => {
    for(let i = 0, len = arr.length ; i < len; i++) {
        let j = i + 1;
        while(j > 0) {
            let right = arr[j],
            left = arr[j - 1];
            if (right > left) {
                arr[j - 1] = right;  
                arr[j] = left;  
            } else {
                break;
            }
            j--;
        }
    }
}
```
## ES6
let和cont
1. 没有变量提升
2. 不可重复声明
3. const赋值后不可变
4. 块级作用域

箭头函数
1. 没有arguments
2. 函数体没有大括号，隐式返回第一个语句
3. 函数内部`this`指向函数定义的位置

Promise
[Promise A+](http://www.ituring.com.cn/article/66566)

## 动画
> requestAnimationFrame
为什么会有requestAnimationFrame？原始状态使用定时器 `setTimeout` 和 `setInterval` 方法去刷新DOM或Canvas，由于页面可能有多个定时器，他无法保证帧速率达到显示器的刷新速率 `60帧/秒 (60FPS)`，并且只有当前面队列（代码）执行完毕且轮到了自己才能被执行。
[一段兼容性的代码](https://github.com/darius/requestAnimationFrame/)、[requestAnimationFrame API MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/requestAnimationFrame)
```javascript
// demo
var start = null;
var element = document.getElementById('SomeElementYouWantToAnimate');
element.style.position = 'absolute';

function step(timestamp) {
  if (!start) start = timestamp;
  var progress = timestamp - start;
  element.style.left = Math.min(progress / 10, 200) + 'px';
  if (progress < 2000) {
    window.requestAnimationFrame(step);
  }
}

window.requestAnimationFrame(step);
```

# 工具

## Babel配置
1. babel搭配webpack使用，需要安装`babel-loader、babel-core`，插件可以对使用es6的.js文件进行处理
2. 使用`babel-preset-es2015`可以对`es6`的语法进行打包处理成为`es5`语法，`babel-preset-stage-*`（0~4） 打包处于 strawman 阶段的语法，以上每种预设都依赖于紧随的后期阶段预设。例如，babel-preset-stage-1 依赖 babel-preset-stage-2，后者又依赖 babel-preset-stage-3。
3. `babel-polyfill`是对全局对象的原型添加`es6`的`API`，例如`Promise、Array.from`，缺点是污染全局变量。而`babel-runtime`的作用和它相同，它可以提供新的全局的方法，但是无法提供内置对象下`prototype`的方法。例如使用`Promise`时`const Promise = require('babel-runtime/core-js/promise')`即可。但是这样过于繁琐，并且多个模块的使用对造成代码的冗余，因此出现了`babel-plugin-transform-runtime`插件，它可以解决以上两个问题，使用时也不需要手动引入。
```javascript
// babel-runtime
module: {
  loaders: [{
    loader: 'babel',
    test: /\.jsx?$/,
    include: path.join(__dirname, 'src'),
    query: {
      plugins: ['transform-runtime'],
      presets: [
        'es2015', 
        'stage-0',
        'stage-1',
        'stage-2',
        'stage-3',
        'react',
      ],
    }
  }]
}
// babel-polyfill
entry: [
  'babel-polyfill',
  'src/index.jsx',
],

module: {
  loaders: [{
    loader: 'babel',
    test: /\.jsx?$/,
    include: path.join(__dirname, 'src'),
    query: {
      presets: [
        'es2015', 
        'stage-0',
        'stage-1',
        'stage-2',
        'stage-3',
        'react',
      ],
    }
  }]
}
```
