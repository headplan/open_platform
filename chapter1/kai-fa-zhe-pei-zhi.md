# 开发者配置

可以参考接入指南中的接入指南 : [https://mp.weixin.qq.com/wiki?t=resource/res\_main&id=mp1421135319](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421135319)

或者读一读前面的入门指南 : [https://mp.weixin.qq.com/wiki?t=resource/res\_main&id=mp1472017492\_58YV5](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1472017492_58YV5)

准备好前面的内容 , 登录微信公众平台后台进入\[开发-&gt;基本配置\]选项 , 配置服务器了 .

#### 配置和启用服务

接入微信公众平台开发 , 开发者需要按照如下步骤完成 :

* 填写服务器配置
* 验证服务器地址的有效性
* 依据接口文档实现业务逻辑

**填写服务器配置**

登录微信公众平台官网后，在公众平台官网的开发-基本设置页面，勾选协议成为开发者，点击“修改配置”按钮，填写服务器地址（URL）、Token和EncodingAESKey，其中URL是开发者用来接收微信消息和事件的接口URL。Token可由开发者可以任意填写，用作生成签名（该Token会和接口URL中包含的Token进行比对，从而验证安全性）。EncodingAESKey由开发者手动填写或随机生成，将用作消息体加解密密钥。

同时，开发者可选择消息加解密方式：明文模式、兼容模式和安全模式。模式的选择与服务器配置在提交后都会立即生效，请开发者谨慎填写及选择。加解密方式的默认状态为明文模式，选择兼容模式和安全模式需要提前配置好相关加解密代码，[详情请参考消息体签名及加解密部分的文档](https://open.weixin.qq.com/cgi-bin/showdocument?action=dir_list&t=resource/res_list&verify=1&id=open1419318479&token=&lang=zh_CN)。

![](/assets/fuwuqkaifazhe.png)

**验证服务器地址的有效性**

也就是验证消息的确来自微信服务器 . 开发者提交信息后，微信服务器将发送GET请求到填写的服务器地址URL上，GET请求携带参数如下表所示 :

| 参数 | **描述** |
| :--- | :--- |
| signature | 微信加密签名，signature结合了开发者填写的token参数和请求中的timestamp参数、nonce参数。 |
| timestamp | 时间戳 |
| nonce | 随机数 |
| echostr | 随机字符串 |

开发者通过检验signature对请求进行校验（下面有校验方式）。若确认此次GET请求来自微信服务器，请原样返回echostr参数内容，则接入生效，成为开发者成功，否则接入失败。加密/校验流程如下：

* 将token、timestamp、nonce三个参数进行字典序排序
* 将三个参数字符串拼接成一个字符串进行sha1加密
* 开发者获得加密后的字符串可与signature对比，标识该请求来源于微信

简单的PHP代码示例

```
<?php
header('Content-type:text');
WechatCallbackApiTest::valid();

class WechatCallbackApiTest
{
    const TOKEN = 'FuckFair';
    public static function valid()
    {
        $echoStr = $_GET["echostr"];
        if(self::checkSignature()){
            echo $echoStr;
            exit;
        }
    }

    private static function checkSignature()
    {
        $signature = $_GET["signature"];
        $timestamp = $_GET["timestamp"];
        $nonce = $_GET["nonce"];

        $tmpArr = array(self::TOKEN, $timestamp, $nonce);
        sort($tmpArr);
        $tmpStr = implode( $tmpArr );
        $tmpStr = sha1( $tmpStr );

        if( $tmpStr == $signature ){
            return true;
        }else{
            return false;
        }
    }
}
```

提交代码到我们前面准备的新浪SAE服务器 , 并提交配置 , 就可以启用我们的服务器了 . 这里消息加解密我们暂时使用明文的方式 , 后期再详细介绍 .OK配置完毕 . 

**依据接口文档实现业务逻辑**

验证URL有效性成功后即接入生效，成为开发者。你可以在公众平台网站中申请微信认证，认证成功后，将获得更多接口权限，满足更多业务需求。

成为开发者后，用户每次向公众号发送消息、或者产生自定义菜单、或产生微信支付订单等情况时，开发者填写的服务器配置URL将得到微信服务器推送过来的消息和事件，开发者可以依据自身业务逻辑进行响应，如回复消息。

公众号调用各接口时，一般会获得正确的结果，具体结果可见对应接口的说明。返回错误时，可根据返回码来查询错误原因。[全局返回码说明](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1433747234)

用户向公众号发送消息时，公众号方收到的消息发送者是一个OpenID，是使用用户微信号加密后的结果，每个用户对每个公众号有一个唯一的OpenID。

此外，由于开发者经常有需在多个平台（移动应用、网站、公众帐号）之间共通用户帐号，统一帐号体系的需求，微信开放平台（[open.weixin.qq.com](http://open.weixin.qq.com/)）提供了UnionID机制。开发者可通过OpenID来获取用户基本信息，而如果开发者拥有多个应用（移动应用、网站应用和公众帐号，公众帐号只有在被绑定到微信开放平台帐号下后，才会获取UnionID），可通过获取用户基本信息中的UnionID来区分用户的唯一性，因为只要是同一个微信开放平台帐号下的移动应用、网站应用和公众帐号，用户的UnionID是唯一的。换句话说，同一用户，对同一个微信开放平台帐号下的不同应用，UnionID是相同的。详情请在微信开放平台的资源中心-移动应用开发-微信登录-授权关系接口调用指引-获取用户个人信息（UnionID机制）中查看。

另请注意，微信公众号接口必须以http://或https://开头，分别支持80端口和443端口。

这部分内容会在下一接以简单的例子进行讲解 . 



