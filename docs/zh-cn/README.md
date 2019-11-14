# 快速开始 :id=quick-start

该小程序将APP、邮箱、微信推送进行聚合，一行代码轻松实现推送。同时支持查看历史推送记录、订阅第三方推送、生成临时推送链接等能力<br>

> 该部分文档面向普通使用者

## 安装
微信搜索“Bark助手” 或扫描下方小程序码<br>
  ![Snap](../images/gh_38cb1ca0be75_344.jpg)

## 绑定社交账号

### 如何绑定

> 依次进入 我的 > 推送管理 > 绑定Bark / 绑定邮箱 / 绑定微信 <br >

 ![Snap](../images/bind.png)

### 绑定Bark
> 暂不支持自建服务器

1. 打开Bark，复制bark内任一推送链接
2. 进入小程序Bark绑定页，输入推送链接（软件会自动识别并粘贴）
3. 点击绑定，收到绑定确认推送
4. 点开推送信息，完成绑定

 ![Snap](../images/bindBark.png)


### 绑定邮箱
> 建议将邮箱`push@hellyw.com`、`wahao93@163.com` 添加至白名单或VIP名单，以免无法收到邮件信息

!> 绑定邮箱成功后会出现页面频闪现象，请直接退出并删除小程序后重新进入即可。  该问题将在1.0.7.3版本修复

1. 打开Bark，进入小程序邮箱绑定页，输入邮箱地址（软件会自动识别并粘贴）
2. 点击绑定，收到绑定确认邮件 （未添加白名单，可能在垃圾邮件内）
3. 点开邮件`详情`，完成绑定


 ![Snap](../images/bindEmail.png)


### 授权微信
> 默认授权为`关闭`
> 该模式下部分推送可能会丢失或无法送达

1. 打开Bark，进入微信授权页
2. 打开授权或取消授权

## 使用
> 允许用户通过简单的api接口进行推送，接口基本继承Bark内接口。对多种推送模式做了一些扩展 <br >
> 小程序内提供一些常见操作的示例及说明

### 接口
> 支持GET、POST请求

```
/:key
/:key/:content 
/:key/:title/:content 
```

!> 此处的key及host 并非bark内提供的key值

### 参数

> POST参数

* title：标题  会显示在正文上方 <br >
* content： 正文 <br >

!> 以上参数优先级高，作为post请求会覆盖url内参数。 以下参数也可作为post内参数调用。

> 基础参数

* url：点击跳转链接 `Bark推送可直接点击；邮件推送将以详情按钮展示；微信则不展示` <br >
* automaticallyCopy：自动复制 `automaticallyCopy=1` <br >
* copy： 复制文本 <br >

> 扩展参数

wechat： 指定微信发送 `wechat=1` <br >



## 说明

### 推送次序

> Q1: 支持哪些途径推送消息

* 用户通过小程序内分配的API发送，支持`Bark`、`邮件`、`微信` 三种方式

!> 推送方式必须被绑定或授权

* 第三方授权应用仅支持`Bark`、`邮件`两种方式 不支持微信推送

> Q2: 推送方式可以被指定么

* 仅微信推送可以通过参数指定，且第三方授权应用不被允许

> Q3: 推送有顺序么？可以指定么

* 推送顺序为尝试`Bark`，无效转`邮件`。其中微信不参与推送顺序尝试

* 暂时不可以指定，系统默认

### 授权相关

> Q1: 不想被打扰，如何关闭通知

* 您可以选择对指定授权应用`关闭提醒`或`取消授权` 

!> Bark助手API推送将通过Bark助手发出。 关闭Bark助手提醒，将无法收到通过上面API推送的信息

> Q2: 关闭提醒和取消授权有什么区别

* 关闭提醒：将不通过Bark或邮件推送，但仍会记录在Bark助手历史记录内。随时可以查看

* 取消授权：授权应用将无法向您推送信息

# utools插件

为方便电脑客户端向手机推送消息，我们开发了utools平台的bark助手插件版。您可以通过以下方式体验。

## 下载安装utools

