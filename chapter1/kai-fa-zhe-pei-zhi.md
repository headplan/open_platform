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



