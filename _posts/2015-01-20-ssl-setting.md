---
layout: default
title:  "Linux SSL配置"
author: itsmikej
---

## 1.申请证书

* 免费：[www.startssl.com][free]
* 付费：[www.namecheap.com][pay]

## 2.nginx配置

{% highlight Nginx config files %}
listen 443 ssl spdy default_server;
listen [::]:443 ssl spdy default_server ipv6only=on;

root /usr/share/nginx/www;
ssl_certificate /etc/nginx/certs/ssl.crt;
ssl_certificate_key /etc/nginx/certs/ssl.key.unsecure;
{% endhighlight %}

避免输入密码：
`openssl rsa -in /etc/nginx/certs/ssl.key -out /etc/nginx/certs/ssl.key.unsecure`

然后修改配置即可。

[free]: http://www.startssl.com/ "free"
[pay]: https://www.namecheap.com/security/ssl-certificates/comodo/positivessl.aspx