下载地址 ： https://u.tools/download.html <br>

说明文档 ： https://u.tools/docs/guide/about-uTools.html <br>

> 支持 MACOS、WINDOWS、LINUX 平台 <br>

## 安装Bark助手插件 

 呼出utools （ Option + Space 、 Alt + Space ）在插件中心选择 <br>

## 使用 

呼出utools （ Option + Space 、 Alt + Space ） 输入 `push`、`bark`、`推送` 快速进入 <br>

# 开发者接入
> 该部分文档可帮助开发者快速接入Bark助手推送平台

## 微信小程序

1. 授权允许跳转
```json
    "navigateToMiniProgramAppIdList":["wx74db71d8a9e3b699"]
```
2. 跳转
```javascript
  wx.navigateToMiniProgram({
    appId: 'wx74db71d8a9e3b699',
    path: '/pages/bind/app',
    extraData: {
      appName: 'Bark Helper',    // 必填，修改为您当前小程序名称
      openid: ''    // 必填，修改为当前用户的openid
    },
    envVersion: 'release',
    success(res) {
      // 打开成功
    }
  })
  // openid 请填写真实有效的openid
  // 授权成功后， 该用户与之绑定的openid不可修改
  // 虚假的openid将导致信息发送错乱
```
3. 接收授权结果

> 用户授权成功或失败后，Bark助手都将返回源小程序 <br />
> 您需要在`App.onLaunch`或`App.onShow`监听来自`appId: 'wx74db71d8a9e3b699'`的`extraData`数据<br >
> 建议在`App.onShow`内监听<br />

数据格式为：

```json
  "extraData":{
    "key":"",   //app的授权key （ 用户key ），可忽略
    "bind":true,   //绑定状态 Boolean
    "errMsg":""   //错误信息 bind为false是会返回
  }
```

示例代码：

```javascript
  onShow(event){
    if(event && event.referrerInfo && event.referrerInfo.appId === 'wx74db71d8a9e3b699'){
      const _extraData = ( event && event.referrerInfo && event.referrerInfo.extraData ) || {}
      if(_extraData.bind){
        //绑定成功
        console.log(_extraData.key)
      }else{
        //绑定失败
        console.log(_extraData.errMsg)
      }
    }
  }
```

至此，小程序前端绑定成功。 服务端可通过 `appid` 和 `openid` 向该用户发送推送消息

## 服务端

### 准备开始

> host: `https://push.hellyw.com/access/`

> appId: 微信分配的appid

### 获取 appSerect

!> 初次请求，appSerect可传空。 之后请求需传原appSerect

```
  GET
  /security/getappserect/?appid=APPID&appserect=APPSERECT

```

```json
{
  "ret": 0, // 0 为正确响应
  "data":{
      "appSerect":"rWsdahw4aj-04hbxjsa-1jbsaj"
    },
  "errMsg":"success"
}
```

### 获取 ACCESS_TOKEN

```
  GET
  /security/getaccesstoken/?appid=APPID&appserect=APPSERECT

```

```json
{
  "ret": 0, // 0 为正确响应
  "data":{
      "access_token": "sdahw4aj-04hbxjsa-1jb",
      "expire": 7200
    },
  "errMsg":"success"
}
```

!> access_token有效期为7200秒，频繁刷新会被拒绝哦，记得缓存并刷新哦~~ 


### 推送消息

#### 指定对象推送

* 推送

```
  POST
  /message/?openid=OPENID&access_token=ACCESS_TOKEN

```

```json
{
  "title" : "推送标题", // 必填
  "content" : "推送的内容", // 必填
  "automaticallyCopy" : 1,// 可选
  "copyText" : "自定义拷贝文本",// 可选
  "url" : "详情链接"// 可选
}

```

```json
{
  "ret": 0, // 0 为正确响应
  "data":{
      "id": "5d19a088a5a60b536f514c69" // 发送成功 message id
    },
  "errMsg":"success"
}
```

* 查询结果

```
  GET
  /message/{id}?access_token=ACCESS_TOKEN
```

