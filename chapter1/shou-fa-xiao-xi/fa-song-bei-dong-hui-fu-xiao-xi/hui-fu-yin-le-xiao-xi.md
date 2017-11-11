# 回复音乐消息

```
<xml>
<ToUserName><![CDATA[toUser]]></ToUserName>
<FromUserName><![CDATA[fromUser]]></FromUserName>
<CreateTime>12345678</CreateTime>
<MsgType><![CDATA[music]]></MsgType>
<Music>
<Title><![CDATA[TITLE]]></Title>
<Description><![CDATA[DESCRIPTION]]></Description>
<MusicUrl><![CDATA[MUSIC_Url]]></MusicUrl>
<HQMusicUrl><![CDATA[HQ_MUSIC_Url]]></HQMusicUrl>
<ThumbMediaId><![CDATA[media_id]]></ThumbMediaId>
</Music>
</xml>
```

| 参数 | 是否必须 | 说明 |
| :--- | :--- | :--- |
| ToUserName | 是 | 接收方帐号（收到的OpenID） |
| FromUserName | 是 | 开发者微信号 |
| CreateTime | 是 | 消息创建时间 （整型） |
| MsgType | 是 | music |
| Title | 否 | 音乐标题 |
| Description | 否 | 音乐描述 |
| MusicURL | 否 | 音乐链接 |
| HQMusicUrl | 否 | 高质量音乐链接，WIFI环境优先使用该链接播放音乐 |
| ThumbMediaId | 是 | 缩略图的媒体id，通过素材管理中的接口上传多媒体文件，得到的id |



