!> This document is no longer maintained and updated

# quick start :id=quick-start

The applet aggregates APP, email, and WeChat, and one line of code is easily pushed. At the same time, support the ability to view historical push records, subscribe to third-party push, generate temporary push links, etc.<br>


> This part of the document is for ordinary users

## install
Wechat searches for "iGot" or scans the miniprogram code below<br>
  ![Snap](./images/gh_38cb1ca0be75_344.jpg)

## Binding social accounts


### How to bind

> Go to my > push management > bind bar / bind mailbox / bind wechat < br >

 ![Snap](./images/bind.png)

### Bark
> Self-built servers are not supported for the time being

1. Open Bark and copy any push link in bark
2. Enter the widget Bark binding page and enter the push link (the software will automatically identify and paste)
3. Click on Binding to receive Binding Confirmation Push
4. Push information to complete binding

 ![Snap](./images/bindBark.png)

### E-mail
> It is suggested that `push@hellyw.com`、`wahao93@163.com` add to the whitelist or VIP list to avoid failing to receive mail messages

!> after binding the mailbox successfully, there will be page stroboscopic phenomenon. Please directly exit and delete the applet and re-enter This issue has be fixed in version 1.0.7.3

1. Open Bark, enter the widget mailbox binding page, and enter the mailbox address (the software will automatically identify and paste)
2. Click on Binding to receive Binding Confirmation Mail (no white list added, possibly in spam)
3. Click on the email `详情` to complete the binding

 ![Snap](./images/bindEmail.png)

### Wechat
> default :  `off` <br >
> Partial push may be lost or not delivered in this mode

1. Open Bark and enter the Wechat authorization page
2. Open or revoke authorization

## Using
> Users are allowed to push through a simple API interface, which basically inherits the Bark internal interface. Some extensions have been made to various push modes. <br >
> Some examples and illustrations of common operations in miniprogram

### API
> Support GET, POST 

```
/:key
/:key/:content 
/:key/:title/:content 
```
!> The key and host here are not key values provided in the bark

### params

> POST parameter

* title: the title will be displayed above the text < br >
* content: body < br >

!> the above parameters have a high priority. As a post request, the parameters in the URL will be overwritten The following parameters can also be called as post intrinsic parameters.

> base

* url：Click on the jump link `Bark can click directly; mail will be displayed with the details button; Wechat will not show.` <br >
* automaticallyCopy：automaticallyCopy `automaticallyCopy=1` <br >
* copy： copyed text <br >

> extension

wechat： use wechat push `wechat=1` <br >



## Q&A

### Push order

> Q1: Which Ways to Support Pushing Messages

* Users send through the API allocated in the applet, and support `Bark`, `Mail`, `Wechat`.

!> Push mode must be bound or authorized

* Third-party authorized applications only support `Bark`and `Mail`, but do not support Wechat push.

> Q2: Can push mode be specified ?

* Wechat push alone can be specified by parameters, and third-party authorized applications are not allowed.

> Q3: Is there a sequence of push? Can you specify it?

* Push order is to try `Bark`, invalid transfer `mail`. Wechat does not participate in the push sequence attempt

* Can not be specified temporarily, system default

### Authorization-related

> Q1: How to close the notification if you don't want to be disturbed

* You can choose to apply `close reminder`or `cancel authorization` to a specified authorization. 

!> Bark assistant API push will be sent through Bark assistant. Close the Bark helper reminder and you will not be able to receive messages pushed through the API above.

> Q2: What's the difference between turning off reminders and revoking authorizations?

* Close reminder: It will not be pushed through Bark or email, but it will still be recorded in Bark's assistant history. Ready to view

* Unauthorization: Authorized applications will not be able to push you information


# utools plug-in

To make it easier for computer clients to push messages to their phones, we've developed a plug-in version of bark assistant for the utools platform.
You can experience it in the following ways.

## Download and install utools

Download address: https://u.tools/download.html < br >

Documentation: https://u.tools/docs/guide/about-uTools.html < br >

> supports MACOS, WINDOWS, LINUX platforms <br>

## Install the Bark helper plug-in

Exhale utools (Option + Space, Alt + Space) and select <br> in the plug-in center

## Use

