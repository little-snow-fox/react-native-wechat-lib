<img height="200" src="./image/weixin.png?raw=true">

# React-Native-Wechat-Lib
![Version](https://img.shields.io/badge/Version-V3.0.0-brightgreen)
![npm version](https://img.shields.io/badge/npm-v1.1.24-blue)
![Wechat SDK](https://img.shields.io/badge/WechatSDKAndroid-V6.8.20-brightgreen)
![Wechat SDK](https://img.shields.io/badge/WechatSDKIos-V2.0-brightgreen)
![react version](https://img.shields.io/badge/react-v70-blue)

本库为 react-native 项目提供 Wechat SDK 支持

[React Native] bridging library that integrates WeChat SDKs:

- Android SDK 6.8.20
- iOS SDK 2.0

<br>

## 路线图
- 3.0.0
  - [X] React native 70
  - [X] Android SDK 6.8.20
  - [ ] iOS SDK 2.0
  - [ ] iOS SDK 2.0 No payment function
  - [X] Example
- 1.1.x
  - [X] React native 60
  - [X] Android SDK 5.5.6
  - [X] iOS SDK 1.8.7.1
  - [X] iOS SDK 1.8.7.1 No payment function

<br>

## 注意
如果你的 IOS 应用需要使用**不带支付功能**的 WeChat SDK，请使用带有 “-notpay” 后缀的 NPM 包。

目前最新代码版本为 3.0.0，但 NPM Last 版本暂时只到 1.1.26，因为 **3.0.0 暂时还处于开发阶段**，很多功能**未经过测试**。

如果你需要使用 3.0.0 版本，请在 package.json 中加上版本号 react-native-wechat-lib@3.0.0，切换前请你清楚了解该版本的风险，该版本为开发版。

我会尽快推出 3.0.0 发行版。

<br>

## 附言
本库由 [little-snow-fox](https://github.com/little-snow-fox) 发起。

希望各位大佬积极提交 PR，单靠我一个人维护工作量大。


<br>

## 目录

- [安装](#安装)
- [起步](#起步)
- [API 文档](#API文档)

<br>

## 安装
NPM 安装
```sh
npm install react-native-wechat-lib --save
# 3.0.0 开始弃用
react-native link react-native-wechat-lib
```
源码安装
```sh
git clone https://github.com/little-snow-fox/react-native-wechat-lib
cd react-native-wechat-lib
npm link
cd ../my-project
npm link react-native-wechat-lib
```
源码安装指定版本
```sh
git clone https://github.com/little-snow-fox/react-native-wechat-lib
cd react-native-wechat-lib
git checkout 1.1.x
npm link
cd ../my-project
npm link react-native-wechat-lib
```
<br>

## 起步

- [iOS 安装](./docs/build-setup-ios.md)
- [Android 安装](./docs/build-setup-android.md)
- [样例工程](./example)

<br>

## API 文档

本库支持 `TypeScript`，使用 `Promise` 或 `async/await` 来接收返回。

接口名称和参数尽量跟腾讯官网保持一致性，除了嵌套对象变成扁平对象，你可以直接查看腾讯文档来获得更多帮助。

#### registerApp(appid) 注册

- `appid` {String} the appid you get from WeChat dashboard
- returns {Boolean} explains if your application is registered done

This method should be called once globally.

```js
import * as WeChat from 'react-native-wechat-lib';

WeChat.registerApp('appid', 'universalLink');
```

#### isWXAppInstalled() 判断微信是否已安装

- returns {Boolean} if WeChat is installed.

Check if the WeChat app is installed on the device.

#### isWXAppSupportApi() 检查支持情况

- returns {Boolean} Contains the result.

Check if wechat support open url.

#### getApiVersion() 获取 API 版本号

- returns {String} Contains the result.

Get the WeChat SDK api version.

#### openWXApp() 打开微信

- returns {Boolean}

Open the WeChat app from your application.

#### sendAuthRequest([scope[, state]]) 微信授权登录

- `scope` {Array|String} Scopes of auth request.
- `state` {String} the state of OAuth2
- returns {Object}

Send authentication request, and it returns an object with the
following fields:

| field   | type   | description                         |
| ------- | ------ | ----------------------------------- |
| errCode | Number | Error Code                          |
| errStr  | String | Error message if any error occurred |
| openId  | String |                                     |
| code    | String | Authorization code                  |
| url     | String | The URL string                      |
| lang    | String | The user language                   |
| country | String | The user country                    |

#### ShareText(ShareTextMetadata) 分享文本

ShareTextMetadata

| name  | type   | description                    |
| ----- | ------ | ------------------------------ |
| text  | String | 分享文本                       |
| scene | Number | 分享到, 0:会话 1:朋友圈 2:收藏 |

Return:

| name    | type   | description                         |
| ------- | ------ | ----------------------------------- |
| errCode | Number | 0 if authorization succeed          |
| errStr  | String | Error message if any error occurred |

```js
import * as WeChat from 'react-native-wechat-lib';

WeChat.shareText({
  text: 'Text content.',
  scene: 0,
});
```

#### ShareImage(ShareImageMetadata) 分享图片

ShareImageMetadata

| name     | type   | description                    |
| -------- | ------ | ------------------------------ |
| imageUrl | String | 图片地址                       |
| scene    | Number | 分享到, 0:会话 1:朋友圈 2:收藏 |

Return:

| name    | type   | description                         |
| ------- | ------ | ----------------------------------- |
| errCode | Number | 0 if authorization succeed          |
| errStr  | String | Error message if any error occurred |

```js
import * as WeChat from 'react-native-wechat-lib';

WeChat.shareImage({
  imageUrl: 'https://google.com/1.jpg',
  scene: 0,
});
```

#### ShareLocalImage(ShareImageMetadata) 分享本地图片

ShareImageMetadata

| name     | type   | description                    |
| -------- | ------ | ------------------------------ |
| imageUrl | String | 图片地址                       |
| scene    | Number | 分享到, 0:会话 1:朋友圈 2:收藏 |

Return:

| name    | type   | description                         |
| ------- | ------ | ----------------------------------- |
| errCode | Number | 0 if authorization succeed          |
| errStr  | String | Error message if any error occurred |

#### ShareFile(ShareFileMetadata) 分享文件

ShareFileMetadata

| name  | type   | description    |
| ----- | ------ | -------------- |
| url   | String | 文件地址       |
| title | String | 文件标题       |
| scene | Number | 分享到, 0:会话 |

Return:

| name    | type   | description                         |
| ------- | ------ | ----------------------------------- |
| errCode | Number | 0 if authorization succeed          |
| errStr  | String | Error message if any error occurred |

```js
import * as WeChat from 'react-native-wechat-lib';

WeChat.shareFile({
  imageUrl: 'https://sdcard/test.png',
  title: '测试文件.pdf',
  scene: 0,
});
```

#### ShareMusic(ShareMusicMetadata) 分享音乐

ShareMusicMetadata

| name                | type   | description                           |
| ------------------- | ------ | ------------------------------------- |
| title               | String | 标题                                  |
| description         | String | 描述                                  |
| thumbImageUrl       | String | 缩略图地址，本库会自动压缩到 32KB     |
| musicUrl            | String | 音频网页的 URL 地址                   |
| musicLowBandUrl     | String | 供低带宽环境下使用的音频网页 URL 地址 |
| musicDataUrl        | String | 音频数据的 URL 地址                   |
| musicLowBandDataUrl | String | 供低带宽环境下使用的音频数据 URL 地址 |
| scene               | Number | 分享到, 0:会话 1:朋友圈 2:收藏        |

Return:

| name    | type   | description                         |
| ------- | ------ | ----------------------------------- |
| errCode | Number | 0 if authorization succeed          |
| errStr  | String | Error message if any error occurred |

```js
import * as WeChat from 'react-native-wechat-lib';

WeChat.shareMusic({
  title: 'Good music.',
  musicUrl: 'https://google.com/music.mp3',
  thumbImageUrl: 'https://google.com/1.jpg',
  scene: 0,
});
```

#### ShareVideo(ShareVideoMetadata) 分享视频

ShareVideoMetadata

| name            | type   | description                       |
| --------------- | ------ | --------------------------------- |
| title           | String | 标题                              |
| description     | String | 描述                              |
| thumbImageUrl   | String | 缩略图地址，本库会自动压缩到 32KB |
| videoUrl        | String | 视频链接                          |
| videoLowBandUrl | String | 供低带宽的环境下使用的视频链接    |
| scene           | Number | 分享到, 0:会话 1:朋友圈 2:收藏    |

Return:

| name    | type   | description                         |
| ------- | ------ | ----------------------------------- |
| errCode | Number | 0 if authorization succeed          |
| errStr  | String | Error message if any error occurred |

```js
import * as WeChat from 'react-native-wechat-lib';

WeChat.shareVideo({
  title: 'Interesting video.',
  videoUrl: 'https://google.com/music.mp3',
  thumbImageUrl: 'https://google.com/1.jpg',
  scene: 0,
});
```

#### ShareWebpage (ShareWebpageMetadata) 分享网页

ShareWebpageMetadata

| name          | type   | description                       |
| ------------- | ------ | --------------------------------- |
| title         | String | 标题                              |
| description   | String | 描述                              |
| thumbImageUrl | String | 缩略图地址，本库会自动压缩到 32KB |
| webpageUrl    | String | HTML 链接                         |
| scene         | Number | 分享到, 0:会话 1:朋友圈 2:收藏    |

Return:

| name    | type   | description                         |
| ------- | ------ | ----------------------------------- |
| errCode | Number | 0 if authorization succeed          |
| errStr  | String | Error message if any error occurred |

```js
import * as WeChat from 'react-native-wechat-lib';

WeChat.shareWebpage({
  title: 'Interesting web.',
  videoUrl: 'https://google.com/music.mp3',
  thumbImageUrl: 'https://google.com/1.jpg',
  scene: 0,
});
```

#### ShareMiniProgram(ShareMiniProgramMetadata) 分享小程序

ShareMiniProgram

| name            | type   | description                                                                        |
| --------------- | ------ | ---------------------------------------------------------------------------------- |
| title           | String | 标题                                                                               |
| description     | String | 描述                                                                               |
| thumbImageUrl   | String | 缩略图地址，本库会自动压缩到 32KB                                                  |
| userName        | String | 小程序的 userName，填小程序原始 id                                                 |
| path            | String | 小程序的页面路径                                                                   |
| hdImageUrl      | String | 小程序新版本的预览图二进制数据，6.5.9 及以上版本微信客户端支持                     |
| withShareTicket | String | 是否使用带 shareTicket 的分享                                                      |
| miniProgramType | Number | 小程序的类型，默认正式版，1.8.1 及以上版本开发者工具包支持分享开发版和体验版小程序 |
| webpageUrl      | String | 兼容低版本的网页链接                                                               |
| scene           | Number | 分享到, 0:会话 1:朋友圈 2:收藏                                                     |

Return:

| name    | type   | description                         |
| ------- | ------ | ----------------------------------- |
| errCode | Number | 0 if authorization succeed          |
| errStr  | String | Error message if any error occurred |

```js
import * as WeChat from 'react-native-wechat-lib';

WeChat.shareMiniProgram({
  title: 'Mini program.',
  userName: 'gh_d39d10000000',
  webpageUrl: 'https://google.com/show.html',
  thumbImageUrl: 'https://google.com/1.jpg',
  scene: 0,
});
```

#### LaunchMiniProgram (LaunchMiniProgramMetadata) 跳到小程序

LaunchMiniProgramMetadata

| name            | type   | description                                                                                                                |
| --------------- | ------ | -------------------------------------------------------------------------------------------------------------------------- |
| userName        | String | 填小程序原始 id                                                                                                            |
| miniProgramType | Number | 可选打开 开发版，体验版和正式版                                                                                            |
| path            | String | 拉起小程序页面的可带参路径，不填默认拉起小程序首页，对于小游戏，可以只传入 query 部分，来实现传参效果，如：传入 "?foo=bar" |

Return:

| name    | type   | description                         |
| ------- | ------ | ----------------------------------- |
| errCode | Number | 0 if authorization succeed          |
| errStr  | String | Error message if any error occurred |

```js
import * as WeChat from 'react-native-wechat-lib';

WeChat.launchMiniProgram({
  userName: 'gh_d39d10000000',
  miniProgramType: 1,
});
```

#### ChooseInvoice (ChooseInvoice) 选择发票

ChooseInvoice

| name      | type   | description |
| --------- | ------ | ----------- |
| cardSign  | String | 签名        |
| signType  | String | 签名类型    |
| timeStamp | Number | 当前时间戳  |
| nonceStr  | String | 随机字符串  |

Invoice

| name        | type   | description |
| ----------- | ------ | ----------- |
| appId       | String |             |
| cardId      | String | 发票 Id     |
| encryptCode | String | 加密串      |

Return:

| name    | type      | description                         |
| ------- | --------- | ----------------------------------- |
| errCode | Number    | 0 if authorization succeed          |
| cards   | Invoice[] | 发票数据                            |
| errStr  | String    | Error message if any error occurred |

```js
import * as WeChat from 'react-native-wechat-lib';

// ios 什么都不填都可以，android可以填写以下假的内容都可以正常运行，具体参数获取可以去看微信文档
WeChat.chooseInvoice({
  cardSign: 'cardSign',
  signType: 'SHA256',
  timeStamp: Date.now(),
  nonceStr: `${Date.now()}`,
});
```

#### pay(payload) 支付

- `payload` {Object} the payment data
  - `partnerId` {String} 商家向财付通申请的商家 ID
  - `prepayId` {String} 预支付订单 ID
  - `nonceStr` {String} 随机串
  - `timeStamp` {String} 时间戳
  - `package` {String} 商家根据财付通文档填写的数据和签名
  - `sign` {String} 商家根据微信开放平台文档对数据做的签名
- returns {Object}

Sends request for proceeding payment, then returns an object:

| name    | type   | description                         |
| ------- | ------ | ----------------------------------- |
| errCode | Number | 0 if authorization succeed          |
| errStr  | String | Error message if any error occurred |

#### subscribeMessage(SubscribeMessageMetadata) 一次性订阅消息

- returns {Object}

| name       | type   | description                                                                                                                                                                                                                           |
| ---------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| scene      | Number | 重定向后会带上 scene 参数，开发者可以填 0-10000 的整形值，用来标识订阅场值                                                                                                                                                            |
| templateId | String | 订阅消息模板 ID，在微信开放平台提交应用审核通过后获得                                                                                                                                                                                 |
| reserved   | String | 用于保持请求和回调的状态，授权请后原样带回给第三方。该参数可用于防止 csrf 攻击（跨站请求伪造攻击），建议第三方带上该参数，可设置为简单的随机数加 session 进行校验，开发者可以填写 a-zA-Z0-9 的参数值，最多 128 字节，要求做 urlencode |

#### 回调事件订阅

从小程序回到 APP，或者支付成功回到 APP 都会触发回调事件来返回相应信息，请在触发相应方法前提前添加事件队列。

```
    WeChat.registerApp(Global.APP_ID, Global.UNIVERSAL_LINK);
    DeviceEventEmitter.addListener('WeChat_Req', req => {
      console.log('req:', req)
      if (req.type === 'LaunchFromWX.Req') { // 从小程序回到APP的事件
        miniProgramCallback(req.extMsg)
      }
    });
    DeviceEventEmitter.addListener('WeChat_Resp', resp => {
      console.log('res:', resp)
      if (resp.type === 'WXLaunchMiniProgramReq.Resp') { // 从小程序回到APP的事件
        miniProgramCallback(resp.extMsg)
      } else if (resp.type === 'SendMessageToWX.Resp') { // 发送微信消息后的事件
        sendMessageCallback(resp.country)
      } else if (resp.type === 'PayReq.Resp') { // 支付回调
        payCallback(resp)
      }
    });
```

<br>

## License

MIT

Author: [little-snow-fox](https://github.com/little-snow-fox/react-native-wechat-lib)

<br>

## Sponsor
如果觉得本库还行，你愿意的话可以请我喝咖啡 ^_^ 。

<img src="./image/wepay.jpg" alt="wepay" width="380" />
<img src="./image/alipay.jpg" alt="alipay" width="380" />

