---
layout: post
title: "Effective Objective-C 2.0 读书笔记1"
date: 2015-03-05 22:47
comments: true
categories:	iOS 
---
最近看了一遍[《Effective Objective-C 2.0:52 Specific Ways to Improve Your iOS and OS X Programs》英文版](http://book.douban.com/subject/21370593/),感觉受益匪浅，学到了很多以前不了解或者不清楚的知识点。因为英文略让人捉急的原因，所以打算再看一遍这本书，同时看的过程中也做一些笔记。<br>
#一、概况
##①、Objective-C对象都是在堆上申请的，而不是栈上。
##②、不在.h文件中导入，而是在.m文件中导入头文件。<br>
这样的原因是可以限制其他使用当前类的导入范围。例如：</br>
```objc
// EOCPerson.h
#import <Foundation/Foundation.h>

@class EOCEmployer;
//EOCPerson.m
#import “EOCPerson.h"
#import “EOCEmployer.h"
```
这样，其他需要导入EOCPerson.h的类，就不需要导入EOCEmployer.h文件了。同时，也可以尽量的避免互相引用。
##③、用类型常量代替＃define来定义常量，因为前者会带有类型信息。</br>
如果只在当前类中使用，只需在.m文件中定义static const AClass kVarible = a。如果需要暴露给其他类使用(例如，通知的名字),.h文件  extern NSString *const AStringConst;
.m文件 NSString *const AStringConst = @“a"