Exhale utools (Option + Space, Alt + Space) input 'push', 'bark', 'push' quickly enter <br>


# Developer Access
> This part of the document can help developers quickly access the Bark helper push platform

## Front end

1. Authorization allows navigate
```json
    "navigateToMiniProgramAppIdList":["wx74db71d8a9e3b699"]
```
2. navigate
```javascript
  wx.navigateToMiniProgram({
    appId: 'wx74db71d8a9e3b699',
    path: '/pages/bind/app',
    extraData: {
      appName: 'Bark Helper',    // required, change the name of your current widget
      openid: ''    // required,Modified to the openid of the current user
    },
    envVersion: 'release',
    success(res) {
      // success
    }
  })
  // openid Please use the true and valid openid
  // When the authorization is successful, the openid that the user binds to it is not modifiable
  // False openid will lead to messy information transmission
```
3. Receiving authorization results

> After the user's authorization succeeds or fails, the Bark assistant will return to the source applet. <br />
> You need to `App.onLaunch`or `App.onShow` to listen for `extradata`data from `appId:'wx74db71d8a9e3b699`.<br >
> It is recommended to monitor in `App.onShow`.<br />

The data format is：

```json
  "extraData":{
    "key":"",   //The authorized key (user key) of app is negligible
    "bind":true,   //Binding state Boolean
    "errMsg":""   //The error message ,  bind to false is returned
  }
```

demo：

```javascript
  onShow(event){
    if(event && event.referrerInfo && event.referrerInfo.appId === 'wx74db71d8a9e3b699'){
      const _extraData = ( event && event.referrerInfo && event.referrerInfo.extraData ) || {}
      if(_extraData.bind){
        //success
        console.log(_extraData.key)
      }else{
        //error
        console.log(_extraData.errMsg)
      }
    }
  }
```

now，binding was successful. The server can send push messages to the user through `appid` and `openid`.

## server

### Ready

> host: `https://push.hellyw.com/access/`

> appId: Appid for wechat allocation

### get appSerect

!> For the first request, appSerect can be airborne. Then the request needs to be passed to the original appSerect

```shell
  GET
  /security/getappserect/?appid=APPID&appserect=APPSERECT

```

```json
{
  "ret": 0, // 0 is right
  "data":{
      "appSerect":"rWsdahw4aj-04hbxjsa-1jbsaj"
    },
  "errMsg":"success"
}
```

### get ACCESS_TOKEN

```
  GET
  /security/getaccesstoken/?appid=APPID&appserect=APPSERECT

```

```json
{
  "ret": 0, // 0 is right
  "data":{
      "access_token": "sdahw4aj-04hbxjsa-1jb",
      "expire": 7200
    },
  "errMsg":"success"
}
```

!> Access_token is valid for 7200s, frequent refreshes will be rejected, remember to cache and refresh oh~~


### send message

#### single

* push

```
  POST
  /message/?openid=OPENID&access_token=ACCESS_TOKEN

```

```json
{
  "title" : "title", // required
  "content" : "content", // required
  "automaticallyCopy" : 1,
  "copyText" : "copyText",
  "url" : "url"
}

```

```json
{
  "ret": 0, // 0 is right
  "data":{
      "id": "5d19a088a5a60b536f514c69" //  message id
    },
  "errMsg":"success"
}
```

* checked

```
  GET
  /message/{id}?access_token=ACCESS_TOKEN
```

```json
{
  "ret": 0, // 0 is right
  "data":{
      "push": true, // push status
      "read": false, // read status
    },
  "errMsg":"success"
}
```

#### Group

> `Designing...`

### get ip white list

```
  GET
  /security/ip/?access_token=ACCESS_TOKEN

```

```json
{
  "ret": 0, // 0 is right
  "data":{
      "ipList": ["101.200.3.100","121.9.29.19"]
    },
  "errMsg":"success"
}
```

### set ip white list

!> In addition to the initial request, request the interface through the designated whitelist IP

```
  POST 
  /security/ip/?access_token=ACCESS_TOKEN
  
```

```json
{
  "ipList": ["101.200.3.100","121.9.29.19"]
}
```

!> To prevent misoperation, the initial request will add the request IP to the whitelist

