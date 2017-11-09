# Python配置

前文是用PHP脚本完成的服务器验证配置 , 这里使用Python在SAE中进行配置测试 , 使用了webpy这个小框架 .

```py
# -*- coding: utf-8 -*-
# filename: index.wsgi
import sae
import web

urls = (
    '/', 'Handle'
)

class Handle(object):
    def GET(self):
        return "hello, this is a test"

app = web.application(urls, globals()).wsgifunc()
application = sae.create_wsgi_app(app)
```

访问以下 , 已经可以在浏览器看到"hello, this is a test"了 , 说明已经跑通了 . 下面就来验证我们的服务器 . 

修改之前的代码 , 引入验证类 . 

```py
# -*- coding: utf-8 -*-
# filename: index.wsgi

import hashlib
import sae
import web

urls = (
	'/', 'Handle'
)

class Handle(object):

    def GET(self):
        try:
            data = web.input()
            if len(data) == 0:
                return "hello, this is handle view"

            signature = data.signature
            timestamp = data.timestamp
            nonce = data.nonce
            echostr = data.echostr
            token = "testwx" # 自定义token

            list = [token, timestamp, nonce]
            list.sort()
            sha1 = hashlib.sha1()
            map(sha1.update, list)
            hashcode = sha1.hexdigest()
            print "handle/GET func: hashcode, signature: ", hashcode, signature
            if hashcode == signature:
                return echostr
            else:
                return ""
        except Exception, Argument:
            return Argument


app = web.application(urls, globals()).wsgifunc()
application = sae.create_wsgi_app(app)
```



