---
layout: post
title: "最近积累的几个Objective-C知识点"
date: 2014-11-20 21:15
comments: true
categories: iOS
---
###1、__unused修饰符
&nbsp;&nbsp;&nbsp;&nbsp;\_\_unused宏（事实上是__attribute__((unused))这个GCC的编译器属性，更多请参考[mattt大神的博客](http://nshipster.com/__attribute__/)）告诉编译器“如果我没用到这个变量，不要警告我”。
###2、三元表达式的简写
?:是C中唯一一个三目运算符，用来替代简单的if-else语句，同时也是可以两元使用：
```objc
NSString *string = inputString ?: @"default"; 
NSString *string = inputString ? inputString : @"default"; // 等价
```
###3、如果block的参数列表为空的话，相当于可变参数（不是void）。
###4、@compatibility_alias:允许现有类有不同的名称做别名。比如PSTCollectionView使用@compatibility_alias来显著提高对UICollectionView的向后兼容的直接替换的使用体验。(具体可参考NSHipster的文章)
```objc
 // 允许代码使用UICollectionView如同它可以在iOS SDK 5使用一样。
 // http://developer.apple.com/legacy/mac/library/#documentation/DeveloperTools/gcc-3.3/gcc/compatibility_005falias.html
 #if __IPHONE_OS_VERSION_MAX_ALLOWED < 60000
 @compatibility_alias UICollectionViewController PSTCollectionViewController; 
 @compatibility_alias UICollectionView PSTCollectionView;
 @compatibility_alias UICollectionReusableView PSTCollectionReusableView;
 @compatibility_alias UICollectionViewCell PSTCollectionViewCell; 
 @compatibility_alias UICollectionViewLayout PSTCollectionViewLayout;
 @compatibility_alias UICollectionViewFlowLayout PSTCollectionViewFlowLayout; 
 @compatibility_alias UICollectionViewLayoutAttributes PSTCollectionViewLayoutAttributes;
 @protocol UICollectionViewDataSource <PSTCollectionViewDataSource> @end
 @protocol UICollectionViewDelegate <PSTCollectionViewDelegate> @end 
 #endif
```
###5、__attribute__((constructor))
&nbsp;&nbsp;&nbsp;&nbsp;若函数被设定为constructor属性，则该函数会在main（）函数执行之前被自动的执行
###6、看到QQ群里讨论，GCD最多能创建多少线程
&nbsp;&nbsp;&nbsp;&nbsp;66＝64（GCD线程池的最大值） + 主线程 + 其他随机非GCD线程。[来源](http://stackoverflow.com/questions/7213845/number-of-threads-created-by-gcd)
###7、在Darwin层建立Notification监听的方法
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