```json
{
  "ret": 0, // 0 is right
  "data":{},
  "errMsg":"success"
}
```


# Version Conception

> Version design, the latest version is detailed in the latest version in the upgrade log. The large version of the idea will be implemented iteratively in smaller versions.

> The upgrade log and plan of the plug-in version of utools and other platforms will also be synchronized here, and the version number will be shared.

## v1.1.0 

### miniProgram

* ~~ Support message paging query, browse all messages (v1.0.2 implementation)~~
* Support message classification, search and query function [scheduling]
* ~~ Add multiple push modes (v1.0.3, v1.0.4 to realize mailbox and wechat push)~~
* ~~ Allow modification of key value in API to protect propagation privacy （discard,recommended to use temporary links） ~~
* Allow binding to bark build-in server key value [in evaluation]
* ~~ Allow the creation of temporary receive API interfaces [in evaluation]  (v1.0.7 implementation)~~
* Allow unbound mailbox, bark (will be supported in version 1.0.8)
* Allow custom push order (will be supported in version 1.0.8)
* Push access wechat service number (will be supported in version 1.0.9)

### uTools Plugin

* Rewrite page UI (return to 1.0.7 series version number upgrade)
* Support to modify and manage contacts (return to 1.0.7 series version number upgrade)


# CHANGE_LOGS

## v1.0.7.3 (2019-11-16)


* Fixed the problem of binding the flash of mailbox page
* The push logic code is rewritten, and the custom push receive order is supported. Page function will be implemented in version 1.0.8

## v1.0.7.2.1 (2019-11-01)

* uTools Plugin Release「1.0.1」

## v1.0.7 (2019-09-12)

* Add temporary link function, cancel at any time, protect privacy. Don't worry about the push interface being abused anymore ~~

> Entry: My > Experimental Functions > Temporary Links


## v1.0.6.5 (2019-09-02)

* Optimized interface and time display
* Optimize the friendly hints of Wechat push failure (inform by other means, push failure results)
* Optimized Mail Display Style
* Fixed bug that failed to load the data request on the history page

## v1.0.6.1 (2019-08-30)

* Fixed a bug that could not receive Wechat push when unbound bark and mailbox were not bound.

## v1.0.6.1 (2019-08-27)

* Fixed a bug that could not receive Wechat push when unbound bark and mailbox were not bound.

## v1.0.6 (2019-08-20)

* Push with link support opens link in the miniprogram  🎉🎉🎉🎉🎉🎉🎉🎉🎉

## v1.0.5 (2019-08-19)

* Added "Small Transmitter" (Custom Request Simulated Push)

## v1.0.4.1 (2019-08-15)

* Open third party access API

## v1.0.4 (2019-08-14) 🚩

> 50% grayscale Publishing

* Authorized push of Wechat is added, and push interface is unchanged. Add wechat parameter, `wechat = 1`, specify wechat push
* Added "My" module, more convenient management of the push side
* New Wechat Push Success Tips in List
* The next version will continue bug resolution and experience optimization, while standardizing and enriching the push interface for authorized applications

## v1.0.3.1 (2019-08-13)

* Compatible Bark Push and Mailbox Send
* This version may bring some bugs, such as usage problems. Welcome to ask questions

## v1.0.3 (2019-08-12)

* Increase mailbox binding, push interface unchanged. Supporting Intelligent Selection
* List Added Push Mode View
* After fixing the list push failure, it still prompts bark to push successfully
* The next version will continue bug solving and experiential optimization

## v1.0.2.1 (2019-08-08)

* Click on bark push to browse the push information automatically and finish reading at the same time. Avoid unread message error statistics

## v1.0.2 (2019-08-08)

* Allow one key to clear and one key to read
* Support browsing all information
* Cancel text auditing by pushing in-app api, text auditing is only for third-party apps
* Optimized some bugs

## v1.0.1 (2019-07-01)

* Increase the Complaint Function of Push Information
* Introducing scoring mechanism

## v1.0.0 (2019-06-30) 🚩

* Initial


# RELATED DOCUMENTS

* [另辟蹊径：离开模板消息，如何更优雅的向用户推送消息](https://developers.weixin.qq.com/community/develop/article/doc/000c06a47243a80aa7c8541e95b413)

