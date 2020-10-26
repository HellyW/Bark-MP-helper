# 请您知悉 :id=quick-start

iGot是一款聚合APP、邮箱、微信等多种方式的第三方推送平台，您可以通过一行代码、一次请求简单实现消息推送。

本文档面向普通用户、开发者分别进行介绍。如有疑惑，可通过文档结尾的[开放讨论](#contactUs)方式进行反馈

# 基本使用

## 微信小程序

### 获取您的推送key

1. 获取微信小程序

微信搜索“iGot” 或扫描下方小程序码<br>

<img src="./images/gh_38cb1ca0be75_344.jpg" style="width: 300px"/>

2. 获取您的推送key

进入接口，完成认证。点击上方复制按钮即可获取推送key 

<img src="./images/WechatIMG2084.jpeg" style="width: 300px"/>

!> 该推送key 为固定key。 认证申请后将无法进行更改。 如需分享，建议使用临时key。 详见[进阶教程 - 临时链接]

### 发送您的第一条消息

> 主机地址：

```
https://push.hellyw.com
```

> 请求path：

```
GET

/:key/:content 
/:key/:title/:content 
```

```
POST

/:key
```

> 请求参数：

```javascript
{
  "title": "请求标题",
  "content": "请求正文",
  // 推送携带的url
  "url": "https://www.baidu.com",
  // 自动复制； 为1自动复制； 默认为1
  "automaticallyCopy": 1,
  // 需要复制的文本内容
  "copy": "复制文本",
  // 目前仅在订阅消息下有效； 订阅者可通过推送主体选择是否接收消息
  "topic": "推送主题",
  // 其余参数， 其他请求参数会作为调试参数以json的形式显示在推送内容中。
  ...
}
```

e.g:

需要每天早上指定时间向我推送一条天气情况的消息，则可以通过下方请求方式实现。

```
POST

https://push.hellyw.com/5e6e29038c2eec6f24b26408

```
```json

{
  "title" : "今日天气预测",
  "content" : "今天是2021年11月15日，天气晴朗，最高气温为22℃，最低气温为3℃。适合外出活动。",
  "url": "http://www.weather.com.cn",
  "detail": {
    "temperatures": [{
        "hour": "00:00",
        "temperature": 3
      },{
        "hour": "09:00",
        "temperature": 10
      },{
        "hour": "12:00",
        "temperature": 22
      },{
        "hour": "14:00",
        "temperature": 18
      },{
        "hour": "18:00",
        "temperature": 9
      },{
        "hour": "22:00",
        "temperature": 5
      }]
  }
}
```

那么您将收到如下的一条推送消息

<img src="./images/WechatIMG2087.jpeg" style="width: 300px"/>

## Electron-iGot(桌面客户端)

Electron-iGot基于[iGot开放平台 - 客户端接口](#clientOpenApi)，在[Electron-iGot项目](https://github.com/wahao/Electron-iGot)内维护。可通过[国内gitee镜像](https://gitee.com/HellyW/Electron-iGot/releases)快速下载体验。

!> 目前已支持的登录方式为授权码登录。

## utools插件版

> 文档正在整理中
### 安装utools

下载地址 ： https://u.tools/download.html <br>

说明文档 ： https://u.tools/docs/guide/about-uTools.html <br>

### 安装插件

呼出utools （ Option + Space 、 Alt + Space ）进入插件中心搜索”iGot“安装 <br>

### 绑定iGot

调出utools， 输入 bind:igot:XXX  回车

!> 其中XXX为您获取的24位有效的推送key ； 使用该命令也可更新iGot推送key

### 发送消息

您可通过utools输入框完成发送，也可通过复制文本调起超级面板快捷发送

<img src="./images/WX20201026-115321.png" style="width: 300px"/>


## IOS快捷指令

您需要先安装apple官方的快捷指令app

### 安装快捷指令

浏览器输入 `https://jiejinghe.com/shortcuts/9371959496` 按指引完成操作

### 发送消息

点开该快捷指令或是选择文字 > 共享 > iGot推送

!> 关于iGot更多玩法可参考下方[外部链接](#shareHelpDocs)

# 进阶教程

> 文档正在整理中


# 开放平台

> 文档正在整理中

## 第三方授权使用

> 文档正在整理中

## 客户端接口 :id=clientOpenApi

> 文档正在整理中

# 外部链接 :id=shareHelpDocs

* [另辟蹊径：离开模板消息，如何更优雅的向用户推送消息](https://developers.weixin.qq.com/community/develop/article/doc/000c06a47243a80aa7c8541e95b413)

* [「小众工具」打通的不止是手机和电脑的任督二脉](https://mp.weixin.qq.com/s?__biz=MzAwMjg3ODU0NA==&mid=2247483749&idx=1&sn=aed399bb2ac2db084b053b4dbfb49e4a&chksm=9ac2fc5aadb5754c6e7a98da0b29d01dc10bb16e186d7d635e174502046adc5fb1d365657d30&mpshare=1&scene=23&srcid=1023zNEjBacq16jFX2Siz9p0&sharer_sharetime=1603432811199&sharer_shareid=9893d5f0ec65c0f471abe86f0743e12b%23rd)

# 开放讨论 :id=contactUs

[GITHUB](https://github.com/wahao/Bark-MP-helper)

[社区](https://support.qq.com/products/111465)

QQ群： 909540238
