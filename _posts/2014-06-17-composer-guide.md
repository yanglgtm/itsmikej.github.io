---
layout: default
title:  "Composer使用指南"
author: itsmikej
---

## 安装

1. 下载：`curl -sS https://getcomposer.org/installer | php`，下载的`composer.phar`是一个二进制的可执行文件。
2. 全局安装：`mv composer.phar /usr/local/bin/composer`

## 基本使用

1. 先在`composer.json`配置要需要的包。
2. `php composer.phar install` 或者 `composer install`
3. 然后在代码中`require 'vendor/autoload.php';`即可使用。

## composer.json语法

{% highlight json %}
{
  "require": {
    "monolog/monolog": "1.11.*"
  },
  "autoload": {
    "psr-4": {"Acme\\": "src/"}
  }
}
{% endhighlight %}

包版本可以通过以下方法指定：

* 确切的版本号：`1.0.2`
* 范围：`>=1.0` `>=1.0,<2.0` `>=1.0,<1.1|>=1.2` `,`=`AND` `|`=`OR` AND优先级>OR
* 通配符：`1.0.*` 这个表示`>=1.0,<1.1`
* 赋值运算符：`~1.2` 表示下一个重要的版本都行，允许版本好`最后一位数字`上升。比如：`~1.2` 相当于 `>=1.2,<2.0`，而 `~1.2.3` 相当于 `>=1.2.3,<1.3`。

autoload 字段 可以注册自己的autoloader。

如上例：此时 src 会在你项目的根目录，与 vendor 文件夹同级。例如 src/Foo.php 文件应该包含 Acme\Foo 类。

也可以通过如下方法添加更多的autoloader。

{% highlight php %}
$loader = require 'vendor/autoload.php';
$loader->add('Acme\\Test\\', __DIR__);
{% endhighlight %}

PS: Composer 提供了自己的 autoloader。如果你不想使用它，你可以仅仅引入 `vendor/composer/autoload_*.php` 文件，它返回一个关联数组，你可以通过这个关联数组配置自己的 autoloader。


## composer.lock 锁文件

在安装依赖后，Composer 将把安装时确切的版本号列表写入 `composer.lock` 文件。这将锁定改项目的特定版本。

这是非常重要的，因为 `install` 命令将会检查锁文件是否存在，如果存在，它将下载指定的版本（忽略 `composer.json` 文件中的定义）。

这就保证了版本的稳定性，避免潜在错误对部署的影响。

但是如果包更新了版本，你将不会获得任何更新。此时可使用 `update` 命令。这将获取最新匹配的版本（根据你的 composer.json 文件）并将新版本更新进锁文件。`php composer.phar update`，也可以指定更新某些包`php composer.phar update monolog/monolog [...]`


## Composer命令

* 更新库：`composer update`
* 仅更新单个库：`composer update foo/bar` 当你编辑了composer.json文件。如果仅仅增加了一些描述(这会导致文件的md5值改变)，但应该是不打算更新任何库的，`composer update nothing`这条命令你会很有用，它不更新库，只更新lock文件。如果版本足够新，也可以使用`composer update --lock`
* 不编辑composer.json的情况下安装库：`composer require "foo/bar:1.0.0"`
* 优化性能：`composer dump-autoload --optimize`


## 资源

* [官方文档](https://getcomposer.org/)
* [packagist](https://packagist.org/)
* [使用技巧](http://segmentfault.com/a/1190000000355928)
