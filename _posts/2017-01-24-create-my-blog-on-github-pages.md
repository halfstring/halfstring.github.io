---
title: 把博客建立在 github pages 上
date: 2017-01-24
updated: 2017-01-24
tags: Blog github VPS Hexo npm  Markdown CNAME
---

`世上最难得的是专注，最好的工具是让人免于繁琐及旁念得以专注。`

相信很多博主，都用过某园，某浪等博客系统，或者土豪的VPS搭建WP等方式开博，都会有种种限制，现在提供一种，不需要“动态"的系统管理，又符合“多快好省”的原则搭建起来的系统，还可以自定义域名。

倘若系统啊，功能啊，管理啊交由别人来管理，我们只管写文章岂不美哉

好，废话少说，直奔主题。

### 写在前面的话
- 假设各位已经安装了npm，也假设你已经知道 `https://npm.taobao.org/`是干啥的。
- 假设你已经安装并了解git, 假设你知道github。
- 如果需要配置自定义域名，假设你已经知道了域名CNAME记录是什么。
- 我的域名是halfstring.com。

### Hexo Install

- Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页. 其他工具框架自行比对，不在本次讨论范畴。

以下命令依次执行

``` php 
npm install -g hexo-cli
npm install hexo --save
npm install hexo-deployer-git --save
npm install hexo-renderer-less --save
npm install hexo-renderer-jade --save
mkdir -p /data/githubpages/
cd /data/githubpages/
hexo init
npm install
hexo g 
hexo s
```
然后访问 `http://localhost:4000`预览默认生成站点

### github repo: halfstring.github.io
* 注册github账号
* 创建public 仓库："halfstring.github.io", 不含双引号；

修改配置文件
``` shell
vim /data/githubpages/_config.yml
```
修改站点名称，语言，描述等参数。
配置文件中 `deploy` 修改成如下(参数修改成自己对应的参数)：

```
deploy:
  type: git
  repo: git@github.com:halfstring/halfstring.github.io.git
  branch: master
```
- 上面配置部署github创建的仓库信息

### 部署测试
``` shell
hexo g --d 
```
- 上面命令意思是，文件生成后部署至线上。
- 命令执行完毕，访问 `http://halfstring.github.io/`

### 域名
`该部分为了满足自定义域名的需求，不需要的可绕过这一部分。`

以下操作配置自定义域名
```SHELL
echo "www.halfstring.com">>/data/githubpages/source/CNAME
```

``` SHELL
hexo clean ;
hexo g --d
```

登录域名注册商提供的域名管理后台，解析一条CNAME记录

- www.halfstring.com ---CNAME---> halfstring.github.io

解析生效后访问 `http://www.halfstring.com/`

如果没出意外的话，大功告成。

### Hexo theme及常用命令

说到这里，可能你会对hexo默认的主题不太满意，正如聪明的你想的那样，hexo提供了很多的主题。访问 `http://hexo.io/themes/`

选择中意的主题，点主题名字获取主题的github仓库地址。

假设中意`Flexy` 这套主题，github地址为：`https://github.com/sjaakvandenberg/flexy`


使用主题方法如下：
``` shell
git clone https://github.com/sjaakvandenberg/flexy /data/githubpages/themes/flexy
```

``` shell
vim /data/githubpages/_config.yml
找到 `theme:`  改成 `theme: flexy`
```
注意配置文件所有的键和值中间 冒号后面有个`空格`。。。。。`坑`


执行以下命令重建blog
``` shell
hexo clean;
hexo g --d
```

如果不出意外的话，访问 `http://www.halfstring.com/` 应该已经应用了新的主题。

### 发表文章
``` SHELL
vim /data/githubpages/source/_posts/create-my-blog-on-githubpage.md
```

以下写入上面md文件

``` SHELL
---
title: 把博客建立在 github pages 上
---
### 写在前面的话
- 假设各位已经安装了npm，也假设你已经知道 `https://npm.taobao.org/`是干啥的。
- 假设你已经安装并了解git, 假设你知道github。
- 如果需要配置自定义域名，假设你已经知道了域名CNAME记录是什么。
- 我的域名是halfstring.com。

```


``` SHELL
hexo g --d
```
***小技巧***
- 打开两个终端， 都切到 `/data/githubpages/` 目录
- 一个执行命令 `hexo g --w`; 另一个执行 `hexo s `
- 然后打开喜欢的MD编辑器，同时打开浏览器`http://localhost:4000`， MD文件更新，浏览器自动刷新。 

### 彩蛋
- 推荐一款七牛图床小众软件: U图床  [https://itunes.apple.com/cn/app/u-tu-chuang-tu-chuang-shen/id1135023555?mt=12](U图床) 尽管目前仅仅支持七牛图床，但是依然酷酷的，支持截图上传，例如QQ截图~

### 写在后面的话

- /data/githubpages/source/CNAME  我踩过的`坑`
- 如果配置的deploy 出现权限错误，请自行查找github  ssh key导入
- 我在mac上虽然亲测，但是难免有一些问题。欢迎留言。

### 在此感谢伟大的开源社区。

