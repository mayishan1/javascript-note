# History对象

history对象保存着用户上网的历史记录，从窗口被打开的那一刻算起。因为history是window对象的属性，因此每个浏览器窗口、每个标签页乃至每个框架，都有自己的history对象与特定的window对象关联。出于安全方面的考虑，开发人员无法得知用户浏览过的URL。不过，借由用户访问过的页面列表，同样可以在不知道实际URL的情况下实现后退和前进。

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）”。 iBooks.

## History前进与后退

> history.go()

使用go()方法可以在用户的历史记录中任意跳转，可以向后也可以向前。这个方法接受一个参数，表示向后或向前跳转的页面数的一个整数值。负数表示向后跳转（类似于单击浏览器的“后退”按钮），正数表示向前跳转（类似于单击浏览器的“前进”按钮）。

```javascript
history.go(1);

history.go(-1);
```

> history.forward()

使用forward()可以前进一个历史记录，类似于单击浏览器的“前进”按钮。

```javascript
history.forward();
```

> history.back()

使用 back() 可以后退一个历史记录，类似于单击浏览器的“后退”按钮。

```javascript
history.back();
```

## pushState和replaceState

`pushState` 和 `replaceState` 是 html5 新增的 API。

`pushState` 按指定的名称和URL（如果提供该参数）将数据push进会话历史栈。

`replaceState` 按指定的数据，名称和URL(如果提供该参数)，更新历史栈上最新的入口。

pushState、replaceState (这两个方法不会触发 popstate 事件，pushState 会生成新的历史条目，replaceState 不会生成新的历史条目) 搭配 popstate 同样可以做到单页应用

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

    <a route-href="/a" href="#">a</a>
    <a route-href="/b" href="#">b</a>
    <a route-href="/c" href="#">c</a>
    <hr>
    <view id="text"></view>

    <script>
    let textDom = document.getElementById('text');
    class HistoryRoutes {
        constructor() {
            this.links = document.querySelectorAll('a[route-href]');
            this.routes = {};
            this.defaultRoute = '';
            this.currentUrl = '';
        }

        updateView(pathname) {
            let currentUrl = pathname || window.location.pathname;

            if (currentUrl && currentUrl !== '/') {
                history.replaceState({} || {}, null, currentUrl);
                this.routes[currentUrl] && this.routes[currentUrl]();
                return;
            }
        }

        addRoute(url, cab) {
            this.routes[url] = cab;
            return this;
        }

        bindLink() {
            for (let i = 0; i < this.links.length; i++) {
                const current = this.links[i];
                current.addEventListener('click', e => {
                    e.preventDefault();

                    const url = current.getAttribute('route-href');
                    const data = current.getAttribute('data-route-data');

                    history.pushState({}, null, url);

                    this.updateView(url);
                })
            }
        }

        init() {
            this.bindLink();

            // 用户在点击浏览器的前进或后退 更新活动历史记录条目更改时 触发 onpopstate 事件，同时更新视图
            // 使用 history 的单页面应用，访问主页面的地址，在更新了 history 后， 用户在刷新了页面，浏览器把当前 url 发送到送到服务端，为了能够正常的显示页面的内容，需要在服务端nginx设置重定向 默认返回入口文件
            window.addEventListener('popstate', e => {
                this.updateView(window.location.pathname);
            });

            window.addEventListener('load', () => {
                this.updateView(this.defaultRoute || '/')
            }, false);
        }
    }

    let routes = new HistoryRoutes();

    routes.init();

    routes.addRoute('/a', function () {
        textDom.innerHTML = '页面 A';
    }).addRoute('/b', function () {
        textDom.innerHTML = '页面 B';
    }).addRoute('/c', function () {
        textDom.innerHTML = '页面 C';
    })

    routes.defaultRoute = '/a';

    </script>
</body>

</html>
```

[参考链接](https://github.com/happylindz/blog/issues/4)
