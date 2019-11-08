# Navigator对象

Navigator 对象保留了浏览器及其所处环境的基本信息。

## [navigator.onLine](https://developer.mozilla.org/zh-CN/docs/Web/API/NavigatorOnLine/onLine)

返回浏览器的联网状态。true 表示连接网络、false 表示断开网络。

并且可以通过 `window.onOnline` 和 `window.onOffline` 处理浏览器在联网状态的改变情况做出相应的改变。

```javascript
window.addEventListener("offline", _ => alert("offline"));

window.addEventListener("online", _ => alert("online"));
```

## [navigator.userAgent](https://developer.mozilla.org/zh-CN/docs/Web/API/NavigatorID/userAgent)

返回当前浏览器的用户代理（user agent）字符串。

可以用来判断当前浏览器所处的一些运行环境的信息。

```javascript
let mobileReg = /(iPhone|iPad|iPod|iOS|Android)/i;
let navUserAgent = navigator.userAgent;
let isAndroid = navUserAgent.indexOf('Android') > -1 || navUserAgent.indexOf('Adr') > -1;
let isiOS = !isAndroid && !!navUserAgent.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);
let isWechat = navUserAgent.match(/MicroMessenger/i) == 'micromessenger';
let isMobile = mobileReg.test(navUserAgent);

let platformInfos = {
    os: (!isAndroid  && !isiOS ) ? '' : isAndroid ? 'android' : 'ios',
    system: isMobile ? 'mobile' : 'pc',
    isWechat: isWechat
}
console.log(platformInfos);
```

## [navigator.language](https://developer.mozilla.org/zh-CN/docs/Web/API/NavigatorLanguage/language)

表示用户的首先语言，通常是浏览器用户界面的语言。

可以通过此属性，设置页面默认语言。

```javascript
// navigator.language 的值是 navigator.languages 属性返回数组的第一个元素
// 低版本 IE 不存在 navigator.language 可以使用 navigator.userLanguage
let bowerLanguage = navigator.languages ? navigator.languages[0] : (navigator.language || navigator.userLanguage);

window.addEventListener('load', function () {
    if (!!~bowerLanguage.indexOf('en')) {
        document.write('hello world~');
    } else if (!!~bowerLanguage.indexOf('zh')) {
        document.write('世界你好~');
    }
})
```

## [navigator.geolocations](https://developer.mozilla.org/zh-CN/docs/Web/API/Geolocation)

Geolocation 接口是一个用来获取设备地理位置信息，它可以让Web内容访问到设备的地理位置。

```javascript
let options = {
  enableHighAccuracy: true,
  timeout: 5000,
  maximumAge: 0
};

function success(pos) {
    let crd = pos.coords;

    console.log('经度: ' + crd.longitude);
    console.log('纬度 : ' + crd.latitude);
};

function error(err) {
    switch (error.code) {
        case 1:
            console.warn("位置服务被拒绝");
            break;
        case 2:
            console.warn("暂时获取不到位置信息");
            break;
        case 3:
            console.warn("获取信息超时");
            break;
        case 4:
            console.warn("未知错误");
            break;
    }
    console.warn(error.message)
};

navigator.geolocation.getCurrentPosition(success, error, options);
```

## [navigator.plugins](https://developer.mozilla.org/zh-CN/docs/Web/API/NavigatorPlugins/plugins)

返回一个 PluginArray 类型的对象, 包含了当前所使用的浏览器安装的所有插件。

```javascript
let plugin = name => {
    let hsaPlugin = false;
    name = name.toLocaleLowerCase();
    [...navigator.plugins].forEach(i => {
        !!~String(i.name.toLocaleLowerCase()).indexOf(name) && (hsaPlugin = true)
    })
    return hsaPlugin;
}

console.log(plugin('Flash'));
```