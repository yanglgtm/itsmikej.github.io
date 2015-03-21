---
layout: default
title:  "Curl协议指南"
author: itsmikej
---

Curl是一种命令行工具，作用是发出网络请求，然后得到和提取数据，显示在"标准输出"（stdout）上面。它支持多种协议。

### 基本

* `curl mikej.sinaapp.com` 返回源码
* `curl -o [另存文件名] mikej.sinaapp.com` 保存文件 相当于wget
* `-i` 显示头信息
* `-I` 只显示头信息
* `-v` 显示整个通信过程
* `--trace output.txt` 查看更详细的通信过程，同时记录到文件中
* `-X DELETE` curl默认的HTTP动词是GET，使用-X参数可以支持其他动词

### 发送和接收数据

  * get: 直接放到url参数上
  * post: `curl --data "data=xxx" url/action.php` 使用`--data-urlencode`可以对表单数据编码

### 进阶

{% highlight html %}
<form method="POST" enctype='multipart/form-data' action="upload.cgi">
  <input type=file name=upload>
  <input type=submit name=press value="OK">
</form>
{% endhighlight %}

* 上传：`curl --form upload=@localfilename --form press=OK [URL]` (构造上面信息)
* 添加Referer `curl --referer www.example.com www.referer.com`
* User Agent字段 `curl --user-agent "[User Agent]" [URL]`
* Cookie `curl --cookie "name=xxx" www.example.com` 即可发送Cookie
* 增加头信息 `curl --header "Content-Type:application/json" http://example.com`
* HTTP认证 `curl --user name:password example.com`
