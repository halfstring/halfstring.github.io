---
title: 记一次浏览器refresh一次，url被莫名其妙请求两次的排查。
date: 2017-03-25 22:00
updated: 2017-03-25 22:00
---


`背景：公司项目中我负责封装了几个方法，因为牵扯到钱，而且牵扯到数张表，于是写段程序“假装”调用一下。
发现在chrome中一次refresh某数据表竟然被莫名插入了两条记录。`

`排查开始了，index.php首行， error_log($_SERVER, 3, '/tmp/req.log');`

LOG:
```log
array (
  'REDIRECT_TESTPROJECTENV' => 'dev',
  'REDIRECT_STATUS' => '200',
  'TESTPROJECTENV' => 'dev',
  'HTTP_HOST' => 'www.test-project.com',
  'HTTP_ACCEPT' => 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
  'HTTP_UPGRADE_INSECURE_REQUESTS' => '1',
  'HTTP_COOKIE' => 'PHPSESSID=j62or4lffl3o7h1bo9t0ejtfe4',
  'HTTP_USER_AGENT' => 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, like Gecko) Version/10.0.3 Safari/602.4.8',
  'HTTP_ACCEPT_LANGUAGE' => 'zh-cn',
  'HTTP_ACCEPT_ENCODING' => 'gzip, deflate',
  'HTTP_CONNECTION' => 'keep-alive',
  'PATH' => '/sbin:/usr/sbin:/bin:/usr/bin',
  'SERVER_SIGNATURE' => '<address>Apache/2.2.15 (CentOS) Server at www.test-project.com Port 80</address>
',
  'SERVER_SOFTWARE' => 'Apache/2.2.15 (CentOS)',
  'SERVER_NAME' => 'www.test-project.com',
  'SERVER_ADDR' => '11.11.1.28',
  'SERVER_PORT' => '80',
  'REMOTE_ADDR' => '11.11.1.1',
  'DOCUMENT_ROOT' => '/data/testproject',
  'SERVER_ADMIN' => 'root@localhost',
  'SCRIPT_FILENAME' => '/data/testproject/index.php',
  'REMOTE_PORT' => '49489',
  'REDIRECT_URL' => '/admin/go',
  'GATEWAY_INTERFACE' => 'CGI/1.1',
  'SERVER_PROTOCOL' => 'HTTP/1.1',
  'GATEWAY_INTERFACE' => 'CGI/1.1',
  'SERVER_PROTOCOL' => 'HTTP/1.1',
  'REQUEST_METHOD' => 'GET',
  'QUERY_STRING' => '',
  'REQUEST_URI' => '/admin/go',
  'SCRIPT_NAME' => '/index.php',
  'PHP_SELF' => '/index.php',
  'REQUEST_TIME_FLOAT' => 1490317972.477,
  'REQUEST_TIME' => 1490317972,
)
```


```log
array (
  'REDIRECT_TESTPROJECTENV' => 'dev',
  'REDIRECT_STATUS' => '200',
  'TESTPROJECTENV' => 'dev',
  'HTTP_HOST' => 'www.test-project.com',
  'HTTP_CONNECTION' => 'keep-alive',
  'HTTP_ACCEPT' => 'text/plain',
  'HTTP_USER_AGENT' => 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36',
  'HTTP_ACCEPT_ENCODING' => 'gzip, deflate, sdch',
  'HTTP_ACCEPT_LANGUAGE' => 'zh-CN,zh;q=0.8,en;q=0.6',
  'HTTP_COOKIE' => 'OUTFOX_SEARCH_USER_ID_NCOO=106214989.24896166; PHPSESSID=n71nlpbanjnh0ehe4bl5robeb7',
  'PATH' => '/sbin:/usr/sbin:/bin:/usr/bin',
  'SERVER_SIGNATURE' => '<address>Apache/2.2.15 (CentOS) Server at www.test-project.com Port 80</address>
',
  'SERVER_SOFTWARE' => 'Apache/2.2.15 (CentOS)',
  'SERVER_NAME' => 'www.test-project.com',
  'SERVER_ADDR' => '11.11.1.28',
  'SERVER_PORT' => '80',
  'REMOTE_ADDR' => '11.11.1.1',
  'DOCUMENT_ROOT' => '/data/testproject',
  'SERVER_ADMIN' => 'root@localhost',
  'SCRIPT_FILENAME' => '/data/testproject/index.php',
  'REMOTE_PORT' => '49425',
  'REDIRECT_URL' => '/admin/go',
  'GATEWAY_INTERFACE' => 'CGI/1.1',
  'SERVER_PROTOCOL' => 'HTTP/1.1',
  'EQUEST_METHOD' => 'GET',
  'QUERY_STRING' => '',
  'REQUEST_URI' => '/admin/go',
  'SCRIPT_NAME' => '/index.php',
  'PHP_SELF' => '/index.php',
  'REQUEST_TIME_FLOAT' => 1490317708.1760001,
  'REQUEST_TIME' => 1490317708,
)
```


把环境切换成nginx + php-fpm

```nginx.log
11.11.1.1 - - [24/Mar/2017:10:09:53 +0800] "GET /admin/go HTTP/1.1" 200 248 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36" "-"
11.11.1.1 - - [24/Mar/2017:10:09:53 +0800] "GET /admin/go HTTP/1.1" 200 248 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36" "-"
```

显然不是Web service的问题。
那就是浏览器的鬼了。
操作系统中安装了safari, chrome。99.99%时候使用chrome，所以safari特别的“干净”，火速切换到safari执行一次。
结果很是开心，safari中竟然正常的请求了一次。

`锁定了chrome！！！`

chrome自带的开发者工具 network中显示chrome并未请求两次。
好吧，给chrome安装了fiddler插件。。。结果跟开发者工具一样，并无异常。


放大招，google！果然，网上很多网友跟我一样被请求了两次，但是大多是因为favicon.icon.
在一篇帖子中有评论质疑是不是插件的问题。

果断禁掉所有的chrome插件。

对，问题不存在了。请求正常了！！！！
挨个试过来，发现好几款的插件启用后都会使请求再一次发生。 一位网友讲某些插件会让http_headers再一次请求。
'HTTP_ACCEPT' => 'text/plain'
而chrome开发者工具仅仅是对浏览器当前的正常访问做了处理，至于插件对当前访问做了什么操作，并未纳入监控。 
问题查至此暂且终结，留下一个“萝卜坑”，等有时间研究一下chrome的插件开发，再细细“拷问”一下为嘛二次请求。

THX...
