---
title: 统一开发环境神器：Vagrant
date: 2017-02-01  08:00
updated: 2017-02-01 08:00
tags: vagrant
---


<!-- toc -->

`本文不讨论为什么需要统一开发环境`

好多同学玩过VMware、VirtualBox等虚拟机，折腾过程中经常遇到这样那样的问题，各种搜索后发现版本不匹配或者怎样怎样。或者各种教程，各种设置。而Vagrant就是为了简化这一过程而生的。

很多场景下需要统一团队开发人员的开发环境，用以规避新人因开发环境复杂而上手慢，或者A开发者环境下正常B开发者环境出错，或者应对各种参数不对，模块未安装，某某依赖包版本低等问题

### Vagrant介绍
Vagrant基于虚拟机管理软件接口，使用ruby开发，让虚拟化过程配置文件化，保持轻量级等等优点

`快速开启多个虚拟机模拟分布式系统等场景下尤其的酷`


### Vagarnt 安装及使用
### 依赖安装
 Vagrant工作依赖虚拟机管理系统，建议安装VirtualBox
#### Vagrant 下载安装
 http://www.vagrantup.com/ 下载最新稳定版安装
#### 创建环境目录
```
/data/vagrant
```
#### 初始化环境目录
```
vagrant init
```

#### 添加box
```
# http://www.vagrantbox.es/ 查找合适的box
vagrant box add centos-base http://www.lyricalsoftware.com/downloads/centos65.box
# 下载速度太慢的话，可以用其他下载工具将box下载至本地   上面网络地址改成本地相对路径地址亦可
```

#### 查看已添加box
```
vagrant box list
```

#### 修改Vagrantfile
```
vim /data/vagrant/Vagrantfile
```


#### Vagrantfile实例
```
Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.define :dev do |dev|
    dev.vm.provider "virtualbox" do |v|
          v.customize ["modifyvm", :id, "--name", "dev", "--memory", "1024"]
    end
    dev.vm.hostname = "dev"
    dev.vm.box = "centos-base"
  end
end
```


#### Vagrantfile模拟多台机器
```
Vagrant.configure("2") do |config|

  config.ssh.insert_key = false
  config.vm.define "node11" do |node11|
    node11.vm.box = "centos66"
    node11.vm.network "private_network", ip: "11.11.1.11"
    node11.vm.hostname="node11"
    node11.vm.provider "virtualbox" do |v|
          v.customize ["modifyvm", :id, "--name", "node11", "--memory", "1024"]
    end

    node11.vm.network :forwarded_port, guest: 22, host: 2211
  end

  config.vm.define "node12" do |node12|
    node12.vm.box = "centos66"
    node12.vm.network "private_network", ip: "11.11.1.12"
    node12.vm.hostname="node12"
    node12.vm.provider "virtualbox" do |v|
          v.customize ["modifyvm", :id, "--name", "node12", "--memory", "1024"]
    end

    node12.vm.network :forwarded_port, guest: 22, host: 2212
  end
end
```

### 其他
```
vagrant up # 启动虚拟机
vagrant up dev # 启动指定虚拟机

vagrant halt ## 关闭虚拟机
vagrant reload # 重启虚拟机, 一般用以载入新的配置文件
vagrant status # 查看虚拟机状态
vagrant ssh dev # ssh 连接虚拟机或者
# 虚拟机启动时默认开启ssh服务，会将22端口默认映射成2222（具体查看启动信息）
# 也可以用putty等工具连接 vagrant@127.0.0.1:2222, 默认密码：vagrant

vagrant package --output  centos.xxx.box #  导出box
```




