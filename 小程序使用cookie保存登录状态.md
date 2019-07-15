# 小程序使用cookie保存登录状态

## 介绍

微信原生的wx.request网络请求接口并不支持传统的Cookie，但我们现有的后端接口依赖于Cookie（比如服务器用户的登录态），所以需要将Cookie定义在header中，每次向服务器发送请求是携带cookie就可以解决了。

## 步骤

1. 使用小程序的getStorageSync和setStorageSync函数对cookie进行读写。
2. 在每一次请求中，使用getStorageSync获取cookie（如果没有，则初始一个空cookie）并定义在header中发出。
3. 对于服务器的每一次响应，获取其返回的cookie并setStorageSync写入。

## 直接使用封装好的weapp-cookie包

1. 安装

```
npm install weapp-cookie --save

# 将 npm 包复制到 vendor 文件夹，避免小程序可能不能找到文件（tips：使用 wepy/mpvue 等框架无需此步）
cp -rf ./node_modules/ ./vendor/
```

2. 使用

以微信小程序为例，在小程序入口的app.js中引入即可

```
// app.js
import './vendor/weapp-cookie/dist/weapp-cookie'

// tips: 使用 wepy/mpvue 可以直接在入口 js 引入 weapp-cookie 模块
// import 'weapp-cookie'

App({
    onLaunch: function () { }
    // ...
})
```

然后直接调用wx.request函数，不需做任何修改，weapp-cookie会在底层自动代理wx.request的接口访问，实现cookie存取。

其它更多操作请参考[Github](https://github.com/charleslo1/weapp-cookie)传送门。
