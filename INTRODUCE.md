## 
> Q1：您当前是如何实现您的消息推送的？
> Q2：您使用模板消息推送是否会遇到：
>         
>         1.  需要推送的对象涉及多个场景，需要被提醒多次？
>         2.  需要推送的时间点超出操作后7天时间范围？
>         
> Q3:  收集了足够的formId，最终频繁的推送导致客户无法接收到有效信息？
> 

当然模板消息的推送方式和限制是有问题的么？ 不  没有问题！但是依旧会有一些特殊场景需要突破模板消息的限制。

>   **被动接收：** A 提交工单到 B平台 ， B平台安排 C员工处理工单 需及时通知到C
>   

>   **反复提醒：** C员工接收到工单， B平台需向A 及时通告处理进度 [已派单 => 已出发 => 已到达 => 已处理 => 待评价 => 已接单 ]
>   

>   **长时间回复：** A 所提交工单为特殊工单，需指定于一周后安排上门实施
>   
这种场景下，C未操作平台B的小程序，B很难推送给C ； A只操作一次，B很难推送多条信息 ； 超过有效期 ， B 也很难推送信息给A 。

当然最后您也可以选择收集formId 、 邮件推送、短信提醒的方式。


### 言归正传
为了在微信小程序下，更好的解决这些问题。我们在小程序内引入了一种新的消息管理方式。通过集中的消息管理让用户自主选择信息。用户可以选择**强提醒**（app推送，立即接收）、**弱提醒**（消息收取但不提醒，感兴趣再查看）、**不提醒**（不接收任何消息）

![](https://mmbiz.qpic.cn/mmbiz_jpg/icWtmsIaSA69CyHTsibIXzplqso95e8IknUAnEC4JAYX8STEVrZAPCTBLeMjibuvu23XJsa1WicKaeEOh5GPlY5RUQ/0?wx_fmt=jpeg)
> 猝不及防的丢个菊花码给你  是他是他  就是他   干脆再甩个 [github](https://github.com/wahao/Bark-MP-helper/blob/master/README.md)吧

### 流程演示
下面给个Gif 感受一下， 第三方小程序通过授权接入，主动向授权成功用户推送消息的流程。 整个授权流程还是比较简单的。

![](https://mmbiz.qpic.cn/mmbiz_gif/icWtmsIaSA69CyHTsibIXzplqso95e8Ikn5QquxOdA7Qiacg5QAuyozzibnpDcw5oD5UEJe0YibGbsg0hbmZSnE87Ow/0?wx_fmt=gif)

> 说明：最后的推送是由申请授权的第三方小程序主动触发的
> 

### 如何接入

#### 1. 接入
> 添加至app.json
```javascript
    "navigateToMiniProgramAppIdList":["wx74db71d8a9e3b699"]
```
> 微信小程序代码内跳转至授权页
``` javascript
  wx.navigateToMiniProgram({
    appId: "wx74db71d8a9e3b699",
    path: "/pages/bind/app",
    extraData: {
      appName: "Bark Helper",    // 必填，修改为您当前小程序名称
      openid: ""    // 必填，修改为当前用户的openid
    },
    envVersion: "release",
    success(res) {
      // 打开成功
    }
  })
```
- **请填写真实有效的openid，以便于调用接口对指定用户发放。虚假的openid将导致信息发送错乱**

> 接收授权结果
用户授权成功或失败后，Bark助手都将返回源小程序
您需要在`App.onLaunch`或`App.onShow`监听来自`appId: 'wx74db71d8a9e3b699'`的`extraData`数据
数据格式为：
```javascript
  "extraData":{
    "key":"",   //app的授权key
    "bind":true,   //绑定状态 Boolean
    "errMsg":""   //错误信息 bind为false是会返回
  }
```
建议在`App.onShow`内监听
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
当bind为true时，表示授权成功


### 便捷接入

```
  接口仍在设计阶段，如需体验请使用以下测试接口。勿用于正式生产环境
```

>  接口文档：[github](https://github.com/wahao/Bark-MP-helper/blob/master/docs/api.md)
>  

### 用户保护
#### 1. 自由选择接收信息的权利
>  用户可以自由管理已授权的应用，主动屏蔽和解绑已授权的应用，实现消息免打扰和禁收消息
#### 2. 消息推送分离
> - 所有推送不会在微信消息内被提示，只会在小程序内被提示 
> - 对于用户想及时了解的应用可以通过app及时获取 
> - 消息免打扰开启后，该应用所有推送不会通过用户绑定的Bark进行推送，但消息仍会被小程序接收。需要打开小程序查看  
#### 3. 文本过滤
> 通过微信内容安全`security.msgSecCheck`接口对所有应用推送信息进行过滤
#### 4. 投诉与处罚
> 用户可在收到推送后，对推送信息发起投诉。投诉成立后，该应用会受到不同程度的处罚（扣分、临时封禁、永久封禁）。