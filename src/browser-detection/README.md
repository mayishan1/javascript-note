# 浏览器检测

为了网站可以在不同浏览器中运行，需要针对当前的运行环境进行检测，针对不同的检测结果，做出一些兼容的方案。

## 能力检测

不同的浏览器，对于 ECMAScropt 版本支持度不同。

在使用一些新的 API 时，往往需要事先检测，某些方法是否存在，然后在使用。即按需要加载新API，例如 `babel-runtime`;

或者在进入网站的时候直接进行初始化，将各种新的特性直接在 EAMAScript 对象上添加。即全量引入，例如 `babel-polyfill`;

例如使用 `Object.assign()` 如果 Object 下没有此方法，则进行添加

```javascript
if (typeof Object.assign != 'function') {
    Object.defineProperty(Object, "assign", {
        value: function assign(target, varArgs) {
            'use strict';
            if (target == null) {
                throw new TypeError('Cannot convert undefined or null to object');
            }

            var to = Object(target);

            for (var index = 1; index < arguments.length; index++) {
                var nextSource = arguments[index];

                if (nextSource != null) {
                    for (var nextKey in nextSource) {
                        if (Object.prototype.hasOwnProperty.call(nextSource, nextKey)) {
                            to[nextKey] = nextSource[nextKey];
                        }
                    }
                }
            }
            return to;
        },
        writable: true,
        configurable: true
    });
}
```

## 怪癖检测

常见于检测浏览的BUG

IE中存在一个bug，如果某个实例属性与标记为[[DontEnum]]的某个原型属性同名，那么该实例属性将不会出现在for-in循环当中。

```javascript

var hasDontEnumQuirk = function () {
    var o = {
        toString: function () {}
    };
    for (var prop in o) {
        if (prop == "toString") {
            return false;
        }
    }
    return true;
}();
```

Safari3.0以前版本会枚举被隐藏的属性。

```javascript
var hasEnumShadowsQuirk = function () {
    var o = {
        toString: function () {}
    };
    var count = 0;
    for (var prop in 0) {
        if (prop == "toString") {
            count++;
        }
    }
    return (count > 1);
}();
```

## 用户代理检测

“通过检测用户代理字符串来识别浏览器。用户代理字符串中包含大量与浏览器有关的信息，包括浏览器、平台、操作系统及浏览器版本。”

摘录来自: 泽卡斯. “JavaScript高级程序设计（第3版）。” iBooks.

[ua-device 浏览器信息检测插件](https//www.npmjs.com/package/ua-device)

[mobile-detect 浏览器信息检测插件(适用于移动端且体积小)](https://www.npmjs.com/package/mobile-detect)

```javascript
var ud = require('ua-device');
var md = require('mobile-detect');  // 仅适用用移动端  但是体积小

var udInstance = new ud(navigator.userAgent);
var mdInstance = new md(navigator.userAgent);
console.log(udInstance)
```