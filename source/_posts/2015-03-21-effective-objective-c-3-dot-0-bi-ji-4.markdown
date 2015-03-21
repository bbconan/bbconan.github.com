---
layout: post
title: "Effective Objective-C 2.0 笔记3"
date: 2015-03-21 13:43
comments: true
categories: iOS
---
#三、接口和API设计
##①、处理一些从网络服务获得的数据时，对应的Model类中对外可见的属性要设为只读，只暴露需要暴露的属性。
##②、一种处理方式：在.h文件中，属性设置为readonly；.m文件中，对应的属性设置为readwrite。<br>
这样的话，就可以在内部进行读写数据操作。但是同时，KVC还是可以设置属性的。因为，即使没有在.h文件中，KVC还是会直接查找有没有setXXX方法。
##③、为可变集合提供方法，而不是把可变集合作为一个直接暴露属性。
##④、不要是用下划线作为方法前缀，因为下划线是系统保留字。
##⑤、为什么不提倡使用异常(Exception)?<br>
ARC下，异常不是默认安全的。如果在作用域中发生异常，那么后面因该被释放的对象不会被释放。同理，在非ARC下，如果异常在对象释放前发生，那么这个对象永远不会被释放。
##⑥、实现copying协议，需要实现单例方法- (id)copyWithZone:(NSZone *)zone<br>
> [NSMutableArray copy] => NSArray;<br>
 [NSArray mutableCopy] => NSMutableArray<br>
 
##⑦、NSCopying协议中的复制都是浅复制。

