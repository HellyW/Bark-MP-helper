# Bark-MP-helper
## 服务端接口
> 接口会调整，因此请仅将其用于测试，现阶段接入小程序请勿发布具备授权能力的版本。
> 如有业务需求，请在issue内添加

### 推送消息

> 暂不支持群发
```
  https://push.hellyw.com/:appid/:openid/:title/:body

  appid：您当前小程序appid ； 非Bark助手的appid 
  openid：接入时传入的该用户openid 
  title：推送标题 ； 必填 ； 如 “消息提醒”，“最新股市”，“今日特价” ....
  body：消息主体 ； 必填 ； 
```