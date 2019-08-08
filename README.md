# Bark-MP-helper
Bark-MP-helper 是微信小程序端Bark插件，实现Bark推送历史的记录和查看
> Bark是什么？<br>
> Bark is an iOS App which allows you to push customed notifications to your iPhone <br>
> 客户端 <https://github.com/Finb/Bark> <br>
> 服务器端 <https://github.com/Finb/go-tools> <br>
> AppStore <https://itunes.apple.com/cn/app/bark-customed-notifications/id1403753865> <br>
## 写在前面
> 感谢Finb/Bark的软件支撑，Bark是一款很好的消息推送软件，改变了原来通过手机短信或邮件推送即时获取消息的方式。<br>
> Bark助手小程序的开发初衷是为了实现推送记录的管理。方便浏览不慎关闭的推送信息和过往的历史推送数据。<br>
> 在用户和Finb/Bark中间架设了一层服务，服务会存储用户推送的数据（用户删除后会永久从服务器删除）<br>
## 使用说明
1. 微信搜索“Bark助手” 或扫描下方小程序码<br>
![Snap](https://github.com/wahao/Bark-MP-helper/blob/master/images/gh_38cb1ca0be75_344.jpg)<br>
2. 复制bark内推送链接或bark推送key 进入小程序， 按提示绑定bark<br>
3. 点击绑定后，会收到确认绑定的推送信息，点开完成绑定
## 功能清单
1. 支持api推送 ， 接口格式同Bark api接口<br>
2. 支持历史记录查看<br>
3. 允许其他小程序通过授权方式向您的Bark推送消息 <br>
4. 管理授权应用及推送能力<br>
5. 文本过滤；涉及违法违规的文本推送将会被拦截<br>
## 接口文档

### 普通用户api推送
#### 发送推送
> 详情可见小程序内接口一项
```
可以发 get 或者 post 请求 ，请求成功会立即收到推送 

URL 组成: 第一个部分是 key , 之后有两个匹配 
/:key/:body 
/:key/:title/:body 

此处的key值及host 非Bark软件内host及key
```
#### 其他参数
```
包括url、automaticallyCopy、copy 均同Bark软件
```

### 其他小程序授权接入Bark助手帮助文档 
#### 1. 接入
> 添加至app.json
```javascript
    "navigateToMiniProgramAppIdList":["wx74db71d8a9e3b699"]
```
> 微信小程序代码内跳转至授权页
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
```
- 请填写真实有效的openid，以便于调用接口对指定用户发放。虚假的openid将导致信息发送错乱

> 接收授权结果 <br />
用户授权成功或失败后，Bark助手都将返回源小程序 <br />
您需要在`App.onLaunch`或`App.onShow`监听来自`appId: 'wx74db71d8a9e3b699'`的`extraData`数据<br >
数据格式为：<br />
```javascript
  "extraData":{
    "key":"",   //app的授权key
    "bind":true,   //绑定状态 Boolean
    "errMsg":""   //错误信息 bind为false是会返回
  }
```
建议在`App.onShow`内监听<br />
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
当bind为true时，表示授权成功<br />
#### 2. 推送消息
> 暂不支持群发
[API](https://github.com/wahao/Bark-MP-helper/blob/master/docs/api.md)

## 更新日志
[CHANGELOG](https://github.com/wahao/Bark-MP-helper/blob/master/CHANGELOG.md)
## 写在最后
> 如果您有更多更好的意见或建议，可以通过提issue的方式告诉我。
## 小程序推荐
![Snap](https://github.com/wahao/Bark-MP-helper/blob/master/images/gh_72a49c29672c_344.jpg)<br>
> Hacker密码： 一款采用对称加密、本地验证（加解密）的密码存储工具。

