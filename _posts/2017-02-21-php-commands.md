---
title: 把博客建立在 github pages 上
date: 2017-02-21
updated: 2017-02-21
tags: php commands
---

```php
php -i
```
查看phpinfo

```
php -v
```
显示PHP版本

```
php -m
```
查看PHP安装了哪些扩展模块，可修改php.ini添加删除扩展模块。

编译PHP时内置的扩展，无法通过修改php.ini删除
```
php -S
```
启动一个内置的Web服务器，用于开发环境内进行程序的调试。

```
php -S 0.0.0.0:9000
```
内置的Web服务器是一个全功能的Http服务器，在开发模式下可以取代apache,nginx+php-fpm，但不可用于线上生产环境。

可以使用-t参数指定document_root，如果不指定表示使用当前目录作为document_root

```
php -S 0.0.0.0:9000 -t /data/webroot/
```

```
php -c
```
指定加载php.ini的绝对路径

```
php -c /data/xxx/my_php.ini
```
```
php -l
```
检测一个php代码文件是否有语法错误，如 php -l test.php

```
php -r
```
执行一段php代码，如

```
php -r "echo 'hello world';"
```

```
php --ini
```
显示当前加载的php.ini绝对路径

```
php --re swoole
```
显示某个扩展提供了哪些类和函数。

```
php --ri swoole
```
显示扩展的phpinfo信息。与phpinfo的作用相同，不同之处是这里仅显示指定扩展的phpinfo

```
php --rf file_get_contents
```
显示某个PHP函数的信息，一般用于检测函数是否存在
