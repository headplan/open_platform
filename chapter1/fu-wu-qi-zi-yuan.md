# 服务器资源

启用微信公众平台开发者中心需要拥有自己的服务器资源，用于存放自己开发的程序文件。服务器可以是一个虚拟空间、也可以是个云主机或云空间，只要这个空间支持程序的运行并且有域名可以访问。在开发好程序之后，需要把程序上传到服务器上，这样才能被微信访问到。

新浪SAE : [http://sae.sina.com.cn](http://sae.sina.com.cn/)

类似的还有百度云BAE : [https://console.bce.baidu.com/bae/](https://console.bce.baidu.com/bae/)

选择新浪云SAE做为本书讲解服务器资源，主要有以下几个明显的原因。

1. 免备案。申请成功一个新浪云应用之后，SAE提供了一个已备案的二级域名fbstudio.applinzi.com，二级域名是共享一级域名applinzi.com备案的。对于初学者来说，用于消息接口处理是完全够用的，如果想用于网页应用，当然还是建议使用自己的备案域名。
2. 后实名。SAE应用申请之后也要要求上传身份证进行实名认证的，不进行实名认证将会在输出页面插入代码，生成要求实名的提示浮层。但申请者可以先使用着，觉得有必要的时候再进行实名认证。而不需要等待实名通过后才能使用。
3. 后付费。SAE是使用云豆来进行资源计费的，但开发者可以先使用体验，如果觉得好用或在欠费之后再及时补交费用，而不需要先充值才能使用。

新浪微博账号登录 , 注册认证成功后 , 进入控制台的云应用SAE , 创建新应用 .

根据提示创建即可 , 运行环境选择标准环境无费用 .

![](/assets/sinasae.png)

创建完成后自动跳转到代码管理界面 . 点击创建版本 , 版本号默认为1 .

![](/assets/gitbanben.png)

至此，就成功创建了一个域名URL为[http://abc.sinaapp.com/](http://headplan.sinaapp.com/) 的SAE应用 , 新创建的版本还会生成一个以版本号命名的三级域名

[http://1.abc](http://1.headplan.sinaapp.com/)[.sinaapp.com/](http://1.headplan.sinaapp.com/) 后续不会使用 , 暂做了解 .

SAE支持Git、SVN、代码打包上传三种提交方式 , 这里随便使用了Git的方式 . 

**仓库信息**

| 仓库地址 | https://git.sinacloud.com/abc |
| :--- | :--- |
| 用户名 | abc@abc.com |
| 密码 | 您的安全密码 |

**Git代码部署说明**

    在你应用代码目录里，克隆git远程仓库
    $ git clone https://git.sinacloud.com/abc
    输入您的安全邮箱和密码。
    $ cd abc

    编辑代码并将代码部署到 `origin` 的版本1。
    $ git add .
    $ git commit -m 'Init my first app'
    $ git push origin 1



