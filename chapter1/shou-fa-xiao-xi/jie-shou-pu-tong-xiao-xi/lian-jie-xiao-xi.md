# 连接消息

```xml
<xml>
<ToUserName><![CDATA[toUser]]></ToUserName>
<FromUserName><![CDATA[fromUser]]></FromUserName>
<CreateTime>1351776360</CreateTime>
<MsgType><![CDATA[link]]></MsgType>
<Title><![CDATA[公众平台官网链接]]></Title>
<Description><![CDATA[公众平台官网链接]]></Description>
<Url><![CDATA[url]]></Url>
<MsgId>1234567890123456</MsgId>
</xml>
```

| 参数 | 描述 |
| :--- | :--- |
| ToUserName | 接收方微信号 |
| FromUserName | 发送方微信号，若为普通用户，则是一个OpenID |
| CreateTime | 消息创建时间 |
| MsgType | 消息类型，link |
| Title | 消息标题 |
| Description | 消息描述 |
| Url | 消息链接 |
| MsgId | 消息id，64位整型 |

接口调试工具 : 

https://mp.weixin.qq.com/debug/cgi-bin/apiinfo?t=index&type=%E6%B6%88%E6%81%AF%E6%8E%A5%E5%8F%A3%E8%B0%83%E8%AF%95&form=%E9%93%BE%E6%8E%A5%E6%B6%88%E6%81%AF

