# 回复视频消息

```
<xml>
<ToUserName><![CDATA[toUser]]></ToUserName>
<FromUserName><![CDATA[fromUser]]></FromUserName>
<CreateTime>12345678</CreateTime>
<MsgType><![CDATA[video]]></MsgType>
<Video>
<MediaId><![CDATA[media_id]]></MediaId>
<Title><![CDATA[title]]></Title>
<Description><![CDATA[description]]></Description>
</Video> 
</xml>
```

| 参数 | 是否必须 | 说明 |
| :--- | :--- | :--- |
| ToUserName | 是 | 接收方帐号（收到的OpenID） |
| FromUserName | 是 | 开发者微信号 |
| CreateTime | 是 | 消息创建时间 （整型） |
| MsgType | 是 | video |
| MediaId | 是 | 通过素材管理中的接口上传多媒体文件，得到的id |
| Title | 否 | 视频消息的标题 |
| Description | 否 | 视频消息的描述 |



