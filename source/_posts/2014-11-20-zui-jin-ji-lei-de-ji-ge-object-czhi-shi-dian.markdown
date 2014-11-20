---
layout: post
title: "最近积累的几个Objective-C知识点"
date: 2014-11-20 21:15
comments: true
categories: iOS
---
###1、__unused修饰符
&nbsp;&nbsp;&nbsp;&nbsp;\_\_unused宏（事实上是__attribute__((unused))这个GCC的编译器属性，更多请参考[mattt大神的博客](http://nshipster.com/__attribute__/)）告诉编译器“如果我没用到这个变量，不要警告我”。
###2、在Darwin层建立Notification监听的方法
&nbsp;&nbsp;&nbsp;&nbsp;在锁屏和解锁的时候，iOS系统会发送通知，经过搜索，得知大概有下面3种通知：<br>
(1)、com.apple.iokit.hid.displayStatus<br>
锁屏后通知会发出消息，在屏幕变亮后，没有滑动解锁，系统也会发出该通知。<br>
(2)、com.apple.springboard.lockstate<br>
在系统锁屏和滑动解锁后，会发出该通知<br>
(3)、com.apple.springboard.lockcomplete
锁屏后，发出该通知。这个通知总会在(2)通知后发出。<br>

判断屏幕是否有锁的方法<br>
```objc

    CFNotificationCenterAddObserver(CFNotificationCenterGetDarwinNotifyCenter(), NULL, displayStatusChanged, CFSTR("com.apple.iokit.hid.displayStatus"), NULL, CFNotificationSuspensionBehaviorDeliverImmediately);
```
```objc
static void displayStatusChanged(CFNotificationCenterRef center, void *observer, CFStringRef name, const void *object, CFDictionaryRef userInfo) {
    NSLog(@"event received!");
    uint64_t state;
    int token;
    notify_register_check("com.apple.iokit.hid.displayStatus", &token);
    notify_get_state(token, &state);
    notify_cancel(token);
}
```
###3、宏定义 
```objc
 #define nil __DARWIN_NULL
```
nil用于表示空的实例对象<br>

```objc
#define Nil __DARWIN_NULL
```
Nil用于表示空类对象


