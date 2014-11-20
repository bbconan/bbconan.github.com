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
###二、
```
text.boundingRectWithSize(CGSizeMake(w, CGFloat.max), options: NSStringDrawingOptions.TruncatesLastVisibleLine | NSStringDrawingOptions.UsesLineFragmentOrigin | NSStringDrawingOptions.UsesFontLeading, attributes: attribute, context: nil) 
```
&nbsp;&nbsp;&nbsp;&nbsp;这段正常的代码在iOS8.1、Xcode6.1下会报错。搜了一下发现[这个](http://stackoverflow.com/a/24065414/4049468)。意思是说当吧target设为OS X 10.10时，就正常。如果设为iOS8 SDK，就会警告异常。NSStringDrawingOptions被Swift作为enum:Int类型，而不是struct:RawOptionSet，这是Apple的bug。PS:我第一次向Apple报告iOS SDK的bug。<br>
&nbsp;&nbsp;&nbsp;&nbsp;同时，[这里](http://stackoverflow.com/a/25029448/4049468)给出了一个解决方法。就是在Objective-C中写好多个option的连接方法，然后在Swift中调用该方法。
```objc
+ (NSStringDrawingOptions)combine:(NSStringDrawingOptions)option1 with:(NSStringDrawingOptions)option2
{
    return option1 | option2;
}
```
```
let boundingRect = "string".boundingRectWithSize(size, options: StringDrawingOptions.combine(.UsesLineFragmentOrigin, with: .UsesFontLeading), attributes:nil, context:nil)
```


