# 消息交互原理

现在我们以一个简单的功能来体验一下微信的消息交互流程 .

功能描述 :

关注公众号的粉丝给公众号一条文本消息 , 公众号立马回复一条文本消息给粉丝 , 不需要通过公众平台网页操作 .

我们先来看一下流程图 :

![](/assets/xiaoxiliuchengtu.png)

**接收文本消息**

也就是接收微信后台推送的xml数据 , 格式会是这样的

```
<xml>
<ToUserName><![CDATA[公众号]]></ToUserName>
<FromUserName><![CDATA[粉丝号]]></FromUserName>
<CreateTime>1460537339</CreateTime>
<MsgType><![CDATA[text]]></MsgType>
<Content><![CDATA[你好]]></Content>
<MsgId>6272960105994287618</MsgId>
</xml>
```

* CreateTime : 是微信公众平台记录粉丝发送该消息的具体时间
* MsgType : 用于标记该xml 是文本消息 , 一般用于区别判断
* Content : 说明该粉丝发给公众号的具体内容是"你好"
* MsgId : 是公众平台为记录识别该消息的一个标记数值, 微信后台系统自动产生

**被动回复文本消息**

即公众号给粉丝发送的文本消息 , 公众号想回复给粉丝一条文本消息 , 内容为“有事吗?” , 那么开发者发送给公众平台后台的内容也是xml的内容 , 格式如下 :

```
<xml>
<ToUserName><![CDATA[粉丝号]]></ToUserName>
<FromUserName><![CDATA[公众号]]></FromUserName>
<CreateTime>1460541339</CreateTime>
<MsgType><![CDATA[text]]></MsgType>
<Content><![CDATA[有事吗?]]></Content>
</xml>
```

标签含义也是一样的 .

**回复success问题**

查询官方wiki 开头强调 : , 如服务器无法保证在五秒内处理回复， 则必须回复"success"或者""\(空串\) , 否则微信后台会发起三次重试 .

解释一下为何有这么奇怪的规定 . 发起重试是微信后台为了尽可以保证粉丝发送的内容开发者均可以收到 . 如果开发者不进行回复 , 微信后台没办法确认开发者已收到消息 , 只好重试 . 三次重试后 , 依旧没有及时回复任何内容 , 系统自动在粉丝会话界面出现错误提示"该公众号暂时无法提供服务 , 请稍后再试" . 如果回复success , 微信后台可以确定开发者收到了粉丝消息 , 没有任何异常提示 . 因此请大家注意回复success的问题 .

**代码示例**

```php
public static function responseMsg()
{
    $post = file_get_contents('php://input');

    if (!empty($post)) {
        $postXml = simplexml_load_string($post, 'SimpleXMLElement', LIBXML_NOCDATA);
        $fromUserName = $postXml->FromUserName;
        $toUserName = $postXml->ToUserName;
        $content = trim($postXml->Content);
        $createTime = time();
        $textTpl = <<<TPL
            <xml>
                <ToUserName><![CDATA[%s]]></ToUserName>
                <FromUserName><![CDATA[%s]]></FromUserName>
                <CreateTime>%s</CreateTime>
                <MsgType><![CDATA[%s]]></MsgType>
                <Content><![CDATA[%s]]></Content>
                <FuncFlag>0</FuncFlag>
            </xml>
TPL;
        if ($content == '?' || $content == '？' || $content == '几点了') {
            $msgType = 'text';
            $withContent = '现在时间是:'.date('Y-m-d H:i:s',time());
            $result = sprintf($textTpl, $fromUserName, $toUserName, $createTime, $msgType, $withContent);
            echo $result;
            exit;
        }
    } else {
        echo 'success';
        exit;
    }
}
```

---

这里多提一下关于PHP接收POST数据的问题 : 

`$_POST`通过 HTTP POST 方法传递的变量组成的数组 , 是自动全局变量 . 

`$GLOBALS['HTTP_RAW_POST_DATA']`总是产生 $HTTP\_RAW\_POST\_DATA 变量包含有原始的 POST 数据 . 此变量仅在碰到未识别 MIME 类型的数据时产生 . 

通过解释 , 可以看到区别 , 此变量仅在碰到未识别 MIME 类型的数据时产生 . 也就是说PHP默认识别的数据类型是application/x-www.form-urlencoded标准的数据类型 , 无法识别时 , 可以用`$GLOBALS['HTTP_RAW_POST_DATA']`获取POST原始数据 , 比如我们前面微信用到的 text/xml 或者 soap 等等 , 但PHP新版的全局变量一般是Off的 , 所以建议使用`file_get_contents('php://input')`获取POST元数据 . 

注意 : `$GLOBALS['HTTP_RAW_POST_DATA']`和`file_get_contents('php://input')`不能用于enctype="multipart/form-data"





