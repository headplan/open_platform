# 图片消息

```xml
<xml>
 <ToUserName><![CDATA[toUser]]></ToUserName>
 <FromUserName><![CDATA[fromUser]]></FromUserName>
 <CreateTime>1348831860</CreateTime>
 <MsgType><![CDATA[image]]></MsgType>
 <PicUrl><![CDATA[this is a url]]></PicUrl>
 <MediaId><![CDATA[media_id]]></MediaId>
 <MsgId>1234567890123456</MsgId>
</xml>
```

| 参数 | 描述 |
| :--- | :--- |
| ToUserName | 开发者微信号 |
| FromUserName | 发送方帐号（一个OpenID） |
| CreateTime | 消息创建时间 （整型） |
| MsgType | image |
| PicUrl | 图片链接（由系统生成） |
| MediaId | 图片消息媒体id，可以调用多媒体文件下载接口拉取数据。 |
| MsgId | 消息id，64位整型 |

接口调试工具 : 

[https://mp.weixin.qq.com/debug/cgi-bin/apiinfo?t=index&type=消息接口调试&form=图片消息](https://mp.weixin.qq.com/debug/cgi-bin/apiinfo?t=index&type=消息接口调试&form=图片消息)

