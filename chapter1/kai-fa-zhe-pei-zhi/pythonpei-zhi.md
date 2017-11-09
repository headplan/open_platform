# Python配置

前文是用PHP脚本完成的服务器验证配置 , 这里使用Python在SAE中进行配置测试 , 使用了webpy这个小框架 . 

```
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



