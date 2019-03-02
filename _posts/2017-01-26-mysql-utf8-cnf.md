---
title: mysql编码默认utf8
date: 2017-01-26
updated: 2017-01-26
tags: mysql cnf utf8
---


```shell
[mysqld]
port = 3306
socket = /var/lib/mysql/mysql.sock
character-set-server=utf8
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
user=mysql
symbolic-links=0
[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

[client]
port = 3306
socket = /var/lib/mysql/mysql.sock
default-character-set=utf8
[mysql]
no-auto-rehash
default-character-set=utf8
```
