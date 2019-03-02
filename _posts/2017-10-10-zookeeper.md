---
title:  ZooKeeper
date: 2017-10-10 08:00
updated: 2017-10-10 08:00
---

<!-- toc -->

## 准备
### 网络环境及依赖
- CentOS 6.8
- repo: Base.repo; epel.repo; remi.repo
- yum update -y
- yum install git wget gcc -y
- [Zookeeper 3.4.10](https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/zookeeper-3.4.10/zookeeper-3.4.10.tar.gz)

### 准备三台虚拟机
> 11.11.1.61  zk1
> 11.11.1.62  zk2
> 11.11.1.63  zk3

## 安装

### 顺序执行
- ssh root@zk1
- cd /usr/local/src/
- wget https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/zookeeper-3.4.10/zookeeper-3.4.10.tar.gz
- tar xzvf zookeeper-3.4.10.tar.gz
- cd zookeeper-3.4.10/conf
- cp zoo_sample.cfg zoo.cfg 

### 修改zoo.cfg

文件尾追加
``` php 
server.1=11.11.1.61:2888:3888
server.2=11.11.1.62:2888:3888
server.3=11.11.1.63:2888:3888
```

- mkdir -p /usr/loca/src/zookeeper-3.4.10/data/
- mkdir -p /usr/loca/src/zookeeper-3.4.10/logs/
- chown -R root.root data
- chown -R root.root logs

修改 zoo.cfg
```
dataDir=/usr/local/src/zookeeper-3.4.10/data
dataLogDir=/usr/local/src/zookeeper-3.4.10/logs
```

### 复制至zk2, zk3

- cd /usr/local/src/
- scp -r zookeeper-3.4.10  root@zk2:/usr/local/src
- scp -r zookeeper-3.4.10  root@zk3:/usr/local/src

## 创建myid

ssh root@zk1
- echo "1">/usr/local/src/zookeeper-3.4.10/data/myid

ssh root@zk2
- echo "2">/usr/local/src/zookeeper-3.4.10/data/myid

ssh root@zk3
- echo "3">/usr/local/src/zookeeper-3.4.10/data/myid

## 启动
依次登录zk1, zk2, zk3执行如下：

- cd /usr/local/src/zookeeper-3.4.10
- bin/zkServer.sh start

## test

- bin/zkServer.sh status ## 查看状态
- bin/zkCli.sh  ## 客户端连接
