---
layout: post
title: "Swift学习笔记"
date: 2014-11-16 23:25
comments: true
categories: Swift
---
###一、
&nbsp;&nbsp;&nbsp;&nbsp;在Objective-C的
```objc 
[self performSelector:@selector(updateStatus)   onThread:[NSThread mainThread] withObject:nil waitUntilDone:NO];
```
在Swift中是没有performSelector方法的。替代方法有2种
####1、设置Timer定时器
```
NSTimer.scheduledTimerWithTimeInterval(0.1, target: self, selector: Selector("updateStatus"), userInfo: nil, repeats: false)
```
####2、利用GCD定时器
```
dispatch_after(1, dispatch_get_main_queue(), {
    // your function here
})
```

