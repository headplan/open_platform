## 获取access\_token {#title}

公众号可以使用AppID和AppSecret调用本接口来获取access\_token。AppID和AppSecret可在“微信公众平台-开发-基本配置”页中获得（需要已经成为开发者，且帐号没有异常状态）。

**调用接口时，请登录“微信公众平台-开发-基本配置”提前将服务器IP地址添加到IP白名单中，点击查看设置方法，否则将无法调用成功。**

获取Access Token的接口 :

```
https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET
```

| 参数 | 是否必须 | 说明 |
| :--- | :--- | :--- |
| grant\_type | 是 | 获取access\_token填写client\_credential |
| appid | 是 | 第三方用户唯一凭证 |
| secret | 是 | 第三方用户唯一凭证密钥，即appsecret |

#### 返回数据说明

正常情况下，微信会返回下述JSON数据包给公众号：

```
{"access_token":"ACCESS_TOKEN","expires_in":7200}
```

| 参数 | 说明 |
| :--- | :--- |
| access\_token | 获取到的凭证 |
| expires\_in | 凭证有效时间，单位：秒 |

错误时微信会返回错误码等信息，JSON数据包示例如下（该示例为AppID无效错误）:

```
{"errcode":40013,"errmsg":"invalid appid"}
```

| 返回码 | 说明 |
| :--- | :--- |
| -1 | 系统繁忙，此时请开发者稍候再试 |
| 0 | 请求成功 |
| 40001 | AppSecret错误或者AppSecret不属于这个公众号，请开发者确认AppSecret的正确性 |
| 40002 | 请确保grant\_type字段值为client\_credential |
| 40164 | 调用接口的IP地址不在白名单中，请在接口IP白名单中进行设置 |

接口调试工具 : 

https://mp.weixin.qq.com/debug/cgi-bin/apiinfo?t=index&type=%E5%9F%BA%E7%A1%80%E6%94%AF%E6%8C%81&form=%E8%8E%B7%E5%8F%96access\_token%E6%8E%A5%E5%8F%A3%20/token

