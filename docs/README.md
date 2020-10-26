# 请您知悉 :id=quick-start

iGot是一款聚合APP、邮箱、微信等多种方式的第三方推送平台，您可以用过一行代码、一次请求简单实现多种推送能力。

本文档面向普通用户、开发者分别进行介绍。如有疑惑，可通过文档结尾的[与我联系](#contactUs)方式进行反馈

# 👉 用户

## 微信小程序

### 获取您的推送key

* 获取微信小程序

微信搜索“iGot” 或扫描下方小程序码<br>
  ![微信搜索“iGot”](../images/gh_38cb1ca0be75_344.jpg)

* 获取您的推送key

进入接口，完成认证。点击上方复制按钮即可获取推送key

![获取您的推送key](../images/WechatIMG2084.jpeg)

### 发送您的第一条消息

主机地址：

```
https://push.hellyw.com
```

请求path：

```
GET

/:key/:content 
/:key/:title/:content 
```

```
POST

/:key
```

请求参数：

```
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

![收到推送](../images/WechatIMG2087.jpeg)



# 👉 开发者
