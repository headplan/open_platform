# 全局配置

小程序根目录下的`app.json`文件用来对微信小程序进行全局配置 , 决定页面文件的路径、窗口表现、设置网络超时时间、设置多 tab 等 . 

#### 配置示例

以下是一个包含了部分常用配置选项的`app.json`

```js
{
  "pages": ["pages/index/index", "pages/logs/index"],
  "window": {
    "navigationBarTitleText": "Demo"
  },
  "tabBar": {
    "list": [
      {
        "pagePath": "pages/index/index",
        "text": "首页"
      },
      {
        "pagePath": "pages/logs/logs",
        "text": "日志"
      }
    ]
  },
  "networkTimeout": {
    "request": 10000,
    "downloadFile": 10000
  },
  "debug": true,
  "navigateToMiniProgramAppIdList": ["wxe5f52902cf4de896"]
}
```





