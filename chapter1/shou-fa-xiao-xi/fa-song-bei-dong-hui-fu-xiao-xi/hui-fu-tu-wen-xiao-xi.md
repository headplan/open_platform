# 回复图文消息

```
<xml>
<ToUserName><![CDATA[toUser]]></ToUserName>
<FromUserName><![CDATA[fromUser]]></FromUserName>
<CreateTime>12345678</CreateTime>
<MsgType><![CDATA[news]]></MsgType>
<ArticleCount>2</ArticleCount>
<Articles>
<item>
<Title><![CDATA[title1]]></Title> 
<Description><![CDATA[description1]]></Description>
<PicUrl><![CDATA[picurl]]></PicUrl>
<Url><![CDATA[url]]></Url>
</item>
<item>
<Title><![CDATA[title]]></Title>
<Description><![CDATA[description]]></Description>
<PicUrl><![CDATA[picurl]]></PicUrl>
<Url><![CDATA[url]]></Url>
</item>
</Articles>
</xml>
```

| 参数 | 是否必须 | 说明 |
| :--- | :--- | :--- |
| ToUserName | 是 | 接收方帐号（收到的OpenID） |
| FromUserName | 是 | 开发者微信号 |
| CreateTime | 是 | 消息创建时间 （整型） |
| MsgType | 是 | news |
| ArticleCount | 是 | 图文消息个数，限制为8条以内 |
| Articles | 是 | 多条图文消息信息，默认第一个item为大图,注意，如果图文数超过8，则将会无响应 |
| Title | 是 | 图文消息标题 |
| Description | 是 | 图文消息描述 |
| PicUrl | 是 | 图片链接，支持JPG、PNG格式，较好的效果为大图360\*200，小图200\*200 |
| Url | 是 | 点击图文消息跳转链接 |



