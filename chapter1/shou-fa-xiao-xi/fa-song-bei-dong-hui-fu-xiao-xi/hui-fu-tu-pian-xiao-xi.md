# 回复图片消息

```
<xml>
<ToUserName><![CDATA[toUser]]></ToUserName>
<FromUserName><![CDATA[fromUser]]></FromUserName>
<CreateTime>12345678</CreateTime>
<MsgType><![CDATA[image]]></MsgType>
<Image>
<MediaId><![CDATA[media_id]]></MediaId>
</Image>
</xml>
```

| 参数 | 是否必须 | 说明 |
| :--- | :--- | :--- |
| ToUserName | 是 | 接收方帐号（收到的OpenID） |
| FromUserName | 是 | 开发者微信号 |
| CreateTime | 是 | 消息创建时间 （整型） |
| MsgType | 是 | image |
| MediaId | 是 | 通过素材管理中的接口上传多媒体文件，得到的id。 |



