---
title: iterm2/git 设置代理
date: 2017-01-27 10:00
updated: 2017-01-27
tags: proxy iterm2 git
---

### iterm2
vim ~/.bashrc  或者 vim ~/.zshrc
增加如下两句
``` shell
alias enproxy="export http_proxy=socks5://127.0.0.1:1080 https_proxy=socks5://127.0.0.1:1080 ALL_PROXY=socks5://127.0.0.1:1080"
alias disproxy="unset http_proxy https_proxy ALL_PROXY"
```

开启代理
``` shell
enproxy
```

关闭代理
``` shell
disproxy
```
### GIT
设置http代理：
```shell
git config –global https.proxy http://127.0.0.1:8118
git config –global https.proxy https://127.0.0.1:8118
```

取消http代理：
``` shell
git config –global –unset http.proxy
git config –global –unset https.proxy
```

备注：git是可以直接支持socks5代理的，如果只安装了shadowsocks，而不安Privoxy设置方法如下：
``` shell
git config –global http.proxy 'socks5://127.0.0.1:1080'
git config –global https.proxy 'socks5://127.0.0.1:1080'
```
