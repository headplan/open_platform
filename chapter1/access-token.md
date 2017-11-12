# Access Token

官方文档 : 

https://mp.weixin.qq.com/wiki?t=resource/res\_main&id=mp1421140183

在微信公众平台接口开发中，Access Token占据了一个很重要的地位，相当于进入各种接口的钥匙，拿到这个钥匙才有调用其他各种特殊接口的权限。

access\_token是微信公众账号的全局唯一票据，微信公众账号调用各接口时都需使用access\_token。正常情况下access\_token有效期为7200秒，重复获取将导致上次获取的access\_token失效。

微信公众账号可以使用AppID和AppSecret调用本接口来获取access\_token。AppID和AppSecret可在开发模式中获得（需要已经成为开发者，且帐号没有异常状态）。注意调用所有微信接口时均需使用https协议。





