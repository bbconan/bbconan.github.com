---
layout: post
title: "CocoaHTTPServer出现Crash的解决方法"
date: 2013-08-26 17:33
comments: true
categories: iOS
---

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最近工程中，需要使用httpserver，就用了CocoaHTTPServer。代码的整合非常容易。但是运行后用浏览器连接服务器时，程序会 crash。<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;最后发现是在 HTTPConnection::filePathForURI 函数中 config 的 documentroot 是空的，调试好久。然后，通过研究和搜索，发现最新版的CocoaHTTPServer用的是ARC。解决办法就是：选择项目 target，Build Phases -> Compile Sources 中，给所有 CocoaHTTPServer 的代码添加 Compiler Flags ： -fobjc-arc 即可。
{%img /images/cocoahttpserver_crash.png %}
