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

#### app.json 配置项列表

| 属性 | 类型 | 必填 | 描述 | 最低版本 |
| :--- | :--- | :--- | :--- | :--- |
| [pages](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#pages) | String Array | 是 | 页面路径列表 |  |
| [window](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#window) | Object | 否 | 全局的默认窗口表现 |  |
| [tabBar](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#tabbar) | Object | 否 | 底部`tab`栏的表现 |  |
| [networkTimeout](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#networktimeout) | Object | 否 | 网络超时时间 |  |
| [debug](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#debug) | Boolean | 否 | 是否开启 debug 模式，默认关闭 |  |
| [functionalPages](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#functionalpages) | Boolean | 否 | 是否启用插件功能页，默认关闭 | [2.1.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [subpackages](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#subpackages) | Object Array | 否 | 分包结构配置 | [1.7.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [workers](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#workers) | String | 否 | [`Worker`](https://developers.weixin.qq.com/miniprogram/dev/api/Worker.html)代码放置的目录 | [1.9.90](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [requiredBackgroundModes](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#requiredbackgroundmodes) | String Array | 否 | 需要在后台使用的能力，如「音乐播放」 |  |
| [plugins](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#plugins) | Object | 否 | 使用到的插件 | [1.9.6](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [preloadRule](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#preloadrule) | Object | 否 | 分包预下载规则 | [2.3.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [resizable](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#resizable) | Boolean | 否 | iPad 小程序是否支持屏幕旋转，默认关闭 | [2.3.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [navigateToMiniProgramAppIdList](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#navigatetominiprogramappidlist) | String Array | 否 | 需要跳转的小程序列表，详见[wx.navigateToMiniProgram](https://developers.weixin.qq.com/miniprogram/dev/api/wx.navigateToMiniProgram.html) | [2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [usingComponents](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#usingcomponents) | Object | 否 | 全局自定义组件配置 | 开发者工具 1.02.1810190 |
| [permission](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#permission) | Object | 否 | 小程序接口权限相关设置 | 微信客户端 7.0.0 |

---

##### pages

用于指定小程序由哪些页面组成 , 每一项都对应一个页面的路径+文件名信息 . 文件名不需要写文件后缀 , 框架会自动去寻找对于位置的`.json`,`.js`,`.wxml`,`.wxss`四个文件进行处理 .

**数组的第一项代表小程序的初始页面\(首页\) . 小程序中新增/减少页面 , 都需要对 pages 数组进行修改 . **

例如开发目录为 :

```
├── app.js
├── app.json
├── app.wxss
├── pages
│   │── index
│   │   ├── index.wxml
│   │   ├── index.js
│   │   ├── index.json
│   │   └── index.wxss
│   └── logs
│       ├── logs.wxml
│       └── logs.js
└── utils
```

则需要在app.json中写 :

```js
{
  "pages": ["pages/index/index", "pages/logs/logs"]
}
```

---

##### window

用于设置小程序的状态栏 , 导航条 , 标题 , 窗口背景色 . 

| 属性 | 类型 | 默认值 | 描述 | 最低版本 |
| :--- | :--- | :--- | :--- | :--- |
| navigationBarBackgroundColor | HexColor | \#000000 | 导航栏背景颜色，如`#000000` |  |
| navigationBarTextStyle | String | white | 导航栏标题颜色，仅支持`black`/`white` |  |
| navigationBarTitleText | String |  | 导航栏标题文字内容 |  |
| navigationStyle | String | default | 导航栏样式，仅支持以下值： `default`默认样式 `custom`自定义导航栏，只保留右上角胶囊按钮。参见注2。 | 微信客户端 6.6.0 |
| backgroundColor | HexColor | \#ffffff | 窗口的背景色 |  |
| backgroundTextStyle | String | dark | 下拉 loading 的样式，仅支持`dark`/`light` |  |
| backgroundColorTop | String | \#ffffff | 顶部窗口的背景色，仅 iOS 支持 | 微信客户端 6.5.16 |
| backgroundColorBottom | String | \#ffffff | 底部窗口的背景色，仅 iOS 支持 | 微信客户端 6.5.16 |
| enablePullDownRefresh | Boolean | false | 是否开启当前页面的下拉刷新。 详见[Page.onPullDownRefresh](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html#onpulldownrefresh) |  |
| onReachBottomDistance | Number | 50 | 页面上拉触底事件触发时距页面底部距离，单位为px。 详见[Page.onReachBottom](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html#onreachbottom) |  |
| pageOrientation | String | portrait | 屏幕旋转设置，仅支持`auto`/`portrait` 详见[响应显示区域变化](https://developers.weixin.qq.com/miniprogram/dev/framework/view/resizable.html) | 微信客户端 6.7.3 |