```json
{
  "ret": 0, // 0 为正确响应
  "data":{
      "push": true, // 推送状态
      "read": false, // 是否已读
    },
  "errMsg":"success"
}
```

#### 群发信息

> `设计中...`

### 获取IP白名单

```
  GET
  /security/ip/?access_token=ACCESS_TOKEN

```

```json
{
  "ret": 0, // 0 为正确响应
  "data":{
      "ipList": ["101.200.3.100","121.9.29.19"]
    },
  "errMsg":"success"
}
```

### 设置IP白名单

!> 除初次请求外，请通过已指定白名单ip请求该接口

```
  POST
  /security/ip/?access_token=ACCESS_TOKEN

```


```json
{
  "ipList": ["101.200.3.100","121.9.29.19"]
}
```

!> 为防止误操作，初次请求会将请求ip加入白名单

```json
{
  "ret": 0, // 0 为正确响应
  "data":{},
  "errMsg":"success"
}
```

# 版本构想

> 版本设计，当前最新版本详见升级日志内最新版本。构想的大版本将通过小版本迭代实现。

## v1.1.0

* ~~ 支持消息分页查询，可浏览所有消息 （v1.0.2 实现）~~
* 支持消息分类，检索查询功能 [排期]
* ~~ 增加多种推送方式（v1.0.3 、v1.0.4 实现邮箱及微信推送）~~
* ~~ 允许修改api内key值，保护传播隐私  （废弃，建议使用临时链接)~~
* 允许绑定bark自建服务器key值 [评估中]
* ~~允许创建临时接收api接口 [评估中] （v1.0.7 实现)~~


# 升级日志

## v1.0.7 (2019-09-12)

* 新增临时链接功能，随时取消，保护隐私。 再也不担心推送接口被人滥用了 ~~ 

> 入口： 我的 > 实验性功能 > 临时链接

## v1.0.6.5 (2019-09-02)

* 优化了界面和时间显示
* 优化了微信推送失败的友好提示（通过其他方式告知，推送失败结果）
* 优化了邮件显示样式
* 解决了首页加载数据请求失败的bug


## v1.0.6.1 (2019-08-30)

* 修复了未绑定bark及邮箱时，无法接收微信推送的错误


## v1.0.6 (2019-08-20)

* 推送的链接支持在小程序内打开啦  🎉🎉🎉🎉🎉🎉🎉🎉🎉

## v1.0.5 (2019-08-19)

* 增加“小小发报机”（自定义请求模拟推送）

## v1.0.4.1 (2019-08-15)

* 开放第三方接入api

## v1.0.4 (2019-08-14) 🚩

> 50%灰度发布

* 增加微信授权推送，推送接口不变。新增wechat参数，`wechat=1`指定微信推送
* 增加“我的”模块，更便捷管理推送端
* 列表新增微信推送成功提示项
* 下一版本将持续bug解决和体验优化，同时规范和丰富授权应用的推送接口

## v1.0.3.1 (2019-08-13)

* 兼容bark推送及邮箱发送
* 这一版本可能带来一些bug，如有使用问题。欢迎提issue

## v1.0.3 (2019-08-12)

* 增加邮箱绑定，推送接口不变。支持智能选择
* 列表增加推送模式查看
* 修复了列表推送失败后仍提示bark推送成功
* 下一版本将持续bug解决和体验优化

## v1.0.2.1 (2019-08-08)

* 点击bark推送自动浏览推送信息，同时完成阅读。避免未读消息错误统计

## v1.0.2 (2019-08-08)

* 允许一键清空、一键已读
* 支持浏览全部信息
* 通过应用内api推送取消文本审核，文本审核仅针对第三方app
* 优化了一些bug

## v1.0.1 (2019-07-01)

* 增加对推送信息的投诉功能
* 引入评分机制

## v1.0.0 (2019-06-30) 🚩

* 首发

# 其他文档

* [另辟蹊径：离开模板消息，如何更优雅的向用户推送消息](https://developers.weixin.qq.com/community/develop/article/doc/000c06a47243a80aa7c8541e95b413)

