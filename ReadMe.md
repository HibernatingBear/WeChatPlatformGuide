# 实现微信小程序之间的跳转并传递参数
## 前置条件
要实现微信小程序之间的跳转并传递参数，需要有两个微信小程序，并且这两个小程序在微信公众平台上已经进行了绑定。需要确保安装了最新版的微信客户端。

## 步骤
下面介绍在源小程序中如何跳转到目标小程序，并传递参数。

1. 在源小程序的某个页面中，如一个按钮上，添加点击事件。

`<button bindtap="navigateToMiniProgram">跳转到目标小程序</button>`

2. 在相应的.js文件中，编写跳转到子程序的函数，通过调用wx.navigateToMiniProgram方法来实现。

``` 
navigateToMiniProgram: function() {
  wx.navigateToMiniProgram({
    appId: '目标小程序的AppID',
    path: '要跳转到的页面路径',
    extraData: {
      key1: 'value1', //要传递的参数
      key2: 'value2'
    },
    success(res) {
      //跳转小程序成功的逻辑
    },
    fail(res) {
      //跳转小程序失败的逻辑
    }
  })
}
```
- appId：目标小程序的 AppID，可以在微信公众平台获取。
- path：要跳转到的页面路径。
- extraData：传递给目标小程序的数据，是一个 JSON 对象。
3. 在目标小程序中必须实现 App.onLaunch 和 App.onShow 方法，才能获取传递过来的参数。

``` 
///app.js文件中代码片段
App({
  onLaunch: function(options) {
    console.log(options.query.key1) //输出value1
    console.log(options.query.key2) //输出value2
  },
  onShow: function(options) {
    console.log(options.query.key1) //输出value1
    console.log(options.query.key2) //输出value2
  }
})
```
options 对象是一个包含着小程序跳转所携带的信息的对象，其中包含了通过 extraData 传递的数据。这里演示了获取两个参数，key1 和 key2。

### 注意事项
- 注意小程序之间的绑定关系，确保目标小程序已经在微信公众平台上配置了正确的信息。
- 目标小程序需要在 App.onLaunch 和 App.onShow 方法中处理传递过来的参数。
- 微信小程序仅能跳转到其它微信小程序，不能跳转到网页或App中。