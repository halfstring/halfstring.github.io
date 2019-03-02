---
title: Dash(docset) for Swoole
date: 2017-03-30 08:00
updated: 2017-03-30 08:00
---

`愿你走出半生，归来仍是少年。`

`"Alfred + Dash" 一直在用这个神器组合。发现竟然没有swoole的手册。决定手动打造一枚，自己使用，也希望能给大家带来方便. `

### html来源

没在swoole官网找到html包下载的地方。但是找到了swoole-chm的github地址。
https://github.com/smalleyes/swoole-chm 下载 `swoole.chm`至本地。

安装chmlib， 把chm转成html。安装方法在此略过~~

`extract_chmLib ./swoole.chm /data/dash/swoole-html/`

```
/data/download/>  extract_chmLib ./swoole.chm /data/dash/swoole-html
./swoole.chm:
--> /#IDXHDR
--> /#IVB
--> /#STRINGS
--> /#SYSTEM
--> /#TOPICS
--> /#URLSTR
--> /#URLTBL
--> /#WINDOWS
--> /$FIftiMain
--> /$OBJINST
--> /$WWAssociativeLinks/Property
--> /$WWKeywordLinks/BTree
--> /$WWKeywordLinks/Data
--> /$WWKeywordLinks/Map
--> /$WWKeywordLinks/Property
...
```

### 给html文件找个新家。

```
mkdir -p /data/dash/swoole/swoole.docset/Contents/Resources/Documents/
cd /data/dash/swoole-html;
cp -R * /data/dash/swoole/swoole.docset/Contents/Resources/Documents/

```

### 创建Info.plist信息，欠swoole.docset一个描述~~
```
vim /data/dash/swoole/swoole.docset/Conttents/Info.plist
内容：
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>dashIndexFilePath</key>
    <string>index.htm</string>
    <key>CFBundleIdentifier</key>
    <string>swoole</string>
    <key>CFBundleName</key>
    <string>swoole</string>
    <key>DocSetPlatformFamily</key>
    <string>swoole</string>
    <key>isDashDocset</key>
    <true/>
</dict>
</plist>
```

其中index.htm是html文档包中的入口html文件。

### 创建索引

mac自带sqlite3，

```
> cd /data/dash/swoole.docset/Contents/Resources/;
> sqlite3 docSet.dsidx
> CREATE TABLE searchIndex(id INTEGER PRIMARY KEY, name TEXT, type TEXT, path TEXT); 
> .exit
```

sqlite3文件是用来存放关键字等索引的。在Dash中检索的时候就是依据这些关键字快速定位到单个html文件的。

我注意到html包中有两个特殊文件，Swoole.hhc, Swoole.hhk。一个是层级关系索引列表，一个扁平等级索引列表。
正是我们需要的。拖到单文本编辑器中，手动整理。不难整理成如下格式：

```
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::__construct','Keyword','server/functions/construct.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::set','Keyword','server/functions/set.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::on','Keyword','server/functions/on.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::addListener','Keyword','server/functions/addListener.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::addProcess','Keyword','server/functions/addProcess.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::listen','Keyword','server/functions/listen.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::start','Keyword','server/functions/start.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::reload','Keyword','server/functions/reload.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::stop','Keyword','server/functions/stop.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::shutdown','Keyword','server/functions/shutdown.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::tick','Keyword','server/functions/tick.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::after','Keyword','server/functions/after.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::defer','Keyword','server/functions/defer.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::clearTimer','Keyword','server/functions/clearTimer.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::close','Keyword','server/functions/close.htm');
INSERT INTO searchIndex(name, type, path) VALUES ('swoole_server::send','Keyword','server/functions/send.htm');
...
```

将上述SQL内容保存至`/data/dash/swoole/swoole.docset/Contents/Resources/abc.sql`中。

```
> cd /data/dash/swoole/swoole.docset/Contents/Resources;
> sqlite3 docSet.dsidx  
> .rend abc.sql
```

上述命令将SQL导入到searchIndex表中后索引就创建好了
swoole.docset也创建成功了。

```
> open /data/dash/swoole/
```

![双击`swoole.docset`](http://ok73hb95u.bkt.clouddn.com/20170330149081048312651.png)

双击 `swoole.docset`

不出意外导入Dash成功。
让我们来看看牛逼的效果吧~~


![demo](https://github.com/halfstring/swoole-docset/blob/master/demo.gif?raw=true)

![dash-demo](https://github.com/halfstring/swoole-docset/blob/master/dash-demo.png?raw=true)


https://github.com/halfstring/swoole-chinese-docset   点这里下载吧~




