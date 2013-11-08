---
layout: post
title: "[UIDeviceRGBColor CGColor]: message sent to deallocated instance xxx"
date: 2013-11-08 14:48
comments: true
categories: iOS
---
&nbsp;&nbsp;&nbsp;&nbsp;近期开发过程中，遇到一个“[UIDeviceRGBColor CGColor]: message sent to deallocated instance xxx”的bug，google了很长时间也没找到原因所在。后来，发现<a href="http://www.91r.net/ask/11318138.html"> 点击此处</a>，这里面所遇到的bug和我的类似。其中，这段话</p>
When setting buttonColor = bc without retaining, buttonColor will become a dangling pointer when the current autorelease pool flushes (assuming it's not retained elsewhere).

[self setNeedsDisplay] will invoke drawRect: later and at that point, buttonColor may already have been deallocated which will crash your app when referring to it.</p>
说明问题所在:当设置完color后，是自动释放的（autorelease）。下次，调用 [self setNeedsDisplay]后，调用drawRect 方法时，里面的color已经被释放了。所以我的解决办法是在设置 self.color = color 后，紧接着
[self.color retain]。

参考地址：<http://www.91r.net/ask/11318138.html>