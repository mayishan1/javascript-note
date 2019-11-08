# 理解对象

## 对象的创建

> 显示声明

```javascript
let obj = {
    key: 1
}
```

> Object()

```javascript
let obj = new Objcet({
    key: 1
})
```

> [Object.create()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)

```javascript
let obj = Object.create(null, {
    key: {
        value: 1,
        writable: true,
        enumerable: true,
        configurable: true
    }
});
```

> 构造函数

```javascript
function CreateObj(key) {
    this.key = key;
}

let obj = new CreateObj(1);
```

## 属性描述符

属性描述符中包含以下几个属性

> value

该属性的值(仅针对数据属性描述符有效)

> writable (Boolean)

属性的值可是否可以被改变。

> get (Function)

获取该属性的访问函数。

> set (Function)

设置该属性的设置函数。

> configurable (Boolean)

属性是否可以被删除。

> enumerable (Boolean)

属性是否可以被枚举。

Object 中的每个属性都有对应的属性描述符。

通过 `Object.getOwnPropertyDescriptor(targetObj, targetKey)` 可以获取对应属性的属性描述符。

```javascript
console.log(Object.getOwnPropertyDescriptor({key: 1}, 'key'))
```

`Object.defineProperty()` 方法可以修改对应属性的属性描述符。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="text"></div>
    <script>
    let textDom = document.getElementById('text');
    let obj = {
        text: '一'
    }

    textDom.innerHTML = obj.text;

    let configObjDescriptor = (obj, key) => {
        let value = obj[key];
        Object.defineProperty(obj, 'text', {
            configurable: false,
            set: function (newValue) {
                if (value === newValue) return;
                textDom.innerHTML = newValue;
                value = newValue;
            },
            get: function () {
                return value;
            }
        })
    }

    configObjDescriptor(obj, 'text');
    // 不可删除 返回 false
    console.log(delete obj.text);

    setTimeout(() => {
        obj.text = '二';
    }, 1000);

    </script>
</body>

</html>
```