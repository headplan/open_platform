# Access Token

官方文档 :

[https://mp.weixin.qq.com/wiki?t=resource/res\_main&id=mp1421140183](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421140183)

在微信公众平台接口开发中，Access Token占据了一个很重要的地位，相当于进入各种接口的钥匙，拿到这个钥匙才有调用其他各种特殊接口的权限。

access\_token是微信公众账号的全局唯一票据，微信公众账号调用各接口时都需使用access\_token。正常情况下access\_token有效期为7200秒，重复获取将导致上次获取的access\_token失效。

微信公众账号可以使用AppID和AppSecret调用本接口来获取access\_token。AppID和AppSecret可在开发模式中获得（需要已经成为开发者，且帐号没有异常状态）。注意调用所有微信接口时均需使用https协议。

公众平台的API调用所需的access\_token的使用及生成方式说明：

1. 建议公众号开发者使用中控服务器统一获取和刷新Access\_token，其他业务逻辑服务器所使用的access\_token均来自于该中控服务器，不应该各自去刷新，否则容易造成冲突，导致access\_token覆盖而影响业务；
2. 目前Access\_token的有效期通过返回的expire\_in来传达，目前是7200秒之内的值。中控服务器需要根据这个有效时间提前去刷新新access\_token。在刷新过程中，中控服务器可对外继续输出的老access\_token，此时公众平台后台会保证在5分钟内，新老access\_token都可用，这保证了第三方业务的平滑过渡；
3. Access\_token的有效时间可能会在未来有调整，所以中控服务器不仅需要内部定时主动刷新，还需要提供被动刷新access\_token的接口，这样便于业务服务器在API调用获知access\_token已超时的情况下，可以触发access\_token的刷新流程。



