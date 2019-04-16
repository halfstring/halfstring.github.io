---
title: 安装epel/remi
date: 2017-01-28 13:00
updated: 2017-01-28 13:00
tags:  linux epel remi php5.6
---

`今天是2017年的春节，在此恭祝大家鸡年大吉，万事如意 。`

### epel

``` shell
32位系统选择：
rpm -ivh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
64位系统选择：
rpm -ivh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
导入key：
rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6
```


centos 上yum安装
``` shell
yum install epel-release
```

### remi
``` shell
rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm﻿﻿
 
导入key：
rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-remi
```


开启默认php5.6
``` shell
开启remi-php56

vim /etc/yum.repo.d/remi.repo

[remi-php56]

enabled=0    改成： enabled=1
```

