# 消息体加解密

> 因为新浪SAE在未实名认证会影响加密验证 , 这里转用个人服务器 , 也方便打log

消息的交互已经可以了 , 我们前面还留下一个问题 , 就是配置服务器资源时的消息加解密方式问题 .

微信公众平台在配置服务器时，提供了3种加解密的模式供开发者选择，即明文模式、兼容模式、安全模式，选择兼容模式和安全模式前，需在开发者中心填写AES对称加密算法的消息加解密密钥EncodingAESKey。公众帐号用此秘钥对收到的密文消息体进行解密，回复消息体也用此秘钥加密。

* 明文模式：维持现有模式，没有适配加解密新特性，消息体明文收发，默认设置为明文模式
* 兼容模式：公众平台发送消息内容将同时包括明文和密文，消息包长度增加到原来的3倍左右；公众号回复明文或密文均可，不影响现有消息收发；开发者可在此模式下进行调试
* 安全模式（推荐）：公众平台发送消息体的内容只含有密文，公众账号回复的消息体也为密文，建议开发者在调试成功后使用此模式收发消息

> 微信官方提供的消息加解密接入指引 :
>
> [https://open.weixin.qq.com/cgi-bin/showdocument?action=dir\_list&t=resource/res\_list&verify=1&id=open1419318479&token=⟨=zh\_CN](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419318479&token=&lang=zh_CN)

我们先来看看指引的内容 :

开发者在代替授权公众号接收和处理消息时，出于安全考虑，必须对消息收发的过程进行必须的加解密。

> **开发者在接收消息和事件时，都需要进行消息加解密（某些事件可能需要回复，回复时也需要先进行加密）。但是，通过API主动调用接口（包括调用客服消息接口发消息）时，不需要进行加密。**

公众号第三方平台可能会接收到两种类型的消息 :

1、用户发送给公众号的消息（由公众号第三方平台代收）。此时，消息XML体中，ToUserName（即接收者）为公众号的原始ID（可通过《接口说明》中的获取授权方信息接口来获得）。

2、微信服务器发送给服务自身的事件推送（如取消授权通知，Ticket推送等）。此时，消息XML体中没有ToUserName字段，而是AppId字段，即公众号服务的AppId。这种系统事件推送通知（现在包括推送component\_verify\_ticket协议和推送取消授权通知），服务开发者收到后也需进行解密，接收到后只需直接返回字符串“success”。

下面是具体消息加解密的做法 , 当关注者与已授权公众号进行交互时，公众号第三方平台将接收到相应的消息推送、事件推送。为了加强安全性，将对此过程进行2个措施 :

1、在接收已授权公众号消息和事件的URL中，增加2个参数（此前已有2个参数，为时间戳 timestamp，随机数nonce），分别是encrypt\_type（加密类型，为aes）和msg\_signature（消息体签名，用于验证消息体的正确性）

2、postdata中的XML体，将使用第三方平台申请时的接收消息的加密symmetric\_key（也称为EncodingAESKey）来进行加密。

**下面开始动手实现**

更换了独立的服务器 , 我们先来修改一下日志方法 , 让我们更方便全面的追log

```php
public static function traceHttp()
{
   $content = date('Y-m-d H:i:s')."\nREMOTE_ADDR:".$_SERVER["REMOTE_ADDR"]."\nQUERY_STRING:".$_SERVER["QUERY_STRING"]."\n";
   $content .= file_get_contents("php://input")."\n\n";
   // 这里判断如果是SAE的应用,则使用其提供的函数打日志
   if (isset($_SERVER['HTTP_APPNAME']) == 'sae') {
      sae_set_display_errors(false);
      sae_debug(trim($content));
      sae_set_display_errors(true);
   }else {
      $max_size = 100000;
      $log_filename = "wxlog.log";
      if (file_exists($log_filename) and (abs(filesize($log_filename)) > $max_size)) {
         unlink($log_filename);
      }
      file_put_contents($log_filename, $content, FILE_APPEND);
   }
}
```

现在准备三个常量

```
// @param sToken: 第三方平台申请时填写的接收消息的校验token
// @param sEncodingAESKey: 第三方平台申请时填写的接收消息的加密symmetric_key
// @param sAppid: 公众号第三方平台的appid
```

当用户向公众账号发送消息时 , 微信公众账号将会在URL中带上signature、timestamp、nonce、encrypt\_type、msg\_signature等参数 :

```
signature=35703636de2f9df2a77a662b68e521ce17c34db4&timestamp=1414243737&nonce=1792106704&encrypt_type=aes&msg_signature=6147984331daf7a1a9eed6e0ec3ba69055256154
```

同时向该接口推送如下XML消息 , 即一个已加密的消息

```
<xml> 
  <ToUserName><![CDATA[gh_680bdefc8c5d]]></ToUserName>  
  <Encrypt><![CDATA[MNn4+jJ/VsFh2gUyKAaOJArwEVYCvVmyN0iXzNarP3O6vXzK62ft1/KG2/XPZ4y5bPWU/jfIfQxODRQ7sLkUsrDRqsWimuhIT8Eq+w4E/28m+XDAQKEOjWTQIOp1p6kNsIV1DdC3B+AtcKcKSNAeJDr7x7GHLx5DZYK09qQsYDOjP6R5NqebFjKt/NpEl/GU3gWFwG8LCtRNuIYdK5axbFSfmXbh5CZ6Bk5wSwj5fu5aS90cMAgUhGsxrxZTY562QR6c+3ydXxb+GHI5w+qA+eqJjrQqR7u5hS+1x5sEsA7vS+bZ5LYAR3+PZ243avQkGllQ+rg7a6TeSGDxxhvLw+mxxinyk88BNHkJnyK//hM1k9PuvuLAASdaud4vzRQlAmnYOslZl8CN7gjCjV41skUTZv3wwGPxvEqtm/nf5fQ=]]></Encrypt>
</xml>
```

获取这些参数和消息 , 使用官方提供的加密解密类即可 :

> 具体的加密解密方案可以查看
>
> [https://open.weixin.qq.com/cgi-bin/showdocument?action=dir\_list&t=resource/res\_list&verify=1&id=open1419318482&token=&lang=zh\_CN](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419318482&token=&lang=zh_CN)

```
# 代码查看Learning_WeiXin部分
```



