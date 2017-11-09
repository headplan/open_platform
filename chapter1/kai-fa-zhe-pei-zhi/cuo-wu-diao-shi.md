# 错误调试

之前我们在配置验证服务器提交URL和Token的时候 , 有时候会碰到提交不成功的情况 , 具体有以下几种 :

* 请求URL超时 - 这种情况一般是由于服务器网速或响应速度太慢。可以先重试几次或者等一段时间再来试，如果还是这样，则需要考虑更换速度更快、性能更好的服务器。
* 系统发生错误 , 请稍后重试 - 这种情况一般是由于微信服务器短时内的异常引起的，一般重试或者过一段时间尝试即可。
* Token验证失败 - 这里就是我们需要控制的 . 

#### Token验证失败

这种情况需要具体分析验证过程被卡在哪一个环节了 , 可以通过调用变量$\_SERVER来获取服务器和执行环境信息来分析 .

> 了解更多关于$\_SERVER的信息 , 可访问官方网站 :
>
> [http://www.php.net/manual/zh/reserved.variables.server.php](http://www.php.net/manual/zh/reserved.variables.server.php)

这里我们需要使用以下两个元素 :

$\_SERVER\['REMOTE\_ADDR'\] : 来访者的IP地址 , 此处为微信服务器的IP

$\_SERVER\['QUERY\_STRING'\] : 查询请求字符串，此处为微信服务器发过来的GET请求字符串

OK , 现在我们就可以写一个方法 , 将上面两个服务器变量打到日志中去了 .

```php
public static function traceHttp()
{
   $content = date('Y-m-d H:i:s')."\nREMOTE_ADDR:".$_SERVER["REMOTE_ADDR"]."\nQUERY_STRING:".$_SERVER["QUERY_STRING"]."\n\n";

   // 这里判断如果是SAE的应用,则使用其提供的函数打日志
   if (isset($_SERVER['HTTP_APPNAME'])) {
      sae_set_display_errors(false);
      sae_debug(trim($content));
      sae_set_display_errors(true);
   }else {
      $max_size = 100000;
      $log_filename = "log.xml";
      if (file_exists($log_filename) and (abs(filesize($log_filename)) > $max_size)) {
         unlink($log_filename);
      }
      file_put_contents($log_filename, $content, FILE_APPEND);
   }
}
```

下面可以得出结出初步结论 : 

如果没有生成日志文件：微信服务器没有访问到你的服务器，需要先检查一下你的服务器是否可以通过公网访问，以及URL路径是否正确并且可以访问，如果公网访问而微信服务器不能访问，那可能是防火墙拦截了80端口或微信服务器IP，也可能服务器所在区域与微信服务器通讯不畅，需要更换服务器。

如果已经生成日志文件：查看REMOTE\_ADDR和QUERY\_STRING内容是否和上述类似。确认signature、timestamp、nonce、echostr四个参数都有值。如果这些都没有问题，则检查程序中定义的Token值是否和提交的一致，再检查程序流程及数据处理是否和官方文档描述的一致。

