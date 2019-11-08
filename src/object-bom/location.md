# Location对象

`Location` 接口表示其链接到的对象的位置（URL）。所做的修改反映在与之相关的对象上，例如对 location 对象上的某些属性进行修改，会导致浏览器刷新。 Document 和 Window 接口都有这样一个链接的Location，分别通过 `Document.location` 和 `Window.location` 访问

> location 属性

属性 | 例子 | 介绍
-- | -- | --
protocol | `http` 或 `https` | url 地址所使用的协议
host | 127.0.0.1:8080 | 服务器地址和端口号（如果有）
hostname | 127.0.0.1 | 服务器地址
port | 8080 | 端口号
pathname | /src/index.html | host 后面的资源路径，开头有一个 "/"
search | ?a=1&b=2 | URL中的参数，开头有一个 "?"
hash | #1/2/3 | URL中的 hash 值，开头有一个 "#"
href | 127.0.0.1:8080?a=1&b=2#1/2/3 | 页面完整的 URL

> location 方法

方法 | 介绍
-- | --
Location.assign() | 跳转到指定的URL
Location.reload() | 刷新当前页面
Location.replace() | 用给定的URL替换掉当前的资源（此方法不会生成新的历史记录）

## 应用

> 传递参数

根据URL的 search 向目标页传递参数

```javascript

// 传参
let addUrlSearch = (url, search) => {
    let serializeSearch = Object.keys(search).reduce((pre, cur) => `${pre}&${encodeURIComponent(cur)}=${encodeURIComponent(search[cur])}`, '').substr(1);
    return `${url}?${serializeSearch}`
}

// 获取参数
let getUrlSearch = (url) => {
    let urlSearch = location.search;
    let searchObj = {};

    if (urlSearch === '') return searchObj;

    urlSearch.substr(1).replace(/([^=&]+)=([^=&]+)(?=&|$)/g, function (match, $1, $2) {
        searchObj[decodeURIComponent($1)] = decodeURIComponent($2)
    })

    return searchObj;
}
```

> 根据 hash 路由的单页面应用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Route</title>
    <style>
    * {
        margin: 0px;
        padding: 0px;
    }
    </style>
</head>

<body>

    <a href="#a">a</a>
    <a href="#b">b</a>
    <a href="#c">c</a>
    <hr>
    <div id="text"></div>

    <script>

    let textDom = document.getElementById('text');
    class HashRoutes {
        constructor() {
            this.routes = {};
            this.defaultRoute = '';
            this.currentUrl = '';
        }

        updateView() {
            let currentHash = location.hash && location.hash.substr(1);
            if (currentHash) {
                this.routes[currentHash] && this.routes[currentHash]();
                return;
            }

            location.hash =  currentHash = this.defaultRoute;
        }

        addRoute(url, cab) {
            this.routes[url] = cab;
            return this;
        }

        setDefaultRoute(url) {
            this.defaultRoute = url;
        }

        init() {
            window.addEventListener('load', this.updateView.bind(this), false);
            // 浏览器的前进和后退同样会触发 hashchange
            window.addEventListener('hashchange', this.updateView.bind(this), false);
        }
    }

    let routes = new HashRoutes();

    routes.init();

    routes.addRoute('a', function () {
        textDom.innerHTML = '页面 A';
    })

    routes.addRoute('b', function () {
        textDom.innerHTML = '页面 B';
    })

    routes.addRoute('c', function () {
        textDom.innerHTML = '页面 C';
    })

    routes.setDefaultRoute('b');

    </script>
</body>
</html>
```

[参考链接](https://github.com/happylindz/blog/issues/4)