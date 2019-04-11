---
title:  Keepalived
date: 2018-05-12 08:00
updated: 2018-05-12 08:00
---

<!-- toc -->

## 准备
### 网络环境及依赖vagrant

```
# 准备三台server
ip2 11.11.1.82  ka2
ip3 11.11.1.83  ka3
ip4 11.11.1.84  ka4

# 准备一枚vip: 11.11.1.8
```

```
Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.box = "centos67"

 config.vm.define "ka2" do |ka2|
    ka2.vm.network "private_network", ip: "11.11.1.82"
    ka2.vm.hostname="ka2"
    ka2.vm.box="centos67"
    ka2.vm.provider "virtualbox" do |v|
          v.customize ["modifyvm", :id, "--name", "ka2", "--memory", "2024"]
    end
  end

  config.vm.define "ka3" do |ka3|
    ka3.vm.network "private_network", ip: "11.11.1.83"
    ka3.vm.hostname="ka3"
    ka3.vm.box="centos67"
    ka3.vm.provider "virtualbox" do |v|
          v.customize ["modifyvm", :id, "--name", "ka3", "--memory", "2024"]
    end
  end
  config.vm.define "ka4" do |ka4|
    ka4.vm.network "private_network", ip: "11.11.1.84"
    ka4.vm.hostname="ka4"
    ka4.vm.box="centos67"
    ka4.vm.provider "virtualbox" do |v|
          v.customize ["modifyvm", :id, "--name", "ka4", "--memory", "2024"]
    end
  end
end
```

## 安装
三台server依次按照
- yum install keepalived openssl openssl-devel -y
- yum install nginx -y

## 修改nginx默认页面
- cd /usr/share/nginx/html

在 index.html body中填入ka2，ka3或ka4   用以区分当前请求了哪台机器的web server

## Keepaliced.conf

- vim /etc/keepalived/keepalived.conf

```
! Configuration File for keepalived

vrrp_script chk_nginx {
 script "/etc/keepalived/check_nginx.sh" # 判断nginx状态
 interval 2
 weight 2
}

global_defs {
   notification_email {
     acassen@firewall.loc
     failover@firewall.loc
     sysadmin@firewall.loc
   }
   notification_email_from Alexandre.Cassen@firewall.loc
   smtp_server 127.0.0.1
   smtp_connect_timeout 30
   router_id lvs_v1 #0
}

vrrp_instance VI_1 {
    state MASTER   #1
    interface eth1   #2
    virtual_router_id 51 #3
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        11.11.1.8 #4
    }
}
```

\#0 为当前节点名称设置；\#1 一台为MASTER其余两台为BACKUP \#2 当前使用网卡标识 \#3 三台机器相同 \#4 设置VIP

## 其他设置

- chkconfig nginx on && chkconfig keepalived on

## 启动 keepalived，nginx

