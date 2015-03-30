---
layout: post
title: "Effective Objective-C 2.0 笔记5"
date: 2015-03-30 21:41
comments: true
categories: iOS
---
#五、内存管理
##①、当一个对象调用autorelease时，它就会在稍后自行释放，通常是在2个事件loop之间。
##②、在ARC下，仍然需要手动清理非Objective-C对象。例如：CoreFoundation对象或者堆上malloc申请的对象。
##③、不需要调用[super dealloc]。<br>
在生成和运行.cxx_destruct方法的过程中，系统自动调用了super dealloc。所以在ARC下，delloc方法可能是这个样子：
```objc
 - (void)dealloc {
 	CFRelease(_coreFoundationObject);
 	free(_heapAllocatedMemoryBlob);
 }
```
##④、在ARC下，系统对于retain、release、autorelease方法进行了优化，使它们不进行Objective-C的消息分发。
##⑤、在ARC下，使用try catch是很危险的行为。<br>
因为ARC不会对异常的代码进行内存管理。除非增加大量样板代码来处理这个情况，但是这样没有异常发生又会降低运行时的性能，同时也会显著的增加应用的代码量。
##⑥、一个对象引用它不拥有的对象的例子：代理模式。
##⑦、autoreleas pool可以嵌套。当新增一个autorelease对象时，是加到最内层的。
##⑧、autorelease pool是在栈中申请的，当对一个对象发送autorelease消息时，它就被加到autorelease栈的栈顶。
##⑨、不用对NSString、NSNumber进行retainCount操作。<br>
首先，看个例子：
```objc
NSString *string = @"Some string";
NSLog(@"string retainCount = %lu",[string retainCount]);
```
```objc
string retainCount = 18446744073709551615
```
这个数字是2^64-1。因为NSString是单例对象。string是一个编译时常量。这种情况下，编译器会制作一个特殊对象，在应用的二进制文件中替换NSString的数据。<br>
NSNumber也是类似。用标签指针(tagged pointer)来区分各种类型的数值。在这种模式下，没有NSNumber对象。指针本身的值包含所有关于数字的消息。运行时在消息分发过程中发现一个标签指针，它会装作像一个NSNumber对象一样来操作指针的数值。
