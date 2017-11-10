# 文本消息

```xml
<xml>
<ToUserName><![CDATA[toUser]]></ToUserName>
<FromUserName><![CDATA[fromUser]]></FromUserName>
<CreateTime>1348831860</CreateTime>
<MsgType><![CDATA[text]]></MsgType>
<Content><![CDATA[this is a test]]></Content>
<MsgId>1234567890123456</MsgId>
</xml>
```

| 参数 | 描述 |
| :--- | :--- |
| ToUserName | 开发者微信号 |
| FromUserName | 发送方帐号（一个OpenID） |
| CreateTime | 消息创建时间 （整型） |
| MsgType | text |
| Content | 文本消息内容 |
| MsgId | 消息id，64位整型 |

接口调试工具 :

[https://mp.weixin.qq.com/debug/cgi-bin/apiinfo?t=index&type=消息接口调试&form=文本消息](https://mp.weixin.qq.com/debug/cgi-bin/apiinfo?t=index&type=消息接口调试&form=文本消息)

